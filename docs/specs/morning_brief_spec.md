# Morning Brief — Technical Specification

**Status:** **Accepted by Strategist, 2026-07-02** — ready to build, pending Privacy & Governance Reviewer pass before final close (see `NOWH_Decision_Log.md`)
**Author:** NOWH Architect
**Milestone:** First workflow spec (per `NOWH_Strategist_Continuity.md` Immediate Next Steps, `CLAUDE.md` Section 14 Default Build Sequence)
**Depends on:** Nothing built yet. This spec originates the first slice of the NOWH Director Workspace Google Sheet schema.
**Revision (2026-07-02):** Adds Google Calendar read access to Section 3.2 item 4 ("Meetings Needing Prep"), per CEO decision recorded in `NOWH_Decision_Log.md` ("Morning Brief: Calendar read access approved for v1"). Scope: Sections 3, 5(4), 7, 8, 9, 10 only. See those sections for the integration design. This revision does not change the Sheet schema (Section 3.1) and does not touch Gmail, Entities, Touchpoints, Open Loops, or Approval Queue design.

---

## 1. WHAT & WHY

**What:** A Claude Cowork workflow, triggered when the director says something like "Run my morning brief," that reads a small, defined slice of the NOWH Director Workspace Google Sheet, assembles a same-day briefing, writes it as a new Google Doc into a defined Drive location, and gives the director a short conversational summary with a link to the Doc.

**Why this reduces director friction:** A lean nonprofit director starts every day reconstructing context from memory — what's overdue, what needs a decision, what's sensitive, what's waiting on them versus waiting on someone else. Morning Brief replaces that reconstruction with a single command. It is explicitly named as the anchor workflow: four of the other five core workflows (Meeting Prep, Open Loops, Approval Queue, and indirectly Grantee Burden Check) read from the same underlying rows this spec defines, so getting this schema right first prevents rework later.

**Why this protects the mission:** Morning Brief is read-only and internal-facing. It surfaces sensitivity and approval status rather than hiding it, and it never drafts or sends anything external. Its entire value is "tell the director the truth about today, clearly labeled" — consistent with the core product belief in `NOWH_Project_Overview.md` Section 4.

---

## 2. CONTEXT INTAKE

**What exists today:** Nothing. `NOWH_Current_Status.md` and `NOWH_Data_Schema.md` both confirm no Sheet, no Docs templates, and no Drive structure exist yet. Column-level schema has never been designed for any tab. This spec is the first one written.

**What changes:** This spec originates:
- The **NOWH Director Workspace** Google Sheet, with four new tabs (Entities, Touchpoints, Open Loops, Approval Queue) and one new tab (Daily Brief Items) — the minimum slice Morning Brief needs.
- One new Google Doc template: **Morning Brief**.
- One new Drive folder path: `NOWH Workspace/Briefs/`.
- One new Claude Cowork command surface: "Run my morning brief" (+ close variants).

**What stays the same:** Nothing yet exists to preserve — there is no migration of prior director data. (See Section 4, Migration.)

**Which existing patterns are reused/extended:** The tab names (Entities, Touchpoints, Open Loops, Approval Queue, Daily Brief Items) are taken directly from the *recommended* tab list in `NOWH_Project_Overview.md` Section 12 and `NOWH_Data_Schema.md` — this spec is the first to actually assign columns, types, and sensitivity to those names rather than inventing new ones.

**New pattern introduced, and why:** The task brief suggested Morning Brief might reference **Grantees**, **Funders/Donors**, and **Board & Founder** as separate tabs. This spec instead originates a single lightweight **Entities** tab as the universal lookup/join table (id, name, type, status, sensitivity) rather than three separate relationship-management tabs. Rationale:
- `NOWH_Project_Overview.md` Section 12 already lists **Entities** as tab 1, distinct from Grantees and Funders/Donors as tabs 2–3 — implying Entities is meant to be the thin universal layer, with richer per-type tabs layered on later by workflows that actually need per-type depth (e.g., Grantee Burden Check needs much more grantee-specific history than Morning Brief does).
- Morning Brief only needs to *resolve a name and flag sensitivity* for anything referenced from Open Loops/Touchpoints/Approval Queue — it does not need grant amounts, funder giving history, or board bios. Building three full relationship tabs now would violate the Architect charter's anti-bloat rule ("prefer the smallest design that satisfies the approved requirement").
- This is flagged explicitly in Open Questions (Section 10) since it's a divergence from the task brief's suggested shape, and the Strategist should confirm before Grantees/Funders-Donors/Board & Founder are built out in full for later workflows.

---

## 3. DATA & STRUCTURE DESIGN

### 3.1 Google Sheet: NOWH Director Workspace

Five tabs originate from this spec. All are additive — this is a brand-new Sheet, so there is nothing to preserve elsewhere in it yet.

#### Tab: `Entities`

Universal lightweight lookup/join table for anything Morning Brief (and later workflows) needs to reference by name. Not a full CRM record — deliberately thin.

| Column | Type | Required | Sensitivity | Valid Values / Notes |
|---|---|---|---|---|
| Entity ID | Text (short ID, e.g. `E-001`) | Yes | Low | Unique. Referenced by Touchpoints, Open Loops, Approval Queue via this ID. |
| Display Name | Text | Yes | Low | e.g. "Riverbend Youth Collective," "Jordan Lin (Board Chair)." Used in briefs. |
| Entity Type | Enum | Yes | Low | `Grantee`, `Funder`, `Donor`, `Board Member`, `Founder`, `Staff`, `Vendor/Partner`, `Other` |
| Status | Enum | Yes | Low | `Active`, `Inactive`, `Prospective` — factual state only, never a performance judgment |
| Default Sensitivity | Enum | Yes | Medium | `Low`, `Medium`, `High`, `Restricted` — the floor sensitivity applied to any brief item referencing this entity unless the specific row overrides it higher (never lower). Board Member/Founder/Donor rows should default High or Restricted per `NOWH_Privacy_and_Governance.md`. |
| Notes | Text (free text) | No | High | Short internal note only. Never surfaced verbatim in an external-facing draft — Morning Brief is internal, so this may appear in the brief body but always under an "internal note" heading, never rephrased as if approved language. |

**Explicitly not included** (deferred to later workflows/tabs, per anti-bloat rule): grant amounts, funder giving history, board term dates, contact information, consent status for stories. These belong to Grantees / Funders-Donors / Board & Founder tabs when those workflows are specced (Grantee Burden Check, Funder Stewardship, Board/Founder Prep). Noted in Open Questions.

#### Tab: `Touchpoints`

Record of contact events. Feeds "last touchpoint," "funder/grantee relationship needs attention," and care prompts.

| Column | Type | Required | Sensitivity | Valid Values / Notes |
|---|---|---|---|---|
| Touchpoint ID | Text (short ID) | Yes | Low | Unique. |
| Entity ID | Text (FK → Entities) | Yes | Low | Which relationship this touchpoint belongs to. |
| Date | Date | Yes | Low | Date of contact. |
| Type | Enum | Yes | Low | `Email`, `Call`, `Meeting`, `Site Visit`, `Written Update`, `Other` |
| Summary | Text (free text, short) | Yes | High | Factual one-line summary only, e.g. "Discussed Q3 report timeline." No editorializing, no scoring language. |
| Initiated By | Enum | Yes | Low | `Director`, `Entity`, `Staff` — used to compute "waiting on someone" logic without judgment framing. |
| Sensitivity Override | Enum | No | — | `Low`, `Medium`, `High`, `Restricted` — only set if this specific touchpoint is more sensitive than the entity's default (e.g., a hard conversation with a board member). Never used to lower sensitivity below the entity default. |

#### Tab: `Open Loops`

Tracks commitments, follow-ups, and deadlines. Directly feeds Morning Brief's "open loops," "overdue," and "can wait" sections, and is the primary shared tab with the future standalone Open Loops workflow.

| Column | Type | Required | Sensitivity | Valid Values / Notes |
|---|---|---|---|---|
| Loop ID | Text (short ID) | Yes | Low | Unique. |
| Entity ID | Text (FK → Entities) | No | Low | Optional — some loops are purely internal (e.g., "file 990") with no external party. |
| Description | Text (free text, short) | Yes | Medium | e.g. "Send revised budget to funder." Factual, action-oriented. |
| Type | Enum | Yes | Low | `Promise Made`, `Follow-Up Due`, `Waiting On Someone`, `Internal Task`, `Deadline` |
| Owner | Enum | Yes | Low | `Director`, `Staff`, `Waiting on Entity` — who the ball is with right now. |
| Due Date | Date | No | Low | May be blank for open-ended commitments. |
| Status | Enum | Yes | Low | `Open`, `In Progress`, `Waiting`, `Done`, `Dropped` — Morning Brief only surfaces `Open`, `In Progress`, `Waiting`. |
| Urgency Signal | Enum | Yes | Low | `Overdue`, `Due Today`, `Due Soon`, `No Deadline` — Morning Brief computes this from Due Date at read time; stored value is a director/system-set override for cases where date math doesn't capture true urgency (e.g., "technically not due yet but funder is anxious"). |
| Can Wait | Boolean (checkbox) | No | Low | Director-settable flag to explicitly deprioritize an item without deleting it — supports the "what can wait" brief section without requiring the director to justify inaction. |
| Related Sensitivity | Enum | No | — | Inherits Entity default if blank; may be raised (never lowered) for this specific loop. |

#### Tab: `Approval Queue`

Items requiring explicit director approval before any action. Morning Brief surfaces these but never resolves them — resolution is out of scope for this workflow (belongs to the Approval Queue workflow itself).

| Column | Type | Required | Sensitivity | Valid Values / Notes |
|---|---|---|---|---|
| Approval ID | Text (short ID) | Yes | Low | Unique. |
| Entity ID | Text (FK → Entities) | No | Low | The party the action concerns, if any (blank for purely internal items like a compliance filing). |
| Action Type | Enum | Yes | High | `Funding Decision`, `Grant Renewal`, `Spending`, `Funder Message`, `Donor Message`, `Grantee Message`, `Board Material`, `Founder Use`, `Public Narrative`, `Youth/Community Story`, `Sensitive Relationship Note`, `Compliance Action` — matches the locked approval-required list in `CLAUDE.md` Section 11 and `NOWH_Privacy_and_Governance.md` exactly. |
| Description | Text (free text, short) | Yes | High | What's being requested, factually. |
| Status | Enum | Yes | Low | `Pending`, `Approved`, `Rejected`, `Needs Revision` — Morning Brief only surfaces `Pending` and `Needs Revision`. |
| Requested Date | Date | Yes | Low | When it entered the queue. |
| Sensitivity | Enum | Yes | Restricted (default floor) | `High` or `Restricted` only — nothing in this tab should ever be Low/Medium by definition of what belongs here. |

#### Tab: `Daily Brief Items`

A landing tab for one-off items that don't fit Touchpoints/Open Loops/Approval Queue cleanly — e.g., a director-added note like "remember Board Chair's father is ill" or a system-detected item that doesn't map to an existing structured row yet. Optional-use tab; Morning Brief must function correctly even if it's empty.

| Column | Type | Required | Sensitivity | Valid Values / Notes |
|---|---|---|---|---|
| Item ID | Text (short ID) | Yes | Low | Unique. |
| Date Added | Date | Yes | Low | Defaults to today when the director adds a row. |
| Entity ID | Text (FK → Entities) | No | Low | Optional link. |
| Note | Text (free text) | Yes | High | Free-form. Treated as High by default per "when in doubt, classify higher" (`NOWH_Privacy_and_Governance.md`). |
| Surface Until | Date | No | Low | If set, Morning Brief stops surfacing this item after this date (self-expiring reminders). Blank = surface every brief until manually removed. |
| Sensitivity Override | Enum | No | — | `Low`, `Medium`, `Restricted` — director may lower from the High default only for this tab, since it's director-authored free text they control directly (the one tab where the director is the sole author, so they are in the best position to self-classify). All other tabs' overrides may only raise, never lower. |

### 3.2 Google Docs template: Morning Brief

New Doc created fresh each run (not overwritten) with this section order:

1. **Header** — director name, date, generation timestamp.
2. **Decisions Needed Today** — from Approval Queue (`Pending`/`Needs Revision`) + Open Loops where Type = `Deadline` and Urgency = `Overdue`/`Due Today`.
3. **Responses Needed Today** — Open Loops where Owner = `Director` and Urgency = `Overdue`/`Due Today`.
4. **Meetings Needing Prep** — reads today's events live from the director's **primary Google Calendar only** (not any secondary or shared calendar the director can see) at brief-generation time. No new Sheet column or tab is introduced for this — see **3.2.1** below for why this is a live read, not a persisted field, and exactly what's read.
5. **Sensitive Items to Be Aware Of** — anything from any tab at High/Restricted sensitivity surfaced elsewhere in the brief, consolidated as a visibility list (not new content — see Section 8).
6. **Open Loops** — remaining `Open`/`In Progress`/`Waiting` items not already surfaced above, grouped by Urgency Signal.
7. **Funder / Grantee Relationships Needing Attention** — Entities with no Touchpoint in a director-configurable staleness window (default 30 days for Funders/Donors, 45 days for Grantees — see Open Questions), framed per the care-based language rule (Section 6).
8. **Items That Can Wait** — Open Loops flagged `Can Wait`.
9. **Footer** — link back to the Sheet, and a one-line reminder: "This brief is internal only. Nothing here has been approved for external use."

### 3.2.1 Calendar integration for "Meetings Needing Prep"

**No new Sheet column or tab.** Calendar data is read live from the director's primary Google Calendar at brief-generation time and is never written back to the Sheet, never persisted anywhere by NOWH, and never stored between runs. This is a deliberate divergence from this Sheet's normal pattern (Section 3.1, where everything durable lives in a tab) — justified below.

**Why live-read instead of a Sheet column:**
- `NOWH_Data_Schema.md`'s "Gmail & Calendar" section already frames both as "live work context," not data NOWH owns or stores — a persisted Calendar column would contradict that existing framing, not extend it.
- Persisting event data (even just title/time) into the Sheet would require NOWH to keep it in sync with the live Calendar (handling edits, cancellations, reschedules) — real complexity with no benefit, since the brief only needs "today's meetings, as of the moment the brief runs." A stale copy is strictly worse than a live read for a same-day workflow.
- Per the Architect charter's anti-bloat rule, the smallest design that satisfies "flag that prep is needed and by when" is a same-run read that's discarded after the Doc is generated — not a new durable structure.
- This also means a director who has never manually logged a Touchpoint still gets a correct "Meetings Needing Prep" section — removing exactly the friction the CEO decision was made to remove (Decision Log, 2026-07-02).

**What continues to use the Sheet:** Touchpoints rows with `Type = Meeting` are **not removed** from the schema — they remain valid for meetings the director logs after the fact (a record of a meeting that happened) or for historical/non-Calendar meetings (e.g., an informal hallway conversation with no Calendar event). This section's read logic is additive: it surfaces Calendar events for today, and continues to reflect any Touchpoints-based meeting record as before. The two do not need to be reconciled or deduplicated against each other for v1 — see Edge Cases.

**Calendar scope:**
- **Which calendar:** the director's **primary calendar only.** Not secondary calendars, not calendars shared with the director by someone else, not calendars the director can see but doesn't own. This matches the CEO decision's explicit instruction ("do not scope to all calendars they can see") and the Privacy & Governance principle of preferring the narrowest source sufficient for the purpose.
- **Which fields:** **event title and start/end time only.** No attendee list, no event description, no location, no attached documents/links, no conferencing details.
  - **Why title + time is sufficient:** the section's stated job (Section 3.2 item 4, unchanged by this revision) is to flag *that* prep is needed and *by when* — a time-bounded to-do signal, not a briefing. The event's title supplies the "what" a director needs to recognize which meeting it is (e.g., "Board Finance Committee," "Riverbend Youth Collective check-in"); start/end time supplies the "by when." Neither requires knowing who's attending or what the event description says.
  - **Why attendees are excluded:** an attendee list is a set of email addresses/names — PII belonging to people who did not consent to being read by NOWH just by being invited to a meeting on the director's calendar. It also risks reintroducing exactly the Entity-matching ambiguity this spec already avoids elsewhere (Section 7 addresses this directly: an attendee with no matching Entity is not something this design needs to resolve, because attendees are never read in the first place).
  - **Why descriptions are excluded:** per this revision's own instructions and `NOWH_Privacy_and_Governance.md`, description fields can contain highly sensitive content (a director might paste sensitive prep notes, a personal situation, or Restricted-level context into a Calendar event body). Reading descriptions would mean NOWH ingesting arbitrary, unclassified free text with no sensitivity floor — a direct violation of "collect only what is operationally useful" and "when in doubt, classify higher." Full meeting prep content remains explicitly out of scope for Morning Brief (unchanged from the original spec) and is Meeting Prep workflow's job if and when that workflow is specced with its own, more deliberate access design.
  - **Sensitivity classification of what is read:** event title and time are treated as **Medium** sensitivity by default (relationship/coordination context, per the Sensitivity Levels definition in `NOWH_Privacy_and_Governance.md`), consistent with how a Touchpoints row's `Type = Meeting` would already be classified. If a title alone appears to reveal something more sensitive (see Section 7's edge case on sensitive titles), it is surfaced at High per "when in doubt, classify higher," not Medium.

### 3.3 Google Drive structure

| Folder | New/Existing | Notes |
|---|---|---|
| `NOWH Workspace/` | New (root) | Created on first run if it doesn't exist. |
| `NOWH Workspace/Briefs/` | New | Morning Brief Docs are saved here, named `Morning Brief — YYYY-MM-DD`. |

No permission changes beyond whatever the director's own Google account default is — NOWH does not set sharing permissions on generated Docs (see Open Questions on whether this needs to be explicit).

---

## 4. MIGRATION

None needed. No NOWH Director Workspace Sheet exists yet for any director — this spec originates the Sheet's first tabs from nothing. There is no prior data shape to transform and no risk of breaking a director's live data, because no director has live data yet.

For future spec authors extending this Sheet: per the Architect charter's "additive by default" rule, subsequent workflows should add new tabs/columns rather than modify these five. If a future workflow needs to change a column defined here (e.g., Grantee Burden Check needing more structure on `Entities`), that workflow's spec must include a Migration section describing the exact transformation for any director who has been using Morning Brief in the interim.

---

## 5. PROPAGATION

1. **Google Sheet schema** — Originates Entities, Touchpoints, Open Loops, Approval Queue, Daily Brief Items tabs as specified in Section 3.1. This is the first schema written; all five tabs are new.
2. **Google Docs templates** — Originates the Morning Brief Doc template (Section 3.2). No existing templates to change.
3. **Google Drive structure** — Originates `NOWH Workspace/` and `NOWH Workspace/Briefs/` (Section 3.3). No existing structure to change.
4. **Gmail / Calendar conventions** — Gmail: none used by this workflow, unchanged. Calendar: Morning Brief now reads the director's **primary Google Calendar only** (read-only access; NOWH never creates, edits, or deletes Calendar events) for today's events, to populate "Meetings Needing Prep" (Section 3.2 item 4 / 3.2.1). Fields read: event title, start time, end time. Not read: attendees, description, location, attachments, conferencing links. No new Calendar labels or event-naming conventions are introduced — this is a read-only integration against however the director already uses their Calendar, so it imposes no new convention the director must learn or maintain. No data is written back to Calendar or persisted to the Sheet (see 3.2.1). This closes the tradeoff previously flagged in Open Question 3 — see Section 10.
5. **Claude Cowork command surface** — New trigger phrases: "Run my morning brief," "What matters today," "Give me today's brief" (close variants of the locked example in `CLAUDE.md`/`NOWH_Architecture.md`: *"Run my morning brief."*). Returns: a short conversational summary (top 3–5 items across Decisions/Responses/Sensitive) plus a Drive link to the full Doc. Invites follow-up: "Want me to prepare you for [meeting]?" / "Want to see the full Approval Queue?" / "Want to check grantee burden before I draft anything?" — i.e., it should conversationally hand off toward the other five workflows rather than trying to do their jobs inline.
6. **Privacy & sensitivity labeling** — Every column in Section 3.1 has a stated sensitivity level. The Doc itself is internal-only by definition (Section 8) — no column value is copied into external-facing language by this workflow.
7. **Approval gates** — Morning Brief surfaces Approval Queue items; it never changes their Status. It cannot approve, reject, draft, or send anything. This is a hard boundary, not a soft default (see Section 8).
8. **Plugin packaging** — Not designed here (out of scope per task). Note for the Packaging spec when it's written: the plugin's command list must include the Morning Brief trigger phrases from item 5, and the setup flow must ensure the Sheet has these five tabs (or the workflow must gracefully handle a Sheet missing them — see Edge Cases) before "Run my morning brief" is offered as a working command.
9. **Demo seed data** — The synthetic demo dataset (not yet built) will need sample rows in all five tabs producing at least one item per brief section (a decision, a response due today, a meeting today, a sensitive item, a stale funder relationship, a can-wait item) so the demo doesn't show an empty or sparse brief. Flagged for whoever builds demo seed data next — not built as part of this spec.

---

## 6. DIRECTOR FRICTION IMPACT

**Current flow (today, no NOWH):** Director manually checks email, a notebook or task list, memory of promises made, and a mental model of who's waiting on what — every morning, from scratch.

**New flow:** Director opens Claude Cowork, types "Run my morning brief," gets a conversational summary in seconds plus a saved Doc they can reference through the day.

**Friction reduced:**
- No manual reconstruction of "what's overdue" or "what's waiting on me."
- No need to remember which relationships have gone quiet.
- One phrase instead of checking five mental categories.

**Friction introduced:**
- The director must populate five new Sheet tabs for the brief to have anything to say. A brand-new director with an empty Sheet gets an empty (not broken) brief — see Edge Cases.
- The director must learn a small set of enum values (Status, Urgency Signal, Type, etc.) if they want to hand-edit the Sheet directly rather than letting NOWH populate it. **Mitigation:** enums are implemented as Google Sheets data-validation dropdowns, so the director picks from a list rather than free-typing values that could break the workflow. This is a Sheet-setup detail for the implementer, not left to convention.
- The `Can Wait` checkbox and `Surface Until` date are optional director actions with no immediate payoff — they only pay off days later. **Mitigation:** the brief's "Items That Can Wait" and self-expiring Daily Brief Items sections exist specifically to make that investment visible next time, so the director sees the tool honoring what they told it.

**Grantee/funder/board burden check:** This workflow adds zero burden to grantees, funders, or board members — it reads only what the director or staff has already entered into the director's own Sheet. It asks nothing of any external party. It also does not create new data-entry work for grantees (no report requests, no forms) — consistent with the Grantee-Centered Rules in `NOWH_Project_Overview.md` Section 11.

---

## 7. EDGE CASES

- **Empty Sheet / brand-new director:** All five tabs exist (created by setup) but contain zero data rows. Morning Brief must still run successfully and produce a Doc that says, plainly, something like "Nothing is tracked yet — as you and your team use the Sheet or NOWH, your morning brief will start surfacing what matters." It must not error, and must not fabricate placeholder content.
- **Sheet tabs missing entirely** (director or someone else deleted a tab, or setup didn't finish): Morning Brief must detect the missing tab and tell the director specifically which tab is missing and that it can't complete that section, rather than failing the whole brief silently or crashing.
- **Grantee/funder goes silent (no Touchpoint in the staleness window):** Surfaced under "Relationships Needing Attention" using care-based framing only — never "this grantee is unresponsive" or similar judgment language. Matches the locked framing rule directly.
- **Conflicting information between Sheet and reality** (e.g., an Open Loop marked `Done` but the director knows it isn't): Out of this workflow's control — Morning Brief reflects what the Sheet says, not ground truth. The brief's footer/intro should include a one-line reminder that it reflects the Sheet as of generation time, so the director doesn't over-trust it as infallible. Correcting the Sheet is the director's action, not something Morning Brief attempts to infer or auto-correct.
- **Director hand-edits the Sheet in an unexpected way** (e.g., types free text into an enum column instead of using the dropdown): Morning Brief should treat an unrecognized value defensively — surface the row under its best-guess category if one is clearly implied, otherwise place it in a small "Needs Review" note at the end of the brief rather than silently dropping it or crashing.
- **Board material needs last-minute revision:** This is Board/Founder Prep territory, not Morning Brief's job to resolve — but if there's an Approval Queue row with Action Type `Board Material` and Status `Needs Revision` due today, it must appear under "Decisions Needed Today," since that's exactly the kind of quietly-urgent item Morning Brief exists to surface.
- **Consent not yet given for a youth/community story:** If such an item exists in Approval Queue (Action Type `Youth/Community Story`) and Status is `Pending`, it is surfaced as an approval item like any other — Morning Brief does not need to know the specific consent mechanics (that's Narrative & Consent's job, deferred), it only needs to flag that something youth/community-related is pending and Restricted.
- **Duplicate or near-duplicate rows** (e.g., director adds the same Open Loop twice): Out of scope to de-duplicate intelligently in v1 — both surface. Flagged in Open Questions as a possible future refinement, not a blocker.
- **Multiple directors / shared Sheet:** Out of scope for this spec — NOWH's current locked scope is single-director use per foundation. If a co-director or staff member edits the Sheet, their edits are treated the same as the director's own (no per-editor distinction exists in this schema). Flagged in Open Questions.
- **Time zone / "today" ambiguity:** "Due Today" and "Overdue" must be computed against the director's local date, not server/UTC date, to avoid an item silently sliding a day early/late. Implementation detail, but stated here since it's a real correctness edge case, not just bad input.

**Calendar-specific edge cases (added in this revision):**

- **No matching Entity for an attendee:** Does not apply — attendees are never read (Section 3.2.1). An event title may *reference* an entity by name in free text (e.g., "Call with Riverbend Youth Collective"), but Morning Brief does not attempt to parse or match event titles against Entity rows in v1. The event surfaces under "Meetings Needing Prep" as-is, with no Entity linkage, no lookup, and no attempt to guess. This keeps the integration simple and avoids incorrect auto-linking of a sensitive relationship to the wrong Calendar event.
- **Recurring events:** A recurring event occurring today (e.g., a weekly staff check-in) surfaces once per brief, same as any other event today — it is not specially flagged as recurring, deduplicated across days, or suppressed after the first occurrence. Recurring-ness is not tracked by this design.
- **Declined or tentative events:** An event the director has marked "Declined" is excluded — it is not something the director needs to prep for. An event marked "Tentative" (not yet accepted/declined) is **included**, on the reasoning that an unresolved tentative meeting today is exactly the kind of ambiguous, easy-to-miss item Morning Brief exists to surface — but it is labeled "(tentative)" next to the event title in the brief so the director isn't misled into thinking it's confirmed.
- **Multiple calendars:** Only the director's primary calendar is read (Section 3.2.1). If the director has events on a secondary or shared calendar, those do not appear in "Meetings Needing Prep." This is a known, deliberate limitation, not a bug — the brief's footer language (see Section 3.2 item 9) already sets the expectation that the brief reflects a defined data slice, not the director's full reality. No change needed to that footer language for this to hold true here as well.
- **Calendar access revoked or not yet granted:** If Calendar access has not been granted, or has been revoked since the last run, "Meetings Needing Prep" must degrade gracefully — the section states plainly that Calendar isn't connected (e.g., "Calendar isn't connected yet, so today's meetings aren't shown here — connect it in setup to enable this.") rather than silently showing an empty section (which would read as "no meetings today" and could be misleading) or crashing the whole brief. This mirrors the existing handling for a missing Sheet tab (see the edge case above) — a missing data source is named, not hidden.
- **Sensitive event title:** An event title that references a personal situation (e.g., "Jordan — check in re: father's illness") is Calendar data the director themselves created, not data NOWH generated — but it can still be highly sensitive. Per Section 3.2.1's sensitivity classification, any title that reads as more sensitive than routine relationship/coordination context is surfaced at **High**, not Medium, and is included in the "Sensitive Items to Be Aware Of" rollup (Section 3.2 item 5) exactly as any other High-sensitivity item would be — not filtered out, not rewritten, not summarized down to something vaguer, since the brief is internal-only and altering director-authored text risks losing meaning. See Section 8 for the fuller risk/prevention treatment of this case.

---

## 8. PRIVACY & SENSITIVITY CONTROL

**How sensitivity is enforced here:**
- Every column across all five tabs is assigned an explicit sensitivity level (Section 3), following "when in doubt, classify higher."
- Entities carries a `Default Sensitivity` floor; Touchpoints/Open Loops/Daily Brief Items may only *raise* that floor via their override columns, never lower it — except Daily Brief Items' director-authored Note, which the director may self-classify downward since they are the sole author of that field (explicitly called out in Section 3.1 as the one exception, so it isn't accidentally read as a general pattern).
- Approval Queue is High/Restricted by definition — nothing in that tab is ever treated as Low/Medium regardless of content.

**What could leak if built carelessly, and how this design prevents it:**
- **Risk:** Internal notes (Entities `Notes`, Touchpoints `Summary`, Daily Brief Items `Note`) get pasted verbatim into something that looks like director-approved external language later, once Funder Stewardship / Board-Founder Prep / Narrative & Consent workflows are built. **Prevention:** Morning Brief's Doc template (Section 3.2) explicitly labels its own footer as "internal only... nothing here has been approved for external use," and every section is presented as internal briefing content, never as draft external copy. This spec does not create any external-facing artifact — Morning Brief has no send/share/draft-for-approval function at all (see Section 5, item 7). The distinction between internal notes and approved external language, required by `NOWH_Project_Overview.md` Section 8, is preserved by Morning Brief simply never crossing that line in the first place.
- **Risk:** A Restricted item (e.g., youth/community story consent pending) gets flattened into a generic bullet point that loses its sensitivity marking by the time it reaches the brief. **Prevention:** Section 3.2's "Sensitive Items to Be Aware Of" section exists specifically to consolidate and re-surface every High/Restricted item that appeared elsewhere in the brief, so sensitivity is never diluted by being folded into a routine-looking list (e.g., "Open Loops" won't quietly contain an unflagged Restricted item without it also appearing in the sensitive-items rollup).
- **Risk:** Morning Brief scans Gmail broadly to infer touchpoints, exposing far more than the director intended. **Prevention:** This spec explicitly scopes Morning Brief to read only the Sheet plus the narrow Calendar surface described below (Section 5, item 4) — no Gmail access at all, consistent with the "prefer narrower labeled sources over broad inbox searches" rule in `NOWH_Privacy_and_Governance.md`. (Note: this bullet originally also excluded Calendar; that's superseded by the CEO decision below — see the Calendar-specific risk block that follows.)
- **Risk:** The generated Doc inherits open/public Drive sharing by accident. **Prevention:** Not fully resolved by this spec — flagged as Open Question below, since Drive default sharing behavior depends on the director's Google account settings, which this spec cannot control but should account for.
- **No scoring/ranking risk:** Every enum in this schema (Status, Urgency Signal, Owner, Type) is a factual state descriptor, not a judgment. There is no "health score," "risk score," or ranking field anywhere in this design — checked directly against the locked rule in `CLAUDE.md` Section 10/17 and `NOWH_Privacy_and_Governance.md`.

**Calendar access surface (added in this revision):**
- **Risk:** Once Calendar read access exists at all, a future workflow or a careless implementation change could quietly widen it — reading secondary calendars, reading attendees "while we're in there," or reading descriptions to make the brief "more useful." This is exactly how a narrow, justified access grant creeps into a broad one over time. **Prevention:** This spec pins the scope explicitly and narrowly (primary calendar only; title + start/end time only) and states the justification for each field individually (3.2.1), not just as a bundle — so a future change that wants to add attendees or descriptions has to argue against a specific, written rationale for exclusion, not just against a vague "we didn't need it before." Any such widening is a new access-surface decision and must be escalated the same way this one was (CEO-level, per `CLAUDE.md` Section 26 condition 6), not folded silently into a later Architect spec.
- **Risk:** Reading Calendar at all — even narrowly — is a broader data source than the Sheet, and broad-source reads are exactly what `NOWH_Privacy_and_Governance.md` warns against ("do not scan broad data sources when narrower labeled sources are enough"). **Prevention:** The CEO decision (Decision Log, 2026-07-02) explicitly weighed this tradeoff and accepted it in exchange for removing real director friction (manually logging every meeting as a Touchpoint before each brief). This spec keeps the access as narrow as the approved tradeoff allows — one calendar, two fields — rather than treating the CEO's approval as blanket permission to read more. The "narrower labeled sources" principle is honored within the approved surface, not overridden by it.
- **Risk:** A sensitive event title (Section 7) flows into the brief verbatim, and because it's Calendar data rather than a Sheet row, it could be treated as "less governed" than Sheet data and slip past the sensitivity rollup by oversight. **Prevention:** Section 3.2.1 explicitly assigns event titles a sensitivity level (Medium default, High when the content warrants it) using the same scale as every Sheet column, and Section 7 requires sensitive titles to flow into the existing "Sensitive Items to Be Aware Of" rollup exactly like any other High-sensitivity item. Calendar data is not treated as a separate, lesser-governed category — it is classified and routed through the same sensitivity machinery as Sheet data, even though it isn't stored in the Sheet.
- **Risk:** Because Calendar events are read live and never stored, there is a temptation to treat them as exempt from sensitivity governance ("it's not persisted, so it doesn't matter"). **Prevention:** Not persisting the data reduces *storage* risk, not *exposure* risk — the event title still appears in the brief Doc, which is itself a durable artifact saved to Drive (Section 3.3). This spec treats "read live, not stored separately" as a data-minimization choice, not a governance exemption; the sensitivity and approval-gate rules in this section apply to the brief Doc's contents regardless of where each item originated.

---

## 9. ACCEPTANCE TESTS

1. Saying "Run my morning brief" with all five tabs empty produces a Doc and a conversational reply, with no error, stating plainly that nothing is tracked yet.
2. Adding one row to Open Loops (`Type = Deadline`, `Due Date = today`, `Status = Open`) and running the brief again causes that item to appear under "Decisions Needed Today" or "Responses Needed Today" as appropriate, without any other setup step.
3. An Approval Queue row with `Status = Pending` appears in the brief; an Approval Queue row with `Status = Approved` does not.
4. A Touchpoints row for a `Funder` entity dated 45 days ago (past the default staleness window) causes that funder to appear under "Relationships Needing Attention," phrased as a care-based prompt (e.g., "Consider a low-burden check-in"), never as a performance judgment.
5. An Open Loops row with `Can Wait` checked appears only under "Items That Can Wait," not duplicated under the main Open Loops section.
6. A Restricted-sensitivity Approval Queue row (e.g., `Action Type = Youth/Community Story`) appears both in its normal section and in the "Sensitive Items to Be Aware Of" rollup — it is never silently omitted from the sensitivity rollup.
7. Manually editing the Sheet — e.g., changing an Open Loop's `Status` from `Open` to `Done` by hand — and re-running the brief reflects that edit immediately, with no separate sync or setup step (this is the illustrative test named directly in the Architect charter).
8. Deleting the `Approval Queue` tab entirely and running the brief produces a Doc that clearly states the Approval Queue section could not be generated and why, rather than crashing or silently omitting the section without explanation.
9. Running the brief does not modify, approve, reject, send, or share anything — every Sheet row's Status is unchanged after a brief runs, and no email/message is sent as a side effect.
10. Two consecutive runs on the same day produce two distinct Docs (not an overwrite), each named with that day's date, so the director retains a same-day history if they run it more than once.
11. **(Added in this revision)** With Calendar access granted and one event on the director's primary calendar today (e.g., "Funder Check-In," 2:00–2:30pm), running the brief surfaces that event under "Meetings Needing Prep" with its title and time, without the director having logged any corresponding Touchpoints row — proving the live Calendar read works independently of the Sheet-based fallback.
12. **(Added in this revision)** With Calendar access not yet granted (or revoked), running the brief still succeeds end-to-end — it produces a Doc and a conversational reply, "Meetings Needing Prep" plainly states that Calendar isn't connected rather than showing an empty section or an error, and every other section of the brief is unaffected — proving the integration fails gracefully rather than blocking the whole workflow.

---

## 10. OPEN QUESTIONS

1. ~~**Entities vs. three separate relationship tabs**~~ **RESOLVED (Strategist, 2026-07-02): accepted as designed.** No locked decision violated, privacy-conservative, correctly anti-bloat. Later workflows (Grantee Burden Check, Funder Stewardship, Board/Founder Prep) must extend `Entities` via Entity ID join, not redesign it — this is now the intentional pattern, confirmed.
2. ~~**Staleness window values**~~ **RESOLVED (Strategist, 2026-07-02): accepted 30/45 days as placeholder defaults.** Config-like, cheap to tune later, not worth CEO time now.
3. ~~**Calendar integration**~~ **RESOLVED (CEO, 2026-07-02): Calendar read access approved.** See Sections 3.2/3.2.1, 5(4), 7, 8, and 9 for the resulting design (primary calendar only, title + start/end time only, live read never persisted to the Sheet).
4. ~~**Drive sharing permissions on generated Docs**~~ **RESOLVED (Strategist, 2026-07-02): deferred to the Privacy & Governance Reviewer's first pass**, not decided now. Added to that agent's queue once built.
5. **Multiple directors / staff editing the same Sheet:** Confirmed out of current locked scope (single-director use) — no new decision needed. Remains flagged in case the Jeremy Lin Foundation reference case needs staff-level access before a per-editor model exists.
6. ~~**No "operational reality" document exists yet**~~ **RESOLVED (Strategist, 2026-07-02): deferred.** Not commissioning `NOWH_Operational_Reality.md` yet — only if Meeting Prep or Grantee Burden Check specs surface recurring failure patterns that make re-deriving edge cases from scratch each time genuinely costly. Tracked in `NOWH_Backlog.md`.
7. ~~**Duplicate row handling**~~ **RESOLVED (Strategist, 2026-07-02): accepted as out of scope for v1**, no de-duplication logic required for the demo.
8. ~~**Command phrase variants**~~ **RESOLVED (Strategist, 2026-07-02): accepted as proposed** — "Run my morning brief," "What matters today," "Give me today's brief" are the locked v1 trigger set.
9. ~~**Calendar scope creep guardrail**~~ **RESOLVED (Strategist, 2026-07-02): codified as a standing rule in `CLAUDE.md` Section 26**, not left as spec-local rationale. Any future data-source addition or scope widening (for Meeting Prep or any other workflow) is CEO-level by default, full stop.

**All open questions resolved or explicitly deferred to a named future point (Privacy Reviewer pass, or a specific future spec).** This spec is accepted — see the Status line at the top of this document.
