---
title: "Screen Spec — Inbox"
status: draft
created: 2026-09-15
updated: 2026-09-15
tags: [screen-spec, ux]
---

# Screen Spec — Inbox

**Module / PRD:** `prd-01-tl-portal-core` — Phase 1: Bookkeeping & Visibility
**Purpose:** Where Mahima logs a raw input and creates an idea from it in one action — no separate raw-input holding state, since without Slack integration (out of scope this phase) nothing populates a queue for her to triage; by the time she's logging something she typically already knows what it is (REQ-001, REQ-002).

## Entry Points

| From (screen / source) | Trigger | Condition / context passed in |
|---|---|---|
| Portal nav | "Inbox" nav item | None |

## UX Layout

A single form, no list view:

- Title (required)
- Description — the full "what," free text
- Source — free text (e.g. "Sharva — Slack," "Meeting notes 2026-09-10")
- Submitted By — free text
- Pillar (required) — dropdown, 1–5 or Filler, multi-select for combos (per observed classification rules in `workflow-1-ingestion-screening.md`)
- Urgency (required) — High / Medium / Low
- Single button: **"Create & Screen"** — combines REQ-001 (create `Whats?` row + `idea-<slug>.md`) and REQ-002 (set Pillar/Urgency, `Screened = Yes`) into one action, since observed practice never leaves an idea deliberately unscreened once it's been classified

## Data Displayed

This screen has no data to display beyond the form itself — it's a creation form, not a list or dashboard.

## CTAs

| Element | Type (button/link/action) | Behavior (what happens / where it goes) |
|---|---|---|
| "Create & Screen" | Button | Writes `Whats?` row + `idea-<slug>.md`, sets `Screened = Yes`, auto-creates the Tracker row (REQ-002), redirects to the Production Board with the new item's card highlighted (per `screen-production-board.md` entry points) |

## Validations

| Field / Action | Rule | Error message |
|---|---|---|
| "Create & Screen" | Title, Pillar, and Urgency all required | "Fill in Title, Pillar, and Urgency before screening" (button disabled until all three are set) |

## Conditional States

| State | What the user sees |
|---|---|
| Default | Empty form, ready for input |
| Error | "Couldn't create this idea" with retry, if the Sheet/Drive write fails partway (e.g. `Whats?` row written but `idea-<slug>.md` creation fails, or vice versa) |
| Restricted access | Only the Producer role (Mahima, and the future internal-marketer role) sees this screen — Rohan/Sharva have no reason to log raw input, hidden from their nav entirely |

## Open Questions

1. Partial-failure handling — if the `Whats?` row write succeeds but the Drive file write fails (or vice versa), does the portal roll back the first write, retry the second, or leave a visibly inconsistent state for Mahima to notice and fix? Not yet decided.
2. This design deliberately doesn't address the observed gap in `workflow-1-ingestion-screening.md` (no record of raw input that was reviewed and rejected) — is that an acceptable trade-off for Phase 1, or worth a lighter-weight fix than the full two-step design that was set aside (e.g. Mahima just doesn't create an idea for it, with no record anywhere, same as today)?
