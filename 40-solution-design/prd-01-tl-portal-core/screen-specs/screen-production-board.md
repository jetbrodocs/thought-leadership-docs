---
title: "Screen Spec — Production Board"
status: draft
created: 2026-09-15
updated: 2026-09-15
tags: [screen-spec, ux]
---

# Screen Spec — Production Board

**Module / PRD:** `prd-01-tl-portal-core` — Phase 1: Bookkeeping & Visibility
**Purpose:** Mahima's main working view — every active item across the pipeline, grouped by stage, so bottlenecks are visible at a glance instead of buried in a spreadsheet.

## Entry Points

| From (screen / source) | Trigger | Condition / context passed in |
|---|---|---|
| Portal nav | "Production Board" nav item (default landing screen for Mahima's role) | None |
| Inbox screen | After marking an idea `Screened = Yes` (REQ-002) | Redirects to the Board with the new item's card highlighted in the In Production column |
| Review screen | System effect when a reviewer records Changes Requested (REQ-010) | New card appears in the Changes Requested column with `Notes` populated if the reviewer left any |

## UX Layout

Kanban board, one column per active `Overall Status` stage, left to right in pipeline order:

**In Production → On Hold → All Ready → In Review → Changes Requested**

(`Approved`, `Scheduled`, and `Published` are excluded — approved items move to the Calendar view; `Killed` is excluded by default — see Conditional States.)

- **Column header:** stage name + item count.
- **Card:** Title, Pillar tag, Urgency tag, per-channel readiness dots (one per applicable channel — grey `—` if not applicable, colored by To Do/WIP/Ready).
- **On Hold column specifically:** each card also shows the blocker reason (from `Notes`) directly on the card face — this is the one column where *why* matters more than *what's next*.
- Cards do not drag-and-drop between columns — column position is fully derived from `Overall Status`, changed only via actions on the card or Item Detail (drag-and-drop as a status-change mechanism is out of scope; REQ-006's rollup is automatic anyway).
- Clicking a card opens Item Detail (REQ-004) — full spec in `screen-item-detail.md`.

## Data Displayed

| Label | Value / Format | Source (entity.field / API) |
|---|---|---|
| Title | Text | Production Item — Tracker row `Title` |
| Pillar | Tag | Production Item — Tracker row `Pillar` |
| Urgency | Tag | Production Item — Tracker row `Urgency` |
| Per-channel readiness | One dot per channel, color-coded `—`/To Do/WIP/Ready | Production Item — Tracker row channel cells |
| Overall Status (= column) | Derived from card's column position | Production Item — Tracker row `Overall Status` |
| Blocker reason (On Hold cards only) | Text | Production Item — Tracker row `Notes` |
| Column count | Integer | Count of cards currently in that column |

## CTAs

| Element | Type (button/link/action) | Behavior (what happens / where it goes) |
|---|---|---|
| Card click | Action | Opens Item Detail for that item |
| "New idea" (board-level) | Button | Shortcut to the Inbox screen's create-idea flow |
| "Put on Hold" (card overflow menu) | Action | Prompts for a blocker reason (required), sets `Overall Status = On Hold`, writes the reason to `Notes` (REQ-005) — a manual Mahima action, not system-detected |
| "Remove Hold" (On Hold card overflow menu) | Action | Returns the item to `In Production`; `Notes` reason is cleared |
| "Show killed items" (board-level toggle) | Toggle | Reveals a collapsed section below the active columns — off by default |
| "Submit for Review" | Button, appears only when a card is in `All Ready` | Sets `Overall Status = In Review`, triggers routing (REQ-007) |

## Validations

| Field / Action | Rule | Error message |
|---|---|---|
| "Put on Hold" | Blocker reason cannot be empty | "Add a reason before putting this on hold" |
| "Submit for Review" | Only enabled when a card is actually in `All Ready` | Button hidden otherwise — matches REQ-006, no manual override to submit early |
| Column membership | Read-only / derived — no direct "move to column" control | N/A |

## Conditional States

| State | What the user sees |
|---|---|
| Empty (a column has 0 items) | Column renders with just its header and count (0), no placeholder card |
| Empty (whole board) | "Nothing in production — head to Inbox to screen a new idea" with a button linking to Inbox |
| Loading | Column skeletons while Tracker data loads fresh |
| Error | Board-level banner: "Couldn't load the Tracker" with retry |
| Killed items (default) | Hidden entirely unless "Show killed items" is toggled on |
| Approved / Scheduled / Published items | Never shown here — live on the Calendar view instead |

## Open Questions

1. Card ordering within a column — oldest-first, or by Urgency (High first)? Not yet decided.
2. Should "Show killed items" persist per session, or reset to hidden every time the Board loads? Minor UX call.
