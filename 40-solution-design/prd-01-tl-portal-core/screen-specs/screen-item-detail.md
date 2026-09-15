---
title: "Screen Spec — Item Detail"
status: draft
created: 2026-09-15
updated: 2026-09-15
tags: [screen-spec, ux]
---

# Screen Spec — Item Detail

**Module / PRD:** `prd-01-tl-portal-core` — Phase 1: Bookkeeping & Visibility
**Purpose:** Where Mahima writes and edits the brief and per-channel drafts for one item, sets which channels apply, and marks channel readiness.

## Entry Points

| From (screen / source) | Trigger | Condition / context passed in |
|---|---|---|
| Production Board | Card click (any column) | Selected item's ID; opens in its current state |
| Production Board | Auto-redirect after screening an idea (`Screened = Yes`, REQ-002) | New item, all channel cells blank — lands directly on the channel-scope step since nothing's been chosen yet |

## UX Layout

Single scrolling page:

1. **Header:** Title, Pillar, Urgency, Overall Status badge.
2. **Changes Requested banner** *(conditional)* — if `Overall Status = Changes Requested`, a banner under the header shows the reviewer's `Notes` summary (if any was left) and a link to the draft's Drive comments, where the actual detailed feedback lives (per this PRD's Non-Goals — comments were never moved into the portal). Collapsible, expanded by default.
3. **Channel scope row** — five toggles (LinkedIn / X / Instagram / Substack / Website). Toggling one on:
   - Sets that channel's Tracker cell to `To Do` (from blank `—`)
   - Reveals that channel's draft section below
   Toggling off (only allowed before any content is written to that section) hides the section and reverts the cell to `—`.
4. **Brief section** — plain markdown textarea, the master brief all channel drafts derive from.
5. **One section per toggled-on channel** (fixed order: LinkedIn, X, Instagram, Substack, Website) — each a plain markdown textarea plus that channel's readiness control.

## Data Displayed

| Label | Value / Format | Source (entity.field / API) |
|---|---|---|
| Title, Pillar, Urgency | Text/tags | Production Item — Tracker row |
| Overall Status | Badge | Production Item — Tracker row `Overall Status` |
| Changes Requested summary | Text (one line, if present) | Production Item — Tracker row `Notes` |
| Drive comments link | Link | `Production/production-draft-<slug>.md`'s stored Drive link |
| Channel toggles state | On/off per channel | Derived: on if Tracker cell ≠ `—` |
| Brief content | Markdown text | `Production/production-draft-<slug>.md` — brief section |
| Per-channel draft content | Markdown text | `Production/production-draft-<slug>.md` — that channel's `##` section |
| Channel readiness | To Do / WIP / Ready | Production Item — Tracker row, that channel's cell |

## CTAs

| Element | Type (button/link/action) | Behavior (what happens / where it goes) |
|---|---|---|
| Channel toggle (on) | Toggle | Sets cell to `To Do`, reveals section, empty textarea ready for content |
| Channel toggle (off) | Toggle | Only enabled if that section's textarea is still empty; hides section, resets cell to `—` |
| Textarea (brief or any channel) | Text input | Freeform markdown; saves on blur |
| Channel readiness dropdown | Select (To Do / WIP / Ready) | Manual — Mahima marks it herself; not auto-derived from content, since "Ready" is a judgment call |
| "Put on Hold" | Button | Same action as the Production Board's card overflow menu — included here too since Mahima may realize mid-edit something's blocking her |
| "Open Drive Comments" (Changes Requested banner) | Link | Opens the draft file's comment view in Drive, new tab |
| Resubmit (appears only when `Overall Status = Changes Requested` and all applicable channels are back to `Ready`) | Button | Same effect as the Production Board's "Submit for Review": sets `Overall Status = In Review`, re-routes (REQ-007) |

## Validations

| Field / Action | Rule | Error message |
|---|---|---|
| Channel toggle off | Blocked if that channel's textarea has any saved content | "Clear this channel's draft before removing it" |
| Marking a channel Ready | Textarea must be non-empty | "Add content before marking this channel Ready" (dropdown option disabled otherwise) |
| At least one channel | Cannot leave all five toggled off | "At least one channel must apply" |
| Save | Standard save-on-blur; no format validation on markdown content | — |

## Conditional States

| State | What the user sees |
|---|---|
| New item, no channels chosen yet | Only the channel-scope row and brief section are visible |
| Empty (brief not yet written) | Placeholder text: "Start with the master brief — channel drafts build from this" |
| Loading | Skeleton for header + sections while Tracker row and Drive markdown file both load |
| Error | "Couldn't load this item's draft" with retry, if the Drive file read fails independently of the Tracker row succeeding |
| Changes Requested | Banner expanded by default; stays visible until Mahima resubmits |

## Open Questions

1. Save-on-blur granularity — does each textarea save independently the moment it loses focus, or is there one page-level "Save" action for everything at once?
2. Since Drive comments (not portal comments) hold the real feedback, does the portal need any way to show *whether* the reviewer actually left comments, versus just the one-line Notes summary — or is "open the link and look" good enough for Phase 1?
