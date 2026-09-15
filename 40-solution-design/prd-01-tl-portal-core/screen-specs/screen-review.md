---
title: "Screen Spec — Review"
status: draft
created: 2026-09-15
updated: 2026-09-15
tags: [screen-spec, ux]
---

# Screen Spec — Review

**Module / PRD:** `prd-01-tl-portal-core` — Phase 1: Bookkeeping & Visibility
**Purpose:** Where Rohan and Sharva see everything routed to them for review, how long each item has been waiting, and record a decision — the direct fix for the stall in `10-observations/observed-review-stall.md`, where nothing surfaced the problem until a manual nudge.

## Entry Points

| From (screen / source) | Trigger | Condition / context passed in |
|---|---|---|
| Portal nav | "Review" nav item | Lands on the list, filtered to items routed to the logged-in reviewer (see Business Rules) |

No other entry points — per the PRD's Non-Goals, this project has no Slack integration in Phase 1, so there's no deep-link-from-Slack path the earlier version of this project had.

## UX Layout

Single flat list (not master-detail — there's no in-portal draft view or comment thread to show alongside a selection; the actual draft review happens in Drive, per Non-Goals). Sorted oldest-first, so the longest-waiting item is always at the top.

Each row:
- Title, Pillar, Urgency tags
- Staleness badge — "Waiting N days," amber once past that item's own Urgency-based SLA (REQ-009): Filler same-day, High 72h, Standard 2 business days
- "Open Draft" link — opens `production-draft-<slug>.md` directly in Drive, in a new tab. This is where the actual review and any commenting happens (Drive comments, unchanged from today)
- Decision controls: three buttons (Approved / Changes Requested / Killed) plus an optional single-line Notes field

## Data Displayed

| Label | Value / Format | Source (entity.field / API) |
|---|---|---|
| Title | Text | Production Item — Tracker row `Title` |
| Pillar | Tag | Production Item — Tracker row `Pillar` |
| Urgency | Tag (High/Medium/Low/Filler) | Production Item — Tracker row `Urgency` |
| Waiting time / staleness badge | "Waiting N days," amber past that item's SLA threshold | Derived: today − `Overall Status` last-changed timestamp, compared against the Urgency-specific threshold |
| Draft link | Link | Production Item — `Production/production-draft-<slug>.md`, via the Tracker's stored Drive link |
| Reviewer routing | Which of Rohan/Sharva this item is routed to (informational, for a shared view if one is ever built) | Derived: Rohan always; + Sharva if Pillar includes engineering/white-paper |

## CTAs

| Element | Type (button/link/action) | Behavior (what happens / where it goes) |
|---|---|---|
| "Open Draft" | Link | Opens the item's `production-draft-<slug>.md` in Drive, new tab |
| Notes field | Text input (single line) | Optional; if filled, its content is written to the Tracker row's `Notes` column alongside the decision |
| "Approved" | Button | Sets `Overall Status = Approved` immediately (first reviewer to act wins on dual-routed items — see Business Rules); triggers Calendar row creation (REQ-011); item leaves the list |
| "Changes Requested" | Button | Sets `Overall Status = Changes Requested`; Notes optional; item returns to the Production Board (REQ-010); item leaves this reviewer's list |
| "Killed" | Button | Sets `Overall Status = Killed`; Notes optional; row stays in the Tracker (never deleted); item leaves the list |

## Validations

| Field / Action | Rule | Error message |
|---|---|---|
| Decision buttons | Only visible for users routed as a reviewer on this item (Rohan always; Sharva only on engineering/white-paper Pillar) | Non-routed users (including Mahima) don't see this screen's items at all — not just disabled buttons |
| Notes field | No validation — optional on all three outcomes (decided 2026-09-15) | — |
| Dual-reviewer race | If both Rohan and Sharva act on the same eng/white-paper item, whichever decision is recorded first wins (decided 2026-09-15); the second reviewer's action, if attempted after the fact, is rejected | "Already decided: <outcome>, recorded by <reviewer>" shown if the second reviewer tries to act |

## Conditional States

| State | What the user sees |
|---|---|
| Empty | "Nothing waiting on your review" |
| Loading | Skeleton rows while the Tracker sheet loads fresh (no caching, per concurrency rule) |
| Error | "Couldn't load the Tracker" with a retry action, if the Sheets read fails |
| Stale item (past its Urgency's SLA) | Row and badge render amber (REQ-009) |
| Already decided (race) | See Validations — second reviewer on a dual-routed item sees the resolved outcome instead of active decision buttons |

## Open Questions

1. Should there be any confirmation step before a decision is recorded (e.g. "are you sure?" on Killed specifically, since it's the only outcome with no revision path back), or is a single click consistent with how quickly decisions are made today?
2. This screen shows only items currently routed for review. Should Rohan/Sharva have any way to see their own past decisions (an audit trail), or does that responsibility stay entirely with the Tracker sheet itself, which they can already open directly?
