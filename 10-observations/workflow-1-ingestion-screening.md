---
title: "Workflow 1 — Ingestion & Screening"
status: draft
created: 2026-09-11
updated: 2026-09-11
tags: [observation]
---

# Workflow 1 — Ingestion & Screening

Owner: Mahima. Purpose: turn any raw input into a defined, classified, production-ready idea. Source: `Jetbro Content Production Workflow.md` §2.

## Activity

Raw input of any kind is submitted to Mahima, who decides what content (if any) comes out of it, documents that decision, classifies it by pillar and urgency, and marks it screened — the trigger for production to begin.

## Inputs

| Input | Source | Format | Notes |
|---|---|---|---|
| Raw content idea | Team, Rohan, Sharva (anyone) | Any format — URL, text, voice note, audio, video, Slack dump, meeting notes | No required format; goal is nothing useful gets lost |

## Outputs

| Output | Destination | Format | Notes |
|---|---|---|---|
| `idea-<slug>.md` | `Ingestion/` (Drive) | Markdown | Title, full description, source, submitted-by |
| New row | `Whats?` sheet | Sheet row | `ID #`, `Title`, Pillar, Urgency, `Screened` flag |

## People

| Role | Count | Notes |
|---|---|---|
| Mahima | 1 | Single inbox for all raw input; makes the editorial "what" decision; classifies; screens |
| Submitters | Many | Team, Rohan, Sharva — anyone can submit, but only Mahima processes |

## Timing

- **Frequency:** Continuous, whenever raw input arrives
- **Duration:** Not specified in source — no time estimate given for how long screening takes per item

## Systems

| System | Used For | Notes |
|---|---|---|
| `Ingestion/` (Drive folder) | Stores raw input + `idea-<slug>.md` | |
| `Whats?` (Sheet) | Screening register | Frozen once `Screened = Yes` |

## Handoffs

- **Comes from:** Anyone, any channel — no upstream process, this is the pipeline's entry point
- **Goes to:** Workflow 2 (Content Production), triggered by `Screened = Yes` — creates the item's row in the Master Content Tracker; high-urgency items go to the front of the production queue

## Stages (as documented)

1. **Ingestion** — raw input stored in `Ingestion/`
2. **The What** — Mahima's editorial call: one post, several, a case-study angle, or nothing; creates `Whats?` row + `idea-<slug>.md`
3. **Classification** — Pillar (1–5 or Filler, combos allowed) + Urgency (High 48–72h / Medium 1–2 weeks / Low evergreen)
4. **Screening Complete** — `Screened = Yes` set; nothing proceeds without classification

## Problems and Workarounds

| Problem | Impact |
|---|---|
| Single inbox = single point of failure (only Mahima processes intake) | If Mahima is unavailable, nothing enters production regardless of urgency |
| No stated SLA for how quickly raw input gets screened after submission | A submitter has no visibility into when (or whether) their input will be actioned |
| "Nothing at all" is a valid outcome of the What decision, but there's no documented record of *rejected* raw input | No way to audit what was submitted but never turned into content, or why |

## Open Questions

1. Is there a backlog/count of raw inputs sitting un-screened at any given time?
2. What does a submitter see/get told when their input is submitted — any acknowledgment, or silence until (if) it becomes a `Whats?` row?
