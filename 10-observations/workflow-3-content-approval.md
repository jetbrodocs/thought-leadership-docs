---
title: "Workflow 3 — Content Approval"
status: draft
created: 2026-09-11
updated: 2026-09-11
tags: [observation]
---

# Workflow 3 — Content Approval

Approver: Rohan (quality gate); Sharva additionally for engineering/white-paper technical accuracy. Purpose: ensure every piece meets the Jetbro bar before scheduling. Source: `Jetbro Content Production Workflow.md` §4.

## Activity

Mahima submits an `All Ready` item for review by pointing the reviewer at the single `production-draft-<slug>.md` file (all channels in one pass). The reviewer checks it against the ground rules, voice, and factual accuracy, then returns one of three outcomes. Approved items get scheduled; everything else loops back to Mahima.

## Inputs

| Input | Source | Format | Notes |
|---|---|---|---|
| `production-draft-<slug>.md` at `Overall Status = All Ready` | Workflow 2 | Markdown, all channel sections drafted | Single file reviewed in one pass |

## Outputs

| Output | Destination | Format | Notes |
|---|---|---|---|
| Decision + comments | `Master Content Tracker` (`Overall Status`, `Notes`) + Drive comments on the draft | Sheet cell + threaded comments | One-line summary in Tracker `Notes`, full feedback in-context on the file |
| Scheduled row | `Calender` sheet | Sheet row | Only added on `Approved` |

## People

| Role | Count | Notes |
|---|---|---|
| Rohan | 1 | Default reviewer for all content — business angle, POV, voice |
| Sharva | 1 | Additional reviewer for engineering-pillar / white-paper pieces — technical accuracy only, reviews before/alongside Rohan |
| Mahima | 1 | Submits for review, executes revisions on `Changes Requested`, schedules on `Approved` |

## Timing

- **Standard SLA:** Rohan reviews within 2 business days of `In Review`
- **Filler:** same-day review
- **High-urgency/reactive:** compressed review inside the 48–72h window
- **Backlog discipline:** a buffer of `Approved` evergreen items is meant to be kept so a slow review week doesn't create a calendar gap

## Systems

| System | Used For | Notes |
|---|---|---|
| `Master Content Tracker` (Sheet) | `Overall Status` transitions, `Notes` summary | |
| Drive comments | In-context, threaded reviewer feedback on the draft file | |
| `Calender` (Sheet) | Scheduling — one row per scheduled post per channel | Only populated after `Approved` |

## Handoffs

- **Comes from:** Workflow 2, on `Overall Status = All Ready`
- **Goes to:** Scheduling/`Calender`, on `Approved`; back to Workflow 2 (Stage 4/5) on `Changes Requested`

## Stages (as documented)

1. **Submit for Review** — `Overall Status = In Review`; routed to Rohan (default) and Sharva (additionally, for technical pieces)
2. **Review Against the Bar** — 7 Ground Rules, tone of voice, factual/outcome accuracy (esp. case-study anonymisation), per-channel fit (no verbatim copies)
3. **Decision** — `Approved` (channel cells stay `Ready`, cleared to schedule) · `Changes Requested` (comments logged, returns to Workflow 2, re-submits back to `In Review`, no loop limit) · `Killed` (not published, reason in `Notes`, row kept for record — not deleted)
4. **Handoff to Scheduling** — on `Approved`, Mahima adds the item to `Calender` (one row per scheduled post per channel); as each post publishes, its Tracker cell moves `Ready → Scheduled → Live` and `Overall Status` moves `Approved → Scheduled → Published`

## Problems and Workarounds

| Problem | Workaround | Impact |
|---|---|---|
| Approval is item-level, not per-channel, even though feedback can target a specific channel section | "If Rohan wants a specific channel changed, that's Changes Requested on that section" — informal convention, not a distinct tracked state | A single flagged channel blocks the whole item's approval status even if other channels are fine |
| No stated limit on `Changes Requested` revision loops | Each loop is logged on the draft | An item can cycle indefinitely with no escalation trigger |
| Rohan is a single reviewer for all content by default | Sharva only covers technical accuracy, not general review capacity | Rohan is a bottleneck; SLA (2 business days) depends entirely on his availability |

## Open Questions

1. How often does `Changes Requested` actually trigger, and how many loops does a typical item go through before `Approved`?
2. Is there a record of `Killed` items and why — is that data used for anything (e.g. refining screening criteria)?
3. Does the Filler same-day SLA actually hold in practice, or does it slip alongside Standard items when Rohan is busy?
