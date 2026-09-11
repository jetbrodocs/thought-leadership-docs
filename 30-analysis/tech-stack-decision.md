---
title: "Tech Stack Decision — TL Pipeline Tool"
status: approved
created: 2026-09-11
updated: 2026-09-11
tags: [analysis]
---

# Tech Stack Decision — TL Pipeline Tool

## Summary

Build a **lightweight portal, not a standalone app with its own database.** `Whats?`, `Master Content Tracker`, and `Calender` (Google Sheets) plus the `Ingestion/`/`Production/` Drive folders **stay the system of record** — the portal is a dashboard layered on top of them, not a replacement. This reuses the existing Google service account (`claude-sheets@...`) and its `googleapis`-based access. No new database is introduced at this stage.

**One option deliberately left open, not resolved here:** whether this should be built inside **Phlo Hub** — Jetbro's existing internal automation tool — instead of as a separate app. Neither Mahima nor this analysis has enough information about Phlo Hub's actual capabilities to evaluate that option honestly. See Open Questions.

## Findings

| # | Finding | Evidence | Impact |
|---|---|---|---|
| 1 | Working plumbing into the actual 3 sheets already exists. A Google service account (`claude-sheets@...`) with a Node script already directly reads/writes `Whats?`, `Master Content Tracker`, and `Calender`. | `reference_sheets_access.md`, `reference_drive_sheets.md`, `update_sheets.js` in the `thought-leadership` Vault folder | This is the foundation any portal builds on directly — no migration needed, no risk of the portal's data drifting from what Mahima/Rohan/Sharva see when they open the sheets themselves (they still can, any time). |
| 2 | The pipeline (per `20-process-maps/pipeline-overview.md`) is fundamentally a status-tracking and handoff problem — items move through 3 sheets via a shared Item ID — not a high-volume transactional system. | Process map, `shared-foundations.md` | Favors a thin read/write layer over a full application database. The complexity a database buys (transactions, relations, migrations) isn't needed for what's actually observed. |
| 3 | Team size is 3 people today (Mahima, Rohan, Sharva), with one anticipated future hire (Marketing SOP §10, an "internal marketer" role). | `shared-foundations.md` People section, `CLAUDE.md` Team table | A 3-4 person internal tool doesn't need infrastructure sized for scale — this argues against a heavier standalone-app architecture (e.g. a Postgres-backed service with its own auth/user model). |
| 4 | A real alternative — building inside **Phlo Hub**, Jetbro's existing internal automation tool — was raised as a serious option, not a hypothetical. | Prior conversation with Sharva (outside this project's own artifacts — happened in Slack during an earlier, since-deleted pass at this project) | Not evaluable right now: neither this analysis nor Mahima currently knows Phlo Hub's actual architecture, extensibility model, or whether it could support something like a Kanban review dashboard. Flagged as an open question rather than assumed either way. |

## Recommendations

1. **Data layer:** No new database. The 3 Google Sheets and the 2 Drive folders remain authoritative. The portal reads and writes them live via the existing service account + `googleapis`. If solution design later surfaces a genuine need for structured data that doesn't fit a spreadsheet row (e.g. threaded review comments replacing Drive comments), that's a narrow, explicit exception to be decided and documented as an addendum here when it actually comes up — not assumed now.
2. **Backend:** A thin API layer (Next.js API routes are enough at this scale — no separate service needed) that wraps the existing Sheets/Drive calls, extending `update_sheets.js`'s approach rather than replacing it.
3. **Frontend:** Next.js — dashboard views mirroring the process map's stages. Renders data pulled live from Sheets/Drive on each load rather than from a local cache, so the portal can never show something the sheets themselves disagree with.
4. **Auth:** Google OAuth restricted to an explicit email allowlist (Mahima/Rohan/Sharva today, extendable by editing config, not schema, when the anticipated hire happens).
5. **Hosting:** Left open deliberately — with no database yet, a simple serverless deployment (e.g. Vercel) is sufficient. Revisit only if Recommendation 1's database exception is ever triggered (a persistent-disk requirement would rule out pure serverless).
6. **What stays exactly as-is:** the sheets, the Drive folders, the file-per-item pattern (`idea-<slug>.md`, `production-draft-<slug>.md`). The portal is additive — anyone can always fall back to editing a sheet or a Drive file directly if the portal is down or wrong.
7. **Build order:** bookkeeping/sync first (Sheets+Drive read/write, dashboard views) — prove this works before any AI-drafting feature is built on top of it. AI drafting is out of scope for this first pass entirely (see `CLAUDE.md` — this project documents the pipeline as observed; an AI-drafting phase, if pursued, is a separate future PRD).
8. **Concurrency:** last-write-wins. The portal always reads fresh from the Sheets/Drive right before displaying or editing — no locking, no conflict warnings. Same behavior as two people editing a Google Sheet directly today; acceptable at this scale.

## Open Questions

1. **Phlo Hub** — what is it, actually (architecture, extensibility, who owns/maintains it), and could the TL portal reasonably live inside it instead of as a separate app? Needs a real conversation with Sharva (and whoever else knows Phlo Hub) before this can be answered — not something to guess at. Until answered, Recommendation 2 (standalone Next.js app) is the default, not a final ruling-out of the alternative.
2. If Phlo Hub turns out to be a good fit, does that change Recommendation 1 (no new database) — i.e. does Phlo Hub already have its own data layer that would change this calculus entirely?
