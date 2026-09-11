---
title: "Shared Foundations — Where Things Live, Status Vocabulary, Roles"
status: draft
created: 2026-09-11
updated: 2026-09-11
tags: [observation]
---

# Shared Foundations — Where Things Live, Status Vocabulary, Roles

Cross-cutting reality that applies across all three workflows (Ingestion & Screening, Content Production, Content Approval). Captured from `Jetbro Content Production Workflow.md` (v2.0, owner: Mahima, last updated June 2026) — the doc explicitly states it supersedes the partial workflow sketches in `Jetbro Marketing SOP.md` §5 and `Jetbro Thought Leadership Strategy.md` §8.

## Activity

Content moves through the pipeline as a single item that is handed off between three sheets/trackers, never copied or duplicated. Markdown files (in Google Drive) hold the actual content; Google Sheets hold the index/status.

## Inputs

| Input | Source | Format | Notes |
|---|---|---|---|
| Raw content idea | Anyone (team, Rohan, Sharva) | Any — URL, text, voice note, audio, video, Slack dump, meeting notes | No required format; low friction is the explicit goal |

## Outputs

| Output | Destination | Format | Notes |
|---|---|---|---|
| Published post/article | LinkedIn, X, Instagram, Substack, Website | Per-channel | End state: Tracker cell `Live`, Overall Status `Published` |

## People

| Role | Count | Notes |
|---|---|---|
| Mahima | 1 | Single inbox; runs Workflows 1 and 2 end to end; owns all tracker upkeep and scheduling |
| Rohan | 1 | Default approver (Workflow 3); also supplies case-study source material on request |
| Sharva | 1 | Technical reviewer for engineering-pillar / white-paper content only; supplies technical cores on request; not a producer |
| AI agent | 0 (not yet built) | Planned to assist Mahima with drafting/adaptation in Workflow 2. Never owns a stage — Mahima always edits |

## Timing

- **Frequency:** Continuous — items enter the pipeline whenever raw input arrives
- **Queue priority:** High urgency → front (target 48–72h) · Medium → 1–2 weeks · Low → backlog

## Systems

| System | Used For | Notes |
|---|---|---|
| `Whats?` (Google Sheet) | Screening register | Frozen once `Screened = Yes` — becomes historical record |
| `Master Content Tracker` (Google Sheet) | Production & readiness tracking | The working source of truth from `Screened` onward through approval |
| `Calender` (Google Sheet) | Scheduling & publishing | Only receives an item after `Approved` |
| `Ingestion/` (Drive folder) | Raw inputs + `idea-<slug>.md` files | One file per idea, holds full description |
| `Production/` (Drive folder) | `production-draft-<slug>.md` files | One file per item — holds brief + master draft + every channel adaptation |
| Drive comments | Reviewer feedback | Threaded, in-context, on the `production-draft-<slug>.md` file itself |

## Handoffs

Three explicit handoffs tie the sheets together via a shared **Item ID** (`Whats?` `ID #` = Tracker `Item ID`), so a piece is traceable without copying data between sheets:

1. **`Whats?` → Tracker** — triggered when `Screened = Yes`. `Whats?` freezes; Tracker takes ownership.
2. **Tracker → Approval** — triggered when `Overall Status = All Ready`. Item enters Workflow 3.
3. **Tracker → `Calender`** — triggered when `Overall Status = Approved`. Only approved items get scheduled.

## Status Vocabulary (canonical, owned by the Tracker)

**Per-publishing-point (Tracker channel cells):** `—` (N/A) · `To Do` · `WIP` · `Ready` · `Scheduled` · `Live`

**Item-level (Tracker `Overall Status`):** `In Production` · `Partially Ready` · `All Ready` · `On Hold` · `In Review` · `Changes Requested` · `Approved` · `Scheduled` · `Published` · `Killed`

Note: the Workflow doc states the ad-hoc status key previously used in `LinkedIn Posts — Draft.md` is retired in favour of this single vocabulary.

## Problems and Workarounds

| Problem | Impact |
|---|---|
| Marketing SOP §5 describes Workflows 2 and 3 as "to be defined," but a separate, more detailed Workflow doc already exists and supersedes it | Two documents can drift out of sync; a reader consulting only the SOP gets a stale/incomplete picture |
| Marketing SOP §5 Stage 2 says Mahima documents "the what" decision "in the Content Calendar" — the superseding Workflow doc says this happens in the `Whats?` sheet instead | Confirms the SOP is stale on more than just "to be defined" sections — it names the wrong sheet, not just an incomplete one |
| Entire pipeline runs on manually maintained Google Sheets + Drive files, operated by one person (Mahima) | Single point of failure; no automation; status transitions depend on Mahima remembering to update three separate sheets |
| No AI agent built yet, despite being referenced as "when built" across multiple source docs | Workflow 2 (drafting + channel adaptation) is fully manual today |
| Two versions of the top-level strategy docs exist in the Vault (root vs. `thought-leadership/inbox/`) with real content differences, not just duplicates | Risk of working from a stale version; both kept in `00-inbox/` for comparison, inbox versions treated as newer/superset |

## Open Questions

1. Is the `Whats?` / Tracker / `Calender` sheet structure (columns, exact cell values) available to inspect directly, or only as described in the Workflow doc?
2. What does "AI agent, when built" actually mean in scope — is this project's PRD meant to *be* that agent, or a broader tool around the whole pipeline?
3. How many items per week/month currently flow through the pipeline (volume), to size what "automation" needs to handle?
