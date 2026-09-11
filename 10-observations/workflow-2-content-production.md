---
title: "Workflow 2 — Content Production"
status: draft
created: 2026-09-11
updated: 2026-09-11
tags: [observation]
---

# Workflow 2 — Content Production

Owner: Mahima (sole producer). Purpose: turn a Screened idea into finished, channel-adapted drafts, ready for approval. Source: `Jetbro Content Production Workflow.md` §3.

## Activity

One idea becomes many publishing points. Mahima writes a single master draft once, self-edits it against the Ground Rules and voice, then adapts that approved master into every applicable channel's format. Production isn't done until every applicable channel cell is `Ready`.

## Inputs

| Input | Source | Format | Notes |
|---|---|---|---|
| Screened idea (`Whats?` row + `idea-<slug>.md`) | Workflow 1 | Sheet row + Markdown | Trigger: `Screened = Yes` |
| Additional source material (case-study outcome, technical core) | Rohan / Sharva, on request | Varies | Only needed for specific pieces — missing input triggers `On Hold` |

## Outputs

| Output | Destination | Format | Notes |
|---|---|---|---|
| Tracker row | `Master Content Tracker` | Sheet row | Carries `Item ID`, `Title`, `Content Type`, `Pillar`, `Urgency`, per-channel readiness cells |
| `production-draft-<slug>.md` | `Production/` (Drive) | Markdown | Brief + one `##` section per channel — one file per item |

## People

| Role | Count | Notes |
|---|---|---|
| Mahima | 1 | Sole producer — writes brief, master draft, self-edits, adapts to every channel |
| Rohan | 1 (on request) | Supplies case-study source material when a piece needs it |
| Sharva | 1 (on request) | Supplies technical cores for engineering-pillar pieces |

## Timing

- **Queue priority:** High urgency → front (48–72h) · Medium → 1–2 weeks · Low → backlog
- **Duration:** Not specified in source per-stage; no time estimate for how long production takes from intake to `All Ready`

## Systems

| System | Used For | Notes |
|---|---|---|
| `Master Content Tracker` (Sheet) | Readiness tracking — per-channel cells + `Overall Status` | The working source of truth once an item is Screened |
| `Production/` (Drive folder) | Holds `production-draft-<slug>.md` files | One file per item, all channel versions in one place |

## Handoffs

- **Comes from:** Workflow 1, triggered by `Screened = Yes`
- **Goes to:** Workflow 3 (Content Approval), triggered by `Overall Status = All Ready`

## Stages (as documented)

1. **Production Intake & Queue** — Tracker row created; publishing-point scope set (applicable cells `To Do`, non-applicable `—`); `Overall Status = In Production`; queued by urgency
2. **Production Brief** — angle/POV, audience lens, the one idea + closing hook, formats required, source material pulled from `Ingestion/`. **Missing-input rule:** if a needed input (e.g. case-study data from Rohan, technical core from Sharva) isn't available, set `Overall Status = On Hold`, record the blocker in `Notes`, re-queue once unblocked
3. **Master Draft** — the single best expression of the idea in its primary format, written into `production-draft-<slug>.md`'s primary `##` section; primary cell → `WIP`
4. **Self-edit / Quality Pass** — Mahima reviews the master against the 7 Ground Rules + voice *before* adapting to other channels (fix once, not five times); primary cell → `Ready`. This is Mahima's own bar, distinct from Rohan's later approval
5. **Channel Adaptation** — from the `Ready` master, Mahima writes each applicable channel's `##` section (LinkedIn, X, Instagram, Substack, Website), following distribution tables and platform rules — never a verbatim copy across channels; each cell moves `To Do → WIP → Ready`; `Overall Status` rolls up (`Partially Ready` → `All Ready`)
6. **Production Complete** — `Overall Status = All Ready` hands the item to Workflow 3

## Problems and Workarounds

| Problem | Workaround | Impact |
|---|---|---|
| A piece can need input Mahima doesn't have (case-study data, technical core) | `On Hold` status + `Notes` reason + re-queue when unblocked | Item stalls indefinitely if Rohan/Sharva don't respond to the request — no stated SLA for supplying requested input |
| Every channel section lives in one shared file (`production-draft-<slug>.md`) | Single markdown file, one `##` per channel | Simplifies one-pass review (Workflow 3), but means the whole item is one file — no per-channel file-level history or diffing |

## Open Questions

1. How often does the `On Hold` blocker actually trigger, and how long do items typically sit blocked waiting on Rohan/Sharva?
2. Is there a stated or implied SLA for Rohan/Sharva to supply requested case-study/technical input, or is it purely ad hoc?
3. What does the "self-edit bar" actually look like in practice — is it a checklist Mahima runs through, or a subjective read?
