---
title: "Scope — TL Portal, Phase 1"
status: draft
created: 2026-09-15
updated: 2026-09-15
tags: [scope, sign-off]
---

# Scope — TL Portal, Phase 1

**Date:** 2026-09-15
**Prepared for:** Rohan, Sharva

> This document is self-contained. It references nothing else — everything you need to understand and sign off on is written here.

## What We Are Building

Right now, Jetbro's thought-leadership content — company LinkedIn posts and related content — moves through three separate Google Sheets that Mahima updates by hand: one for screening new ideas, one for tracking production, and one for scheduling. Drafts live in Google Drive. Nothing connects these automatically — every handoff between them is a manual copy-paste step.

This project builds a single web dashboard that sits on top of those same three sheets. It reads and writes them directly, so **the sheets stay the real record** — nothing changes about where the data actually lives, and anyone can still open a sheet directly at any time. The dashboard's job is to remove the manual copying between stages, show the whole pipeline at a glance, and — most importantly — make it visible when something has been sitting untouched too long.

That last point is the direct answer to a real problem: five pieces of content were submitted for review on June 24, 2026, and sat waiting with no decision from either reviewer for over two and a half months, discovered only when someone went looking. Nothing in the current process would have surfaced that on its own. This dashboard is built to prevent that from happening silently again.

## Included

- **Idea intake:** a simple form to log a new content idea, classify it, and mark it ready for production — one action instead of separate steps across a sheet and a document.
- **A production dashboard:** every piece of content currently being worked on, grouped by stage (in production, blocked, ready for review, changes requested), so bottlenecks are visible without opening a spreadsheet.
- **Draft editing:** writing and editing the brief and each channel's version (LinkedIn, X, Instagram, Substack, Website) from the dashboard, with drafts still saved as the same Google Doc-style files as today.
- **A review view for Rohan and Sharva:** everything routed to you for review, in one list, with a visible flag once an item has been waiting longer than its own target turnaround time (same-day for time-sensitive filler content, 2-3 days for standard, matching the timelines already in use). You decide Approved, Changes Requested, or Killed directly from this view. The actual detailed feedback still happens as comments on the draft document, exactly as it does today — this dashboard doesn't change how you give feedback, only how visible a waiting item is.
- **A calendar view:** approved content that still needs a publish date, and a calendar of what's scheduled and what's already gone live, so nothing approved quietly falls through the cracks either.
- **Login restricted to an approved list of people** (starting with Mahima, Rohan, and Sharva) using existing Google accounts — no new passwords to manage.

## Not Included

- **No AI-written content.** Mahima still writes every brief, draft, and channel adaptation herself. This dashboard organizes the work; it doesn't do the work.
- **No new commenting system.** Detailed review feedback continues to happen as comments directly on the draft document, the same way it works today. The dashboard shows you the decision status, not a replacement comment thread.
- **No Slack integration.** Ideas still get logged into the dashboard directly, the same way they'd get written into a sheet today — this version doesn't automatically pull anything in from Slack.
- **Not Rohan's personal LinkedIn content.** This covers the Jetbro company page pipeline only.
- **Not the sales/business-development pipeline.** That's a separate system, not touched by this project.
- **No automatic posting to LinkedIn, X, or any other platform.** Publishing a post is still something a person does by hand on the platform itself; the dashboard is only updated afterward to reflect that it went live.
- **Desktop use only.** No dedicated mobile version in this phase.

## Assumptions & Dependencies

- The dashboard depends on continued access to the same Google account credentials already used to manage the sheets today. If that access changes, the dashboard needs to be updated alongside it.
- The dashboard makes waiting items visible; it does not force anyone to act on them. Its value depends on Rohan and Sharva actually checking the review view regularly, the same way any tool depends on being used.
- **One open decision that could still change how this is built:** whether this dashboard should be built as its own standalone tool, or inside Phlo Hub, Jetbro's existing internal automation platform. This hasn't been resolved yet — it needs a conversation with whoever understands Phlo Hub's capabilities before it's settled. It does not change anything described above as Included or Not Included; it only affects where the tool technically lives.
- This is Phase 1. If it proves useful, a later phase could add things explicitly left out here (like AI-assisted drafting) — but that would be a separate scope, signed off separately, not an automatic next step.

## Sign-Off

By signing below, the stakeholder approves the scope described above.

| Name | Role | Signature | Date |
|---|---|---|---|
| | | | |
| | | | |
| | | | |
