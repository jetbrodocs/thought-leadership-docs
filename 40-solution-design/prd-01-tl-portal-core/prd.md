---
title: "PRD — TL Portal, Phase 1: Bookkeeping & Visibility"
status: draft
created: 2026-09-11
updated: 2026-09-11
tags: [prd, solution-design]
---

# PRD — TL Portal, Phase 1: Bookkeeping & Visibility

> Precondition: tech stack decided in `30-analysis/tech-stack-decision.md` — lightweight Next.js portal reading/writing the existing Sheets/Drive live via the service account, no new database, Google OAuth, last-write-wins concurrency.

## Summary

A dashboard for Jetbro's thought-leadership content pipeline (company LinkedIn page only) that gives Mahima one place to see and drive the pipeline instead of hand-editing 3 Google Sheets separately, automates the mechanical handoffs between them, and makes review-stage staleness visible to reviewers instead of invisible — the gap that directly caused the 5-item stall documented in `10-observations/observed-review-stall.md`.

## Goals

- Give Mahima one dashboard reflecting `Whats?`, `Master Content Tracker`, and `Calender` live, instead of three separate sheet tabs.
- Automate every mechanical handoff from `20-process-maps/pipeline-overview.md`: Tracker row creation on `Screened = Yes` (step 4), status rollups (step 9), Calendar row creation on `Approved` (step 13).
- Make review-stage staleness visible on the screen Rohan and Sharva actually look at — directly targeting the stall in `observed-review-stall.md`, where nothing surfaced the problem to either of them until a manual nudge.
- Never become a second source of truth — Sheets/Drive stay authoritative and remain directly editable as a fallback at all times (per `30-analysis/tech-stack-decision.md`, Recommendation 6).

## Non-Goals

- **No AI-drafted content.** Mahima still writes the brief, master draft, and channel adaptations herself. (A separate future PRD, if pursued — see `CLAUDE.md`.)
- **No in-portal review comments.** Drive comments on `production-draft-<slug>.md` stay exactly as they are today (per "preserve what works" — no observed problem with Drive comments themselves, only with reviewers not acting at all). The portal links to the draft file; it doesn't reimplement commenting.
- **No Slack integration of any kind** — neither auto-ingesting raw ideas nor auto-posting review notifications. Not grounded in this pass's observations; a real candidate for a later phase once actually justified by evidence, not assumed.
- **Not Rohan's personal LinkedIn pipeline** — company page only, per `CLAUDE.md` scope.
- **Not the Sales/BD pipeline** — no MQL/lead-handoff features, per `CLAUDE.md` scope.
- **No new database** — per the tech-stack decision. Nothing here should require Postgres, SQLite, or similar.
- **No actual publishing integration** — no LinkedIn/X/Instagram/Substack API calls. "Live" in this pipeline still means a human posted manually on the platform and then flips the status in the portal.
- **No mobile app** — desktop web only, matches how the pipeline is used today.

## Users & Roles

| Role | What they do in this module |
|---|---|
| Mahima (Producer) | Logs raw input, decides "the what," classifies Pillar/Urgency, writes brief/draft/adaptations, submits for review, schedules approved items — Workflows 1 and 2 end to end |
| Rohan (Approver) | Default reviewer for all items — reviews via the linked draft file, records a decision in the portal |
| Sharva (Technical Reviewer) | Additional reviewer for engineering-pillar/white-paper items only, same review mechanism |
| (Future) internal marketer | Same permissions as Producer — role exists in the auth config from day one even though no second Producer exists yet (Marketing SOP §10 anticipates this hire) |

## Requirements

| ID | Requirement | Source (process doc / observation) | Acceptance criteria |
|---|---|---|---|
| REQ-001 | Inbox screen lets Mahima log a raw input and create an idea (`Whats?` row + `idea-<slug>.md`) from it | `pipeline-overview.md` steps 1–2 | Creating an idea writes both the Sheet row and the Drive markdown file in one action, visible immediately without a page reload |
| REQ-002 | Mahima can set Pillar + Urgency on an idea and mark it Screened | `pipeline-overview.md` step 3–4 | Setting `Screened = Yes` auto-creates the corresponding `Master Content Tracker` row with `Overall Status = In Production` — no manual copy step (this is Handoff 1) |
| REQ-003 | Production Board shows every in-flight item with per-channel readiness cells (`—`/`To Do`/`WIP`/`Ready`) and `Overall Status`, matching the Tracker | `shared-foundations.md` — status vocabulary | Board reflects the live Tracker sheet on load (last-write-wins, no caching beyond page load) |
| REQ-004 | Mahima can open an item's `production-draft-<slug>.md` sections (brief + per-channel drafts) and edit them from the portal | `pipeline-overview.md` steps 6–9 | Edits save back to the Drive markdown file; channel cell moves `To Do → WIP → Ready` as Mahima marks it |
| REQ-005 | Mahima can flag an item `On Hold` with a reason when a needed input (case-study data, technical core) is missing | `pipeline-overview.md` Exception A | Sets `Overall Status = On Hold`, records the reason in `Notes`; On Hold items are visibly flagged on the Production Board, not just a status string |
| REQ-006 | When all applicable channel cells reach `Ready`, `Overall Status` auto-rolls to `All Ready` | `pipeline-overview.md` step 9 | Rollup is automatic; no manual click needed for the status change itself |
| REQ-007 | Mahima can submit an `All Ready` item for review, which routes it to Rohan always and Sharva additionally if Pillar is engineering/white-paper | `pipeline-overview.md` step 10 | Sets `Overall Status = In Review`; routing is visible in the portal (who this item is waiting on) |
| REQ-008 | Review screen lets Rohan/Sharva see every item routed to them, open the linked `production-draft-<slug>.md` for the actual review, and record a decision: Approved / Changes Requested / Killed | `pipeline-overview.md` steps 11–12 | Decision updates `Overall Status` immediately; the draft link opens the real Drive file (comments happen there, per Non-Goals) |
| REQ-009 | Every item on the Review screen shows how long it's been waiting, with a visible flag once it's past its own Urgency's SLA | `observed-review-stall.md`, thresholds from `workflow-3-content-approval.md` | Flag renders directly on the screen Rohan/Sharva actually look at — the concrete fix for the observed stall, where nothing surfaced the problem until a manual nudge; threshold varies by Urgency (Filler: same-day, High: 72h, Standard: 2 business days) per the Business Rules section below |
| REQ-010 | Changes Requested returns the item to the Production Board, visible to Mahima, with no loop limit | `pipeline-overview.md` Exception B | Matches current behavior — no artificial cap on revision rounds |
| REQ-011 | Approved items auto-create a `Calender` row (one per applicable channel) | `pipeline-overview.md` step 13 (Handoff 3) | Calendar row(s) appear in both the portal's Calendar view and the raw `Calender` sheet, without Mahima manually copying data across |
| REQ-012 | Mahima can flip a channel's status `Scheduled → Live` once she's actually published it on the platform | `pipeline-overview.md` step 14 | Flipping the last applicable channel to Live auto-rolls `Overall Status` to `Published` |
| REQ-013 | All portal reads/writes go through the existing service account (`claude-sheets@...`); no new Sheets/Drive credentials introduced | `30-analysis/tech-stack-decision.md`, Recommendation 1–2 | Portal can be revoked/audited via the existing service account's access alone |
| REQ-014 | Login is Google OAuth restricted to an explicit email allowlist | `30-analysis/tech-stack-decision.md`, Recommendation 4 | Any Google account not on the allowlist is denied access; role (Producer/Approver/Technical Reviewer) read from the same config |

## Data

No new database (per `30-analysis/`). Entities are the existing Sheets/Drive objects, read/written live:

- **Idea** — `Whats?` row (`ID #`, `Title`, Pillar, Urgency, `Screened`) + `Ingestion/idea-<slug>.md` (full description, source, submitted-by)
- **Production Item** — `Master Content Tracker` row (`Item ID`, channel readiness cells, `Overall Status`, `Notes`) + `Production/production-draft-<slug>.md` (brief + one `##` section per channel)
- **Scheduled Post** — `Calender` row (one per channel/publish date)
- **Staleness** — derived, not stored: computed on each Review screen load from `Overall Status` + the Tracker row's last-modified timestamp. No new persistence needed for REQ-009.

## Business Rules

- Routing: Sharva is added as a reviewer only when Pillar includes an engineering/white-paper designation; Rohan is always the default reviewer (per `workflow-3-content-approval.md`).
- On Hold items do not count toward `All Ready` and cannot be submitted for review while blocked.
- Concurrency: last-write-wins, portal always reads fresh immediately before displaying or writing (per `30-analysis/tech-stack-decision.md`) — no locking.
- Killed items are never deleted — row stays with `Overall Status = Killed` and a `Notes` reason, matching current behavior.
- No portal action can set `Overall Status = Approved` except a reviewer decision on the Review screen — mirrors Ground Rule 7 (nothing goes live without review); enforced in code, not just convention.
- Staleness threshold (REQ-009) varies by Urgency, matching each item's own SLA per `workflow-3-content-approval.md` rather than one flat cutoff: Filler → flag if not decided same day; High → flag past 72h; Standard (Medium/Low) → flag past 2 business days. Decided 2026-09-11.
- Dual-reviewer items (Rohan + Sharva, engineering/white-paper Pillar): no required consensus — whichever reviewer records a decision first is the one that moves `Overall Status`; the other sees the resolved outcome if they act afterward. Decided 2026-09-15.
- Recording Changes Requested or Killed does not require a Notes reason — optional, matching current practice where the real detail lives in Drive comments and the Tracker `Notes` is only ever a one-line summary. Decided 2026-09-15.

## Screens

| Screen | Purpose | Spec file |
|---|---|---|
| Inbox | Log raw input, create ideas, classify Pillar/Urgency, screen | `screen-specs/screen-inbox.md` |
| Production Board | Tracker-equivalent view — all in-flight items, per-channel readiness, On Hold flags | `screen-specs/screen-production-board.md` |
| Item Detail | Edit brief + per-channel drafts for one item | `screen-specs/screen-item-detail.md` |
| Review | Rohan/Sharva's view — routed items, staleness, draft link, decision | `screen-specs/screen-review.md` |
| Calendar | Scheduled/published items by channel and date | `screen-specs/screen-calendar.md` |

Screen specs not yet written — next step after this PRD is confirmed, using the `screen-specs` skill.

## Open Questions

1. Does a future phase revisit the Non-Goals here (Slack integration, in-portal comments) once there's actual evidence they're needed, or are these permanently out of scope for this project?

## Closed Questions

1. ~~If Phlo Hub turns out to be a viable host for this portal, does that change any of these screens or requirements?~~ **Closed 2026-09-15** — Phlo Hub integration not pursued (see `30-analysis/tech-stack-decision.md`). Standalone app stands.
