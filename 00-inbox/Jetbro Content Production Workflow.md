# Jetbro Content Production Workflow

**Version:** 2.0 | **Owner:** Mahima | **Last Updated:** June 2026

> **Scope:** This is the **full content pipeline, end to end** — from a raw input arriving, to a published post. It details all three workflows:
>
> `Workflow 1: Ingestion & Screening → Workflow 2: Content Production → Workflow 3: Content Approval`
>
> The Marketing SOP §5 carries the *overview*; **this document is the detailed operating manual.** It supersedes the partial workflow sketches in the Thought Leadership Strategy §8 and Marketing SOP §5.

---

## 0. The Pipeline at a Glance

```
RAW INPUT (any format)
      │
┌─────▼───────────────────────────────────────────────────────────────┐
│ WORKFLOW 1 — INGESTION & SCREENING            (owner: Mahima)         │
│   Ingest → decide "the what" → classify (Pillar + Urgency) → Screened │
│   lives in: Ingestion/ folder + Whats? sheet                          │
└─────┬───────────────────────────────────────────────────────────────┘
      │  Screened = Yes   ──►  create row in Master Content Tracker
┌─────▼───────────────────────────────────────────────────────────────┐
│ WORKFLOW 2 — CONTENT PRODUCTION               (owner: Mahima)         │
│   intake → brief → master draft → self-edit → adapt to every channel  │
│   lives in: Production/ folder + Master Content Tracker               │
└─────┬───────────────────────────────────────────────────────────────┘
      │  Overall = All Ready   ──►  submit for review
┌─────▼───────────────────────────────────────────────────────────────┐
│ WORKFLOW 3 — CONTENT APPROVAL                 (approver: Rohan)       │
│   review vs the bar → approve / request changes / kill                │
│   lives in: Master Content Tracker (+ Drive comments on the draft)    │
└─────┬───────────────────────────────────────────────────────────────┘
      │  Approved   ──►  schedule into Calender → publish
      ▼
   PUBLISHED  (cells: Ready → Scheduled → Live)
```

---

## 1. Shared Foundations (apply across all three workflows)

### 1.1 Where everything lives

| Thing | Location |
|---|---|
| Raw inputs (any format) | `Ingestion/` (Drive) |
| The "what" per item | `Ingestion/idea-<slug>.md` |
| All drafts (master + every channel) + brief | `Production/production-draft-<slug>.md` (one file per item) |
| Screening register | `Whats?` (sheet) |
| Production & readiness | `Master Content Tracker` (sheet) |
| Scheduling & publishing | `Calender` (sheet) |

**The principle:** **sheets are the index/tracking; markdown files hold the actual content.**

### 1.2 The item's home, by phase

An item is a **baton pass**, not three parallel trackers to keep in sync:

| Phase | Owns the item | Notes |
|---|---|---|
| Ingestion → Screening | `Whats?` | Frozen once `Screened = Yes` |
| Production → Readiness | `Master Content Tracker` | The working source of truth from `Screened` onward |
| Scheduling → Publishing | `Calender` | Added only *after* approval |

The **`Item ID`** (`Whats?` `ID #` = Tracker `Item ID`) ties them together so a piece is always traceable without copying data.

### 1.3 Canonical status vocabulary

There is **one** status vocabulary, owned by the Master Content Tracker. The ad-hoc status key in the old `LinkedIn Posts — Draft.md` is **retired**.

**Per-publishing-point readiness** (Tracker channel cells):
`—` (N/A) · `To Do` · `WIP` · `Ready` · `Scheduled` · `Live`

**Item-level** (Tracker `Overall Status`):
`In Production` · `Partially Ready` · `All Ready` · `On Hold` · `In Review` · `Changes Requested` · `Approved` · `Scheduled` · `Published` · `Killed`

### 1.4 Roles across the pipeline

| Who | Role |
|---|---|
| **Mahima** | Runs **Workflow 1 and Workflow 2 end to end** (single inbox, screener, sole producer) and owns all tracker upkeep + scheduling. Submits items for approval in Workflow 3. |
| **Rohan** | **Approver (Workflow 3)** — the quality gate. Also supplies case-study source material on request. |
| **Sharva** | Technical reviewer for engineering-pillar / white-paper content (accuracy). Supplies technical cores on request. Not a producer. |
| **AI agent** (when built) | Assists Mahima with drafting/adaptation in Workflow 2. Never owns a stage; Mahima always edits. |

---

## 2. Workflow 1 — Ingestion & Screening

**Owner:** Mahima. **Purpose:** turn any raw input into a defined, classified, production-ready idea.

### Stage 1 — Ingestion
Raw input — in **any** format (URL, text, voice note, audio, video, Slack dump, meeting notes) — is submitted to **Mahima**, the single inbox. Anyone (team, Rohan, Sharva) can submit. Low friction is the goal so nothing useful is lost. The raw input is stored in the `Ingestion/` folder.

### Stage 2 — The What
Mahima reviews the raw input and decides **what content comes out of it** — one post, several, a case-study angle, or nothing. This is an editorial decision. She then:
- **Creates the item in `Whats?`** — a new row with `ID #` and `Title`.
- **Creates `Ingestion/idea-<slug>.md`** — the articulated "what": title, full description, source, submitted-by. (The full description lives in this markdown; `Whats?` is the index.)

### Stage 3 — Classification
Mahima classifies the idea in `Whats?`:

| Dimension | Options |
|---|---|
| **Pillar** | Pillar 1–5, or `Filler` (combos allowed, e.g. "Pillar 5 / Pillar 3") |
| **Urgency** | `High` (publish 48–72h) · `Medium` (1–2 weeks) · `Low` (evergreen) |

### Stage 4 — Screening Complete
Mahima sets **`Screened = Yes`** in `Whats?`. Nothing proceeds without classification — unclassified ideas are not produced.

> **Handoff #1 (Whats? → Tracker):** marking `Screened = Yes` is the trigger to create the item's row in the **Master Content Tracker**. From here `Whats?` is frozen; the Tracker owns the item. High-urgency items go to the front of the production queue.

---

## 3. Workflow 2 — Content Production

**Owner:** Mahima (sole producer). **Purpose:** turn a Screened idea into finished, channel-adapted drafts, ready for approval.

**The core principle:** one idea → many publishing points. A single master asset is created once, then adapted to each channel. Production is not "done" until **every applicable point** is ready — exactly what the Master Content Tracker measures.

### Stage 1 — Production Intake & Queue
**Trigger:** `Screened = Yes`.
Mahima:
1. **Creates the Tracker row** — carries over `Item ID`, `Title`, `Content Type`, `Pillar`, `Urgency`.
2. **Sets the publishing-point scope** — marks each applicable readiness cell `To Do`, each non-applicable cell `—` (use the distribution tables in Marketing SOP §4 to decide which points apply).
3. **Sets `Overall Status = In Production`.**
4. **Queues by urgency:** High → front (48–72h) · Medium → 1–2 weeks · Low → backlog.

### Stage 2 — Production Brief
Mahima expands the one-line "what" into a brief (kept at the top of the `production-draft-<slug>.md` file):
- **Angle & POV** — which of the 5 Beliefs it's rooted in (Strategy §2). Every piece carries a POV (Ground Rule 1).
- **Audience lens** — business buyer or technical validator (Strategy §3). Company content leans business-impact.
- **The one idea** (Ground Rule 5) and the **closing hook / CTA** (Ground Rule 6).
- **Formats required** — from the publishing-point scope.
- **Source material** — pull the `idea-<slug>.md` from `Ingestion/`; note any additional inputs needed.

**Missing-input rule:** if the piece needs something Mahima doesn't have (e.g. a case-study outcome from Rohan, a technical core from Sharva), set `Overall Status = On Hold`, record the blocker in `Notes`, and request it. Re-enters the queue when the input arrives.

### Stage 3 — Master Draft (the primary asset)
Mahima writes the **master version** — the single best expression of the idea, in its primary format.
- **Where it lives:** **one** markdown file `production-draft-<slug>.md` in `Production/`. The file holds the brief at the top, then a labelled `##` section for **every** distribution point (LinkedIn, X, Instagram, Substack/Newsletter, Website). Write the **master/primary** section here; the rest are filled in Stage 5. Non-applicable points are marked `— (N/A)`. **Each `##` section maps 1:1 to a readiness column in the Tracker.**
- **Link it:** paste the file's Drive link into the Tracker's `Master Draft` column.
- **State:** primary cell → `WIP`.
- **Write against** (reference, don't restate): Tone of Voice (Strategy §5), the 7 Ground Rules (§7), pillar topics (§4), format specs (SOP §6), case-study rules (Strategy §12).

### Stage 4 — Self-edit / Quality Pass
Mahima reviews the master against the 7 Ground Rules + voice **before** adapting — fix a weak draft once, here, not five times across channels. When it passes, the primary cell → `Ready`. (This is Mahima's bar, **not** Rohan's approval.)

### Stage 5 — Channel Adaptation
From the `Ready` master, Mahima produces the adapted version for **each applicable publishing point** — writing each into its `##` section in the same `production-draft-<slug>.md` file — following the distribution tables (SOP §4) and platform rules (Strategy §6a):

| Publishing point | Adaptation |
|---|---|
| **LinkedIn (Jetbro)** | The master post / carousel (usually primary) |
| **X / Twitter** | Strip to sharpest 1–3 lines, or a 5–8 tweet thread |
| **Instagram** | Reformat carousel to square/portrait, or a 60s reel |
| **Substack / Newsletter** | Long-form article, or a newsletter-section blurb |
| **Website** | Case study (jetbro.in/expertise) or gated resource |

- Move each cell `To Do → WIP → Ready` as its version is drafted.
- `Overall Status` rolls up: some applicable cells `Ready` → `Partially Ready`; **all** → `All Ready`.
- **Never adapt verbatim** — reshape per platform (Strategy §6 "what to avoid").

### Stage 6 — Production Complete
When `Overall Status = All Ready`, production is done → the item enters **Workflow 3**.

---

## 4. Workflow 3 — Content Approval

**Approver:** Rohan (the quality gate, per Ground Rule 7 — *nothing goes live without review*). **Purpose:** ensure every piece meets the Jetbro bar before it's scheduled.

**Input:** an item at `Overall Status = All Ready` with every applicable channel section drafted in `production-draft-<slug>.md`.

### Stage 1 — Submit for Review
Mahima:
1. Sets **`Overall Status = In Review`.**
2. **Routes to the reviewer:**
   - **Rohan** — default for all content (business angle, POV, voice).
   - **Sharva** — additionally reviews engineering-pillar pieces and white papers for **technical accuracy** before/alongside Rohan.
3. Points the reviewer to the **single `production-draft-<slug>.md` file** — all channel versions are in one place, so the whole piece is reviewed in one pass.

### Stage 2 — Review Against the Bar
The reviewer checks the piece against:
- The **7 Ground Rules** (Strategy §7) — has a POV, specific over general, leads with business, no unexplained jargon, one idea, ends with a hook.
- **Tone of voice** (Strategy §5).
- **Factual / outcome accuracy** — especially case-study numbers and **anonymisation** (Strategy §12).
- **Per-channel fit** — each adaptation suits its platform (not a verbatim copy).

**Where feedback goes:** Drive **comments on the `production-draft-<slug>.md` file** (in-context, threaded), with a one-line summary in the Tracker `Notes`.

### Stage 3 — Decision
The reviewer returns one of three outcomes:

| Outcome | Set `Overall Status` | What happens next |
|---|---|---|
| **Approved** | `Approved` | Cleared to schedule. Channel cells stay `Ready`. → Stage 4 |
| **Changes requested** | `Changes Requested` | Comments logged on the draft. Item returns to Mahima → Workflow 2 Stage 4/5 to revise → re-submit (back to `In Review`). |
| **Killed** | `Killed` | Not published. Reason noted in `Notes`. Row kept for the record (not deleted). |

**Approval is at the item level** — because every channel version is adapted from the same approved master, one sign-off covers the whole piece. (If Rohan wants a specific channel changed, that's `Changes Requested` on that section.)

### Stage 4 — Handoff to Scheduling
On `Approved`, Mahima schedules the item:

> **Handoff #3 (Tracker → Calender):** the Tracker measures **readiness**; `Calender` owns **scheduling**. Only an `Approved` item is added to `Calender` (one row per scheduled post — LinkedIn Tuesday, X Tuesday, Substack next week, etc.). As each post goes out, its Tracker cell moves `Ready → Scheduled → Live` and `Overall Status` moves `Approved → Scheduled → Published`.

### Approval fast-paths
- **Filler** — still requires Rohan's sign-off, but on a **same-day** expectation (lightweight review, per SOP §5.5).
- **High-urgency / reactive** — compressed review inside the 48–72h window.

### Approval SLA
- Standard pieces: Rohan reviews within **2 business days** of `In Review`.
- Filler: same day. High-urgency: within hours.
- **Backlog discipline:** keep a buffer of `Approved` evergreen items so a slow review week never leaves a calendar gap.

---

## 5. Edge Cases (whole pipeline)

- **On Hold** — missing input; parked with a `Notes` reason, re-queued when unblocked.
- **Killed** — abandoned in production or rejected at approval; mark `Killed` in `Overall Status`, note the reason, keep the row.
- **Single-channel items** — most Filler is LinkedIn/X/IG only; fewer `##` sections, fewer cells.
- **Series pieces** — recurring series (Strategy §11) follow the same stages; note the series name in `Notes`.
- **Revision loops** — `Changes Requested` sends an item back to Workflow 2; it re-enters Workflow 3 as `In Review`. No limit on loops, but each is logged on the draft.

---

*Workflow 1's overview also appears in Marketing SOP §5. Creative guidelines (voice, pillars, ground rules, formats) live in the Thought Leadership Strategy. Execution status lives in the Master Content Tracker; scheduling in the Calender.*
