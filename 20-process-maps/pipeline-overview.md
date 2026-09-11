---
title: "TL Content Pipeline — End to End"
status: draft
created: 2026-09-11
updated: 2026-09-11
tags: [process]
---

# TL Content Pipeline — End to End

Built from `10-observations/shared-foundations.md` and the three workflow observation docs. This is one continuous process, not three independent ones — an item is a baton pass between three sheets (`Whats?` → `Master Content Tracker` → `Calender`), never copied or run in parallel.

## Process Overview

- **Purpose:** Turn any raw input (from anyone) into a published, on-brand piece of thought-leadership content across every applicable channel.
- **Trigger:** Raw input arrives — any format, from anyone.
- **End condition:** Every applicable channel's post is live, or the item is explicitly `Killed` and parked.
- **Frequency:** Continuous — items enter whenever raw input arrives.
- **Typical duration:** Not documented in source material — no end-to-end SLA exists, only per-stage targets (see Systems and Tools / Known Issues).

## Roles Involved

| Role | Responsibility in This Process |
|---|---|
| Mahima | Single inbox; screens and classifies; sole producer (brief, master draft, self-edit, channel adaptation); submits for review; schedules approved items |
| Rohan | Default approver — reviews every item for business angle, POV, voice; also a source for case-study material on request |
| Sharva | Additional reviewer for engineering-pillar / white-paper items only (technical accuracy); also a source for technical cores on request |
| Submitters (team, Rohan, Sharva) | Anyone can submit raw input; only Mahima processes it |

## Inputs

| Input | Source | Notes |
|---|---|---|
| Raw content idea | Anyone | Any format — URL, text, voice note, audio, video, Slack dump, meeting notes |
| Case-study material / technical core | Rohan / Sharva, on request | Only needed for specific pieces; missing input is what triggers the On Hold exception |

## Outputs

| Output | Destination | Notes |
|---|---|---|
| Published post/article | LinkedIn, X, Instagram, Substack, Website | Per-channel; end state is Tracker cell `Live` and `Overall Status = Published` |
| Killed item record | `Master Content Tracker` (row retained, not deleted) | Reason in `Notes` |

## Process Steps

### Main Flow

1. Raw input arrives from anyone, any format, and is stored in `Ingestion/`.
2. Mahima decides "the what" — one post, several, a case-study angle, or nothing — and creates a `Whats?` row plus `Ingestion/idea-<slug>.md`.
3. Mahima classifies the idea: Pillar (1–5 or Filler, combos allowed) and Urgency (High/Medium/Low).
4. Mahima sets `Screened = Yes`. **Handoff 1:** this freezes the `Whats?` row and triggers creation of the item's row in `Master Content Tracker`.
5. Tracker row is created; applicable channel cells set to `To Do`, non-applicable to `—`; `Overall Status = In Production`; item queued by urgency (High → front, Medium → 1–2 weeks, Low → backlog).
6. Mahima writes the production brief (angle/POV, audience lens, the one idea, closing hook, formats required).
   - **If a needed input is missing** (case-study data, technical core): go to Exception A (On Hold).
   - **If everything needed is on hand:** continue to step 7.
7. Mahima writes the master draft into `Production/production-draft-<slug>.md`'s primary channel section; that cell moves to `WIP`.
8. Mahima self-edits the master against the 7 Ground Rules and Jetbro's tone of voice. This is Mahima's own quality bar, distinct from Rohan's later approval. Primary cell moves to `Ready`.
9. From the `Ready` master, Mahima adapts into every other applicable channel's `##` section — never a verbatim copy. Each cell moves `To Do → WIP → Ready` as it's finished. `Overall Status` rolls up automatically: some cells `Ready` → `Partially Ready`; all applicable cells `Ready` → `All Ready`.
10. At `All Ready`, Mahima submits for review: `Overall Status = In Review`. Routed to Rohan always, plus Sharva if the Pillar is engineering/white-paper.
11. The reviewer checks the single `production-draft-<slug>.md` file (all channels, one pass) against the Ground Rules, tone of voice, factual/case-study accuracy, and per-channel fit.
12. The reviewer records a decision.
    - **Approved:** channel cells stay `Ready`; continue to step 13.
    - **Changes Requested:** go to Exception B.
    - **Killed:** go to Exception C.
13. **Handoff 3:** Mahima adds the item to `Calender` (one row per channel/publish date).
14. As each post actually publishes, its Tracker cell moves `Ready → Scheduled → Live`; `Overall Status` moves `Approved → Scheduled → Published`.

### Exception A: Missing Input (On Hold)

A1. Mahima identifies a needed input she doesn't have (e.g. a case-study outcome from Rohan, a technical core from Sharva).
A2. Sets `Overall Status = On Hold`, records the blocker in `Notes`, and requests the input.
A3. Item sits blocked — no documented SLA for how quickly Rohan/Sharva supply what's requested.
A4. Once the input arrives, item re-enters the queue and returns to the main flow at step 6.

### Exception B: Changes Requested

B1. Reviewer logs comments on the draft (Drive comments, in-context) plus a one-line summary in Tracker `Notes`.
B2. `Overall Status = Changes Requested`.
B3. Item returns to Mahima to revise (back to main flow around step 7–9, whichever channel(s) need rework).
B4. Mahima resubmits — `Overall Status = In Review` again, returning to step 10. No documented limit on how many times this loop can repeat.

### Exception C: Killed

C1. Reviewer marks the item `Killed` instead of `Approved` or `Changes Requested`.
C2. `Overall Status = Killed`; reason recorded in `Notes`.
C3. Row is retained in the Tracker, not deleted. Process ends for this item.

## Connected Processes

- **Upstream:** None — this is the pipeline's entry point. Raw input can originate from any Jetbro team member, not a defined upstream process.
- **Downstream:** Not documented in source material — no observed process for post-publish tracking (engagement, follow-up, repurposing).
- **Related:** Rohan's personal LinkedIn pipeline and the Sales/BD pipeline both exist at Jetbro but are explicitly out of scope for this project (per `CLAUDE.md`) and not observed here.

## Systems and Tools

| Step | System/Tool | How It's Used |
|---|---|---|
| 1–4 | `Ingestion/` (Drive), `Whats?` (Sheet) | Raw input storage; screening register, frozen at `Screened = Yes` |
| 5–12 | `Master Content Tracker` (Sheet), `Production/` (Drive) | Readiness tracking (per-channel cells + `Overall Status`); all draft content in one markdown file per item |
| 11 | Drive comments | In-context, threaded reviewer feedback on the draft file |
| 13–14 | `Calender` (Sheet) | Scheduling — one row per scheduled post per channel; only populated after `Approved` |

## Known Issues

| Issue | Impact | Current Workaround |
|---|---|---|
| Single inbox, single producer, single default reviewer (all Mahima or Rohan) | Each is a single point of failure; if unavailable, their stage of the pipeline stalls entirely regardless of urgency | None documented |
| No SLA for supplying requested input (Exception A) or for screening raw input (step 1–4) | Items can sit blocked or unscreened indefinitely with no visibility to the submitter | None documented |
| No limit on `Changes Requested` loops (Exception B) | An item can cycle indefinitely with no escalation trigger | Each loop is logged on the draft, but that's a record, not a limit |
| Approval is item-level even though feedback can target one channel | A single flagged channel blocks the whole item's approval, even if other channels are fine | Informal convention: "if Rohan wants a specific channel changed, that's Changes Requested on that section" — not a distinct tracked state |
| Marketing SOP §5 is stale — describes Workflows 2/3 as "to be defined" and names the wrong sheet (Content Calendar, not `Whats?`) for the "what" decision | A reader consulting only the SOP gets a stale or incorrect picture of steps 2–14 above | Content Production Workflow doc supersedes it, but both still exist side by side |

## Open Questions

1. What does step 1 (raw input submission) actually look like day to day — is there a real backlog of unscreened input sitting anywhere, and if so, how large?
2. Is there any downstream process at all after `Published` (performance tracking, repurposing), or does the pipeline's observed scope genuinely end at publish?
3. How many items per week/month currently flow through this pipeline — needed to size what future automation has to handle.
4. How often do Exceptions A, B, and C actually fire relative to the main flow completing cleanly?
