---
title: "Screen Spec — Calendar"
status: draft
created: 2026-09-15
updated: 2026-09-15
tags: [screen-spec, ux]
---

# Screen Spec — Calendar

**Module / PRD:** `prd-01-tl-portal-core` — Phase 1: Bookkeeping & Visibility
**Purpose:** Scheduling and publish-tracking view. Shows scheduled posts by date, lets Mahima flip a channel to Live once she's actually posted it, and surfaces Approved items still waiting on a date (REQ-011, REQ-012).

## Entry Points

| From (screen / source) | Trigger | Condition / context passed in |
|---|---|---|
| Portal nav | "Calendar" nav item | None |
| Review screen | Automatic — an item Approved on the Review screen leaves the Production Board and its Calendar row(s) appear here (REQ-011) | One row auto-created per applicable channel, initially unscheduled (no date) |

## UX Layout

Month grid (default view: current month), plus an "Unscheduled" panel above the grid for items a grid has no cell for.

- **Unscheduled panel** *(top, shown only when non-empty)* — Approved items whose Calendar row(s) don't have a date yet. Each entry: Title, Pillar, Platform, a "Set date" action. This is the direct fix for approved-but-forgotten items — the same class of gap that caused the stall in `observed-review-stall.md`, just at the scheduling stage instead of review.
- **Month grid** — standard 7-column calendar. Each date cell holds a small entry per scheduled/published post falling on that date (one entry per channel/row, since REQ-011 creates one row per applicable channel). An entry shows Platform (icon or short tag) and Title (truncated). Multiple entries on one date stack vertically within the cell.
- Month navigation: previous/next month, a "Today" jump.
- Clicking a date-cell entry opens a small detail popover: Title, Pillar, Platform, Content Format, Post Type, Status, Content Link, and the "Mark Live" action.

## Data Displayed

| Label | Value / Format | Source (entity.field / API) |
|---|---|---|
| Week | Not directly shown — the grid's own date layout replaces the sheet's `Week` column | `Calender` sheet `Week` column (unused in this layout, still present in the underlying sheet) |
| Date | Cell position on the grid | `Calender` sheet `Date` column |
| Pillar | Tag (in popover) | `Calender` sheet `Pillar` column |
| Title | Text (truncated on cell entry, full in popover) | `Calender` sheet `What?` column |
| Content Format | Text (in popover) | `Calender` sheet `Content Format` column |
| Platform | Tag/icon | `Calender` sheet `Platform` column |
| Post Type | Tag (in popover) | `Calender` sheet `Post Type` column — confirmed with Sharva (prior conversation) as a pillar-driven post category (e.g. "Thought Leadership," "Case Study"), not a format sub-type |
| Content Link | Link (in popover) | `Calender` sheet `Content Link` column |
| Status | Badge (Scheduled / Live) | `Calender` sheet `Status` column |
| Comments | Text (in popover) | `Calender` sheet `Comments` column |

## CTAs

| Element | Type (button/link/action) | Behavior (what happens / where it goes) |
|---|---|---|
| "Set date" (Unscheduled panel entry) | Action | Opens a date picker; setting a date writes it to the `Calender` row's `Date` column, sets `Status = Scheduled`, and moves the entry from the panel onto the grid. When every channel row for an item has a date, the Tracker's `Overall Status` rolls `Approved → Scheduled` |
| Date-cell entry click | Action | Opens the detail popover |
| "Mark Live" (popover) | Button | Sets that row's `Status = Live` and the corresponding Tracker channel cell `Scheduled → Live` (REQ-012). Prompts for the live post URL to fill `Content Link` |
| Content Link (popover) | Link | Opens the linked URL in a new tab |
| Month navigation | Buttons | Moves the grid forward/back one month; "Today" jumps to the current month |

## Validations

| Field / Action | Rule | Error message |
|---|---|---|
| "Set date" | Date cannot be in the past | "Pick a date from today onward" |
| "Mark Live" | Requires a URL before confirming | "Add the published post's link" |
| Rollup to `Overall Status = Scheduled` | Only fires when every applicable channel row for that item has a date | N/A — automatic |
| Rollup to `Overall Status = Published` | Only fires when every applicable channel row for that item is `Live` | N/A — automatic |

## Conditional States

| State | What the user sees |
|---|---|
| Empty (Unscheduled panel) | Panel is hidden entirely when nothing's waiting on a date |
| Empty (whole month) | Grid renders normally with no entries on any date; no special empty state needed since an empty calendar month is a normal, self-explanatory state |
| Loading | Skeleton grid while the `Calender` sheet loads fresh |
| Error | "Couldn't load the calendar" with retry |
| Past-due Scheduled entry (date has passed, still not marked Live) | Entry renders flagged (amber border/badge) on its date cell — a Scheduled item sitting past its own date with no confirmation it actually went out |

## Open Questions

1. Week view, as an alternative to Month — not scoped for Phase 1 (Month grid covers the observed need), but worth flagging as a natural fast-follow if a month view feels too zoomed-out in practice.
2. Multiple entries stacking in one date cell — at what point (how many posts in one day) does a cell need its own "+N more" overflow treatment rather than just growing taller? Not yet decided; depends on actual publishing cadence, which isn't documented in any observation.
3. Past-due threshold — flag immediately the day after the scheduled date, or allow a small grace period? Not yet decided.
