# NOWH V1: Calendar + Gmail Operation — Technical Specification

**Status:** **Accepted by Strategist, 2026-07-02, including the Contacts-lookup revision** — ready to build, pending (a) the Privacy & Governance Reviewer pass and (b) the CEO's live capability test result being recorded in Open Question 1
**Author:** NOWH Architect
**Milestone:** V1 technical spec, first spec written against the re-sequenced
scope locked 2026-07-02 (`NOWH_Decision_Log.md`, "Major re-sequencing: V1
redefined as Calendar + Gmail operation"). Supersedes Morning Brief as the
current build target — Morning Brief is parked as V2, not touched by this
spec.
**Depends on:** Nothing built yet. No Calendar/Gmail integration exists, no
Sheet exists in any real Google account (`NOWH_Strategist_Continuity.md`,
"Not Started"). This spec does not depend on the V2 Sheet schema and does
not reference it.

**Revision note (2026-07-02, same day, second revision):** Adds Google
Contacts as a targeted, read-only invitee-resolution source for Calendar
actions, per the CEO decision logged in `NOWH_Decision_Log.md` ("Google
Contacts lookup approved for V1 (new access surface)"). This reverses the
prior revision's hard constraint that invitee emails could only ever come
from what the director directly typed/pasted — Contacts lookup is now a
second resolution source, inserted between "director states it" and "ask
the director." **Gmail's resolution rules are unchanged and remain
forbidden as a lookup source** — this revision does not touch that
constraint. The approval-gate contract (Section 3.2) is unchanged in
substance; it gains one new requirement: the confirmation preview must
name the resolution source when an email came from Contacts, so the
director can catch a wrong or stale match before approving. Sections
revised: 3.1, 3.2, 5 (item 4), 7, 8, 9, 10. All other sections are
unchanged from the prior accepted revision.

---

## 1. WHAT & WHY

**What:** Two Claude Cowork-operated capabilities against the director's own
Google account, using Claude Cowork's native Google Workspace connector
only (no custom infrastructure):

1. **Calendar operation** — the director can create, edit, and delete
   events and reminders on their own primary Google Calendar through
   conversation, including events that invite people outside their own
   organization (funders, grantees, board members, vendors). Any such
   external-facing event requires the director's explicit, informed
   confirmation before Cowork actually creates or sends it.
2. **Gmail summarization** — the director can ask NOWH to summarize new
   (unread) mail in their primary inbox. Read-only. No drafting, no
   sending.

**Why this reduces director friction:** A lean nonprofit director currently
operates Calendar and Gmail by hand — clicking through Google's UI to build
events, chase down who needs to be invited, and manually skim an inbox each
morning to figure out what's new and what matters. This lets the director
say "Set up a call with [funder] next Tuesday at 2pm" or "What's new in my
inbox?" and have it happen conversationally, with the same trust boundary
around external-facing actions that the rest of NOWH already enforces.

**Why this protects the mission:** This is the harder unknown NOWH needs to
prove before layering relationship/approval logic on top of it (per the
Decision Log rationale): can Claude Cowork reliably operate two real,
consequential Google services at all, and can NOWH's own command design
enforce a human approval gate on an external-facing action regardless of
what the underlying connector does by default. Getting the approval-gate
design right here is the single most important thing in this spec — an
external calendar invite is functionally equivalent to sending an external
email (same bar, per `CLAUDE.md` Section 11), and NOWH's Human Approval Rule
must hold even if Claude Cowork's connector never adds its own confirmation
step.

---

## 2. CONTEXT INTAKE

**What exists today:** Nothing. No Calendar or Gmail integration has been
built. The only prior spec (`docs/specs/morning_brief_spec.md`) includes a
narrow, read-only Calendar integration (primary calendar, title + time only,
for "Meetings Needing Prep") — that spec is accepted but not yet built, and
belongs to V2 (parked). This V1 spec is a different, broader capability
(full read/write Calendar operation, plus Gmail read/summarize) and does not
depend on Morning Brief being built first.

**What changes:** This spec originates:
- A Claude Cowork command surface for Calendar operation (create/edit/
  delete events and reminders, including external invitees) with a formal,
  NOWH-specified approval-gate conversational contract.
- A Claude Cowork command surface for Gmail summarization (unread, primary
  inbox only).
- No Google Sheet, Doc, or Drive structure — V1 has no data layer of its
  own. Calendar and Gmail themselves are the system of record (see Section
  3.4 for the explicit "no persistence" conclusion).

**What stays the same:** The locked architecture (Claude Cowork runs the
work, Google stores the truth, NOWH defines the rules), the Human Approval
Rules, the sensitivity framework, and the zero-cost/no-MCP-adapter
constraint all apply unchanged. Nothing exists yet to migrate (Section 4).

**Which existing patterns are reused/extended:**
- The **approval-gate framing** reuses the same posture already established
  for external-facing actions elsewhere in NOWH's rules (`CLAUDE.md` Section
  11, `NOWH_Privacy_and_Governance.md` Human Approval Rules) — "external
  party" is defined consistently with those rules (anyone outside the
  director's own organization/staff).
- The **narrow-scope-first pattern** used for Morning Brief's Calendar read
  (primary calendar only, minimum necessary fields) is extended here to
  Gmail (unread, primary inbox only) and reused for Calendar's *read* side,
  though Calendar's *write* side is deliberately broader per the CEO's
  explicit V1 scope decision (full operation, not read-only).
- The **command-surface contract format** (trigger phrase → what it returns
  → what follow-up it invites) established in Morning Brief Section 5 is
  reused here as the backbone of Section 3.

**New pattern introduced, and why:** This is the first spec where NOWH's
own conversational design — not a Sheet, not a Doc template — *is* the
entire deliverable. Section 3 is adapted accordingly: instead of column
tables, it specifies the exact command surface, the exact confirmation
contract, and the (absence of) tracking state. This is a new documentation
shape for an Architect spec, used because the milestone genuinely has no
Sheet/Doc/Drive schema to design — stated explicitly per the task brief's
instruction, not a shortcut.

---

## 3. DATA & STRUCTURE DESIGN

There is no Sheet, Doc, or Drive schema in this spec. This section instead
specifies: (3.1) the Calendar command surface, (3.2) the approval-gate
conversational contract — the core of this spec, (3.3) the Gmail command
surface, and (3.4) NOWH's persistence conclusion for V1.

### 3.1 Calendar command surface

| Trigger phrase (+ close variants) | What it does | What it asks for if missing | What it returns |
|---|---|---|---|
| "Schedule a [meeting/call] with [person/org] on [date] at [time]" | Creates a new Calendar event on the director's primary calendar, with the named person/org as an invitee if an email address can be resolved. **Resolution sources, in order:** (1) the request itself, if the director states or pastes the email; (2) a **targeted, read-only Google Contacts lookup** by the name given, triggered only to resolve this one specific invitee for this action — never a bulk export, never a standing scan of the director's contacts; (3) if no Contacts match is found, or the match is ambiguous (more than one contact with that name), NOWH asks the director directly for the email rather than guessing. **NOWH must never search Gmail or any other source to look up or infer an invitee's email — Gmail remains a forbidden resolution source.** Contacts is the one addition to resolution sources; see Section 10, Open Question 3 for the decision history. | Missing date/time; no Contacts match or an ambiguous Contacts match (multiple same-name contacts) | If any invitee is external (Section 3.2 defines "external"): the full pre-send preview and explicit confirmation request (Section 3.2) before the event is created/sent — **and if the email was resolved via Contacts rather than director-stated, the preview must say so** (Section 3.2). If purely internal (no external invitee, e.g. "Block 2-3pm for budget review"): creates directly, confirms conversationally after the fact ("Blocked 2-3pm today for budget review.") |
| "Move/reschedule [event] to [new date/time]" | Edits an existing event's date/time | Which event, if ambiguous (multiple matches) | Same approval-gate branch as above — a reschedule of an event with external invitees re-triggers the approval gate (Section 7 edge case), since the notification email to those invitees is itself an external-facing send |
| "Cancel/delete [event]" | Deletes an existing event | Which event, if ambiguous | If the event has any external invitee: approval gate applies (Section 3.2) — cancellation notifies invitees, which is itself an external-facing send. If purely internal: deletes directly, confirms after the fact |
| "Add/remove [person] from [event]" | Edits an event's invitee list. Invitee email resolution for an add follows the same order as above (director-stated, then targeted Contacts lookup, then ask) | Which event, if ambiguous; invitee's email if not resolvable via director statement or Contacts | Adding or removing an *external* invitee triggers the approval gate (this changes who is notified externally), with the Contacts-source note in the preview if applicable. Adding/removing an internal-only attendee does not |
| "Set a reminder for [X] on/at [date/time]" | Creates a one-off or recurring reminder (Calendar event with no attendees, or Calendar's native reminder/notification feature — whichever Claude Cowork's connector actually exposes) | Recurrence pattern if the director says "recurring" without specifying frequency | Confirms conversationally after creation — reminders are internal-only by definition (no attendee), so no approval gate applies |
| "What's on my calendar [today/this week/on X date]" | Reads and summarizes events in the requested window from the primary calendar | Nothing — defaults to today if no window given | A conversational summary (not a new Doc — see 3.4) |

**Scope boundary:** all operations are against the director's **own primary
calendar only** — consistent with the locked V1 access surface
(`NOWH_Privacy_and_Governance.md` "V1 Data Access Surfaces"). No secondary
or shared calendars are read or written. If the director asks NOWH to
operate on a calendar other than their own primary calendar, NOWH declines
and states this is out of scope for V1 (Section 7).

**"External" defined for this spec:** anyone who is not the director
themselves and not part of the director's own organization/staff — i.e.
funders, donors, grantees, board members, vendors, or any other outside
party. This matches the definition already locked in `CLAUDE.md` Section 11
and `NOWH_Privacy_and_Governance.md`. Where NOWH cannot confidently
determine whether an invitee is internal or external (Section 7), it must
default to treating them as external — same "when in doubt, classify
higher" posture as the rest of NOWH's privacy rules.

### 3.2 The approval-gate conversational contract (core of this spec)

This is a **NOWH-specified command-design requirement**, not an assumption
about what Claude Cowork's underlying Google Calendar connector does by
default. It must hold regardless of platform behavior. If the connector
also happens to pause before sending an external invite, that is a
redundant safety net on top of this contract — not a substitute for it.

**Trigger condition:** Any Calendar action (create, edit/reschedule, delete/
cancel, or invitee add/remove) that would result in a notification being
sent to one or more people outside the director's own organization/staff.

**What NOWH must show the director before taking the action — the preview:**

NOWH must present, as an explicit, distinct conversational turn (not buried
in a longer message, not combined with an unrelated response), all of the
following before calling any tool/connector action that would actually
create or modify the event:

1. **Who is being invited/notified** — every external invitee by name and
   (if available) email address. If any invitee's identity or email is
   uncertain, NOWH must say so explicitly rather than guessing silently
   (e.g., "I found an email for [Name] but I'm not fully sure it's current
   — confirm before I use it"). **If the email was resolved via a Google
   Contacts lookup rather than stated/pasted directly by the director, the
   preview must say so** (e.g., "Jordan Lin (jordan@riverbendyouth.org,
   found via your contacts)") — this is new information the director needs
   to catch a wrong or stale match, since they didn't type the email
   themselves and can't otherwise tell where it came from. If the director
   stated or pasted the email directly, no source note is needed — the
   requirement only applies to Contacts-resolved matches.
2. **What the event is** — the event title/subject as it will appear on
   the invite.
3. **When** — date, start time, end time (and time zone if there's any
   ambiguity), and whether it's one-off or recurring.
4. **What action is about to happen** — stated plainly: "create and send
   this invite," "send this reschedule notification," "send this
   cancellation notice," or "add/remove this person and notify them" — so
   the director knows exactly what external-facing send is about to occur,
   not just what the event will look like.

**Example preview NOWH must produce (illustrative, not a fixed script —
content requirements above are fixed, exact wording may vary naturally in
conversation):**

> "Here's what I'm about to send: a calendar invite titled **'Budget Review
> Call'** for **Tuesday, July 7 at 2:00–2:30pm ET**, inviting **Jordan Lin
> (jordan@riverbendyouth.org, found via your contacts)** — outside your
> organization. I haven't sent anything yet. Want me to go ahead?"

(If the director had typed or pasted the email directly instead, the same
preview would omit "found via your contacts" — the source note only
appears when NOWH resolved the match itself.)

**What confirmation NOWH needs before proceeding:**

- An affirmative, unambiguous response to that specific preview — e.g.,
  "yes," "send it," "go ahead," "confirmed." A vague or unrelated reply
  ("ok" in response to a different topic, silence, a topic change) does not
  count as confirmation.
- **No partial-credit inference.** If the director's reply changes any
  detail (e.g., "yes but make it 3pm instead"), NOWH must re-preview the
  updated details and get a fresh confirmation — the original confirmation
  does not carry over to a modified action.
- **No default-yes on ambiguity.** If NOWH is not sure whether the director
  confirmed, it must ask again rather than proceed. Silence or an unclear
  reply is treated as "not yet confirmed," never as implicit approval.
- Once confirmed, NOWH proceeds and then reports completion conversationally
  (e.g., "Sent — Jordan should see the invite now.").
- If the director declines or asks for changes, NOWH must not create/send
  anything — it holds the action, revises per feedback, and re-previews.

**What does NOT require this formal gate:** purely internal actions — no
external invitee at all (blocking the director's own time, personal
reminders, internal-only staff meetings). For these, ordinary conversational
confirmation is sufficient (NOWH states what it did or is about to do in
plain language; it does not need the structured preview above). This mirrors
the locked distinction already codified in `CLAUDE.md` Section 11.

**Where this lives in the build:** this contract must be expressed directly
in NOWH's own system prompt / Skill instructions for the Calendar command
surface — not left to be inferred from general good-assistant behavior, and
not delegated to Claude Cowork's connector defaults. Claude Code, when
implementing, must write this as an explicit, unambiguous instruction block
(the specific requirements above: preview contents including the
Contacts-source note when applicable, confirmation requirements,
no-partial-credit rule, default-to-external-on-ambiguity rule) so that the
behavior is enforced by NOWH's design even in a Cowork session/connector
configuration where the platform itself does not pause.

### 3.3 Gmail command surface

| Trigger phrase (+ close variants) | What it does | What it returns |
|---|---|---|
| "What's new in my inbox" / "Summarize my email" / "Any new email?" | Reads unread messages in the primary inbox only (not other labels/folders, not read mail, not a historical search) | A conversational summary: sender, subject, one-line gist per message, grouped or prioritized in whatever way is genuinely useful (e.g., flagging anything that looks time-sensitive) — no Doc is created (Section 3.4) |
| "Anything from [person/org] in my inbox?" | Same read scope, filtered conversationally to messages matching the named sender | Same summary format, scoped to matches; if none found, says so plainly rather than returning an empty/confusing response |

**Scope boundary — locked, not this spec's to widen:** unread mail, primary
inbox only. No read/archived mail, no other labels or folders, no
historical search, no drafting, no sending. This matches
`NOWH_Privacy_and_Governance.md` "V1 Data Access Surfaces" exactly. If a
future need arises to widen this (more of the inbox, drafting, sending),
that is a fresh CEO-level decision per the standing rule (`CLAUDE.md`
Section 26) — this spec does not create it and Claude Code must not infer
it from "it would be more useful."

**No approval gate needed for Gmail in V1** — because there is no
send/draft/write action at all in this surface. Summarization is read-only
and internal-facing by definition (the summary is shown only to the
director, never sent anywhere). This is a direct consequence of the
locked scope, not a separate design decision.

### 3.4 What (if anything) NOWH needs to track — persistence conclusion

**Conclusion: no persistence is needed for V1.** Calendar and Gmail
themselves are the system of record, consistent with the locked
architecture ("Google stores the truth"). Specifically:

- **Calendar events** created/edited/deleted by NOWH exist entirely within
  the director's own Google Calendar. NOWH does not need a separate record
  of "what NOWH did" — the calendar itself shows the current state, and
  Google's own edit history/notifications handle the rest.
- **Gmail summaries** are generated fresh, live, at the moment the director
  asks — there is nothing to persist between runs. A summary from an hour
  ago is stale the moment new mail arrives; re-reading live is strictly
  correct where a cached/stored summary would not be (same reasoning
  already established in `morning_brief_spec.md` Section 3.2.1 for why
  Calendar reads there are live, not persisted).
- **No confirmation/approval log is introduced.** The approval-gate
  contract (Section 3.2) is a real-time conversational requirement enforced
  in the moment, not an auditable record NOWH stores afterward. If the
  Strategist or CEO later wants a durable approval audit trail, that is a
  new requirement (likely part of V2's Approval Queue) — not something this
  spec silently adds now. Flagged in Open Questions.

This is a deliberate anti-bloat conclusion, not an oversight: introducing
any tracking structure (a Sheet, a log file, a Doc) here would be exactly
the kind of "abstraction layer... unless the current milestone requires it"
the Architect charter warns against. V1's whole premise is that no data
layer is required yet.

---

## 4. MIGRATION

None needed. No Calendar/Gmail integration exists in any director's account
today, and this spec introduces no Sheet/Doc/Drive schema to migrate.
Nothing about a director's existing Calendar or Gmail data is transformed,
restructured, or moved — NOWH only operates on events/mail going forward,
through normal Calendar/Gmail actions indistinguishable from the director
doing them by hand.

---

## 5. PROPAGATION

1. **Google Sheet schema** — No impact. V1 introduces no Sheet. (V2's
   5-tab schema, `NOWH_Data_Schema.md`, is untouched and unreferenced.)
2. **Google Docs templates** — No impact. No new Doc template. Calendar
   summaries and Gmail summaries are both conversational, not written to a
   Doc (Section 3.4).
3. **Google Drive structure** — No impact. No new folder.
4. **Gmail / Calendar conventions** — This spec *is* the Gmail/Calendar
   integration. Calendar: full create/edit/delete on the director's primary
   calendar only, including external invitees, gated per Section 3.2. Gmail:
   read-only, unread + primary inbox only, no labels/conventions introduced
   (NOWH does not create Gmail labels in V1 — not needed for a live,
   ungrouped unread-mail summary). No naming convention imposed on event
   titles — the director's own phrasing is used as given. **Google Contacts
   is now a read source for Calendar invitee resolution** (Section 3.1) —
   targeted, read-only, name→email lookup only, triggered per-invitee, never
   a bulk export or standing scan. No new Contacts conventions are
   introduced (no labels, no groups, no writes) — NOWH only reads.
5. **Claude Cowork command surface** — New trigger phrases per Section 3.1
   (Calendar) and Section 3.3 (Gmail). This is the primary deliverable of
   this spec — see those sections for the full contract. Follow-up
   invitations: after a Calendar action, NOWH may offer "Want me to set a
   reminder for this?" or "Anyone else who should be looped in?" (itself
   re-triggering Section 3.2 if that person is external); after a Gmail
   summary, NOWH may offer "Want me to check your calendar for anything
   related?" — light conversational connective tissue, not new commands.
6. **Privacy & sensitivity labeling** — See Section 8. Event titles/details
   and email subject lines are director-authored or third-party-authored
   content NOWH reads/creates but does not classify into a Sheet (there is
   none). Sensitivity is handled conversationally (Section 8), not via a
   column schema, since there is no schema in V1.
7. **Approval gates** — This spec **is** the approval gate design for V1's
   two new capabilities. Section 3.2 is the load-bearing section of this
   entire spec.
8. **Plugin packaging** — `NOWH_Plugin_Packaging_Plan.md`'s command surface
   list must be extended with the Calendar and Gmail trigger phrases from
   Sections 3.1/3.3 once this spec is accepted. The existing Morning Brief
   entry describes Sheet-tab setup requirements that do not apply here — V1
   packaging needs the director to connect Google Calendar and Gmail via
   Claude Cowork's native connector (OAuth through Cowork's own settings,
   no separate NOWH setup step), not to copy a Sheet template. Flagged for
   the Strategist to fold in; not resolved further by this spec (out of
   scope per the task brief — packaging is a separate milestone).
9. **Demo seed data** — V1 has no synthetic Sheet dataset to seed. For a
   live demo, the demo director's own (or a demo Google account's) real
   Calendar/Gmail state is what gets operated on — there is no seed data
   concept for V1 the way there was for Morning Brief's Sheet rows. If a
   scripted demo needs predictable events/mail to react to, that's a QA/demo
   readiness concern (seeding a demo Google account with sample events and
   unread mail before a rehearsal), not something this spec needs to design.

---

## 6. DIRECTOR FRICTION IMPACT

**Current flow (today, no NOWH):** Director opens Google Calendar directly,
builds an event by hand, manually looks up and enters attendee emails,
manually double-checks who's on the invite before hitting send. Separately,
director opens Gmail and skims the inbox each morning to find what's new.

**New flow:** Director says what they want conversationally ("Set up a call
with the board finance chair Thursday afternoon"); NOWH assembles the event
and — if anyone external is involved — shows a clear preview and waits for
a yes before anything is actually sent. For Gmail, the director asks "what's
new" and gets a summary instead of skimming manually.

**Friction reduced:**
- No manual Calendar UI interaction for routine scheduling.
- No manual attendee lookup if NOWH can resolve the person from
  conversation context.
- No manual inbox skim just to find out what's new.

**Friction introduced:**
- The confirmation step (Section 3.2) is a deliberate, real added step for
  any external-facing Calendar action — this is friction the design
  *intentionally* introduces, because it is the entire point of the
  approval gate. It is not a defect to mitigate; it is the safeguard. The
  mitigation that does apply: the preview is a single, clear turn (not a
  multi-step form), so the added friction is one yes/no exchange, not a
  process.
- If NOWH cannot confidently resolve an invitee's email, it must ask rather
  than guess (Section 3.1/3.2) — a small added step, but the alternative
  (guessing wrong and inviting the wrong person, or a wrong address) is
  worse. Mitigation: NOWH tries the full resolution chain first — what the
  director already stated, then a targeted Google Contacts lookup — before
  asking the director to supply the email manually. (Gmail is never part of
  this chain — see Section 3.1's hard constraint.)
- Internal-only actions (blocking time, personal reminders) must not
  inherit this friction — Section 3.1/3.2 explicitly carve these out to
  keep the common case fast.

**Grantee/funder/board burden check:** This workflow does not ask any
grantee, funder, or board member for anything — it only affects what the
director sends to them (an invite, a reschedule, a cancellation), and the
approval gate exists specifically to make sure the director is the one
deciding whether/when that external party gets contacted at all. No new
burden is placed on the external party by this design; if anything, the
approval gate reduces the risk of an accidental or premature invite
reaching them.

---

## 7. EDGE CASES

**Calendar:**
- **Ambiguous internal-vs-external invitee:** NOWH cannot always tell
  whether "Sam" is a staff member or an external contact from name alone.
  Default per Section 3.1: **treat as external and apply the approval gate**
  whenever there is genuine ambiguity — the cost of an unnecessary
  confirmation is far lower than the cost of an unapproved external send.
  NOWH should ask directly if unsure ("Is Sam on your team, or an outside
  contact?") rather than guessing either way.
- **Reschedule or cancellation of an event that already has external
  invitees:** Both notify those invitees (Google sends the update/
  cancellation email automatically as part of the connector action) and
  therefore both re-trigger the approval gate — this is stated explicitly
  in Section 3.1's table row-by-row so it isn't accidentally treated as
  "just an edit," which would be a real gap (a reschedule that silently
  fires an email to a funder without confirmation is exactly the failure
  this spec exists to prevent).
- **Director asks NOWH to invite someone but doesn't specify an email and
  none can be resolved:** NOWH asks the director for it directly rather
  than proceeding with a guessed or fuzzy-matched address. Never silently
  drops the invitee or invites a similarly-named wrong person.
  This is a special case of the preview-content requirement in Section 3.2 requiring
  NOWH to flag uncertainty explicitly.
- **Director says "yes" but the request has changed since the last
  preview** (e.g., they asked to change the time mid-conversation): per
  Section 3.2's no-partial-credit rule, NOWH must re-preview and get a
  fresh confirmation, not reuse the earlier "yes."
- **Director tries to add/remove an external invitee from an existing
  event in the same breath as an unrelated request:** NOWH must still
  isolate the external-facing action into its own explicit preview/
  confirmation turn (Section 3.2 — "not buried in a longer message")
  rather than let it slide through as part of a broader reply.
- **Recurring event with an external invitee:** the preview (Section 3.2)
  must state that it's recurring and the recurrence pattern, since the
  director is approving an ongoing series of external notifications, not
  a one-off — this is part of "what the event is" in the preview
  requirements.
- **Director wants to operate on a calendar other than their own primary
  calendar** (a shared calendar, a team calendar): out of scope for V1
  (Section 3.1's scope boundary). NOWH states this plainly rather than
  attempting it or silently defaulting to the primary calendar without
  saying so.
- **No Contacts match found for a named invitee:** NOWH asks the director
  directly for the email rather than failing, guessing, or fuzzy-matching a
  similarly-named contact. Same posture as the pre-Contacts spec's "no
  email resolvable" case (Section 3.1).
- **Multiple Contacts matches for the same name:** NOWH does not guess which
  one the director means. It asks the director to disambiguate, showing
  what's available (e.g., distinguishing details already visible in
  Contacts, such as a second email or an org affiliation, if that helps the
  director tell them apart) rather than silently picking the first or most
  recent match.
- **A Contacts match exists but looks stale or wrong in some detectable
  way** (e.g., a bounced-address signal NOWH can actually observe): treat it
  the same as an uncertain match — surface the concern explicitly in the
  preview rather than using it silently. Where staleness is *not*
  detectable (the common case — NOWH has no way to know a saved email is
  outdated), this spec does not claim NOWH can verify it; the Section 3.2
  preview requirement (showing "found via your contacts") is the safeguard,
  giving the director the chance to catch it themselves, not a detection
  mechanism on NOWH's side.
- **Google Contacts access not granted:** degrades gracefully, same pattern
  as Calendar/Gmail not being connected — NOWH does not block the whole
  Calendar action. It states plainly that it can't look up contacts right
  now and asks the director for the invitee's email directly, then
  continues the rest of the action normally.
- **Time zone ambiguity:** if the director's stated time is ambiguous
  (traveling, working across zones), NOWH must resolve or ask rather than
  guess — an external invite sent for the wrong time is a real, embarrassing
  failure mode, not a minor one.
- **Director declines or asks for a revision after the preview:** NOWH
  holds the action, does not create/send anything, and revises based on
  feedback before re-previewing (Section 3.2).

**Gmail:**
- **No unread mail:** NOWH states plainly that there's nothing new, rather
  than an empty or confusing response.
- **Unread mail exists but is entirely automated/low-value (newsletters,
  notifications):** NOWH may summarize briefly rather than pretend
  significance, but must not silently omit messages — the summary should
  still account for what's there, even if the gist is "mostly routine
  notifications, nothing that looks time-sensitive."
- **Gmail access not yet granted or revoked:** NOWH states plainly that
  Gmail isn't connected rather than failing silently or fabricating a
  summary — same graceful-degradation pattern already established in
  `morning_brief_spec.md` Section 7 for a missing Calendar connection.
- **Calendar access not yet granted or revoked:** same pattern — NOWH
  states plainly that Calendar isn't connected when a Calendar command is
  attempted, rather than erroring opaquely or pretending to have completed
  an action it could not actually perform. This matters more here than in
  Morning Brief's read-only case, because a failed write could otherwise be
  misreported as successful — NOWH must not confirm an action to the
  director that did not actually happen.
- **A message in the unread/primary inbox contains highly sensitive content**
  (e.g., a donor's financial detail, a youth story): the summary must not
  flatten or strip that sensitivity — see Section 8 for how the summary
  itself must handle this, since there is no Sheet-based sensitivity column
  to route it through here.

---

## 8. PRIVACY & SENSITIVITY CONTROL

**How sensitivity is enforced here (no schema, so this is conversational/
behavioral, not column-based):**

- **Calendar event content the director creates:** event titles and
  attendee lists are treated as at least Medium sensitivity (relationship/
  coordination context), consistent with how Morning Brief's Calendar read
  classifies the same fields (`morning_brief_spec.md` Section 3.2.1). If an
  event title appears to reveal something more sensitive (a personal
  situation, a sensitive negotiation), NOWH must not repeat, summarize, or
  reference that title casually elsewhere in conversation beyond what's
  needed to confirm the action — "when in doubt, classify higher" applies
  the same way it does everywhere else in NOWH.
- **Gmail summaries:** subject lines and message gists may contain High or
  Restricted content (a donor's financial detail, a grantee's sensitive
  situation, a youth/community story reference). The summary is shown only
  to the director — it is inherently internal-facing, never sent anywhere,
  so the main risk is not "external leakage" but **the director later
  copy-pasting a summary line into something external without realizing its
  sensitivity.** NOWH should not proactively re-surface sensitive summary
  content into a different context (e.g., don't fold a Gmail summary detail
  into a Calendar invite description without the director explicitly asking
  for that content to be included) — this prevents a Restricted-level detail
  from Gmail leaking into an external-facing Calendar invite by accident.
- **No scoring/ranking risk:** neither capability introduces any score,
  rank, or judgment field — Calendar operations are factual (create/edit/
  delete), and Gmail summaries describe what's there, never evaluate the
  sender or the relationship.

**What could leak if built carelessly, and how this design prevents it:**

- **Risk:** NOWH treats an external Calendar invite the same as an internal
  one because the underlying Cowork connector doesn't itself pause.
  **Prevention:** Section 3.2 is written as a hard requirement independent
  of platform behavior — the confirmation gate lives in NOWH's own system
  prompt/Skill instructions, not inferred from connector behavior. This is
  the single most load-bearing prevention in this entire spec.
- **Risk:** A reschedule or cancellation is treated as "just an edit," not
  recognized as an external-facing send, and fires a notification to a
  funder/grantee without confirmation. **Prevention:** Section 3.1's table
  and Section 7's edge case explicitly call out reschedule/cancel as
  approval-gated whenever external invitees are present — not left to be
  inferred.
- **Risk:** NOWH guesses at an ambiguous invitee's identity or email and
  gets it wrong, sending a sensitive invite (e.g., board material discussion)
  to the wrong person. **Prevention:** Section 3.1/3.2/7 all require NOWH to
  surface uncertainty explicitly and ask rather than guess.
- **Risk:** A Gmail summary detail (e.g., a grant amount, a youth story
  reference) gets folded into an external-facing Calendar invite's
  description without the director realizing it crossed a sensitivity
  boundary. **Prevention:** stated directly above — NOWH does not
  proactively carry Gmail content into a Calendar action unless the
  director explicitly asks for it, and even then, an external invite still
  goes through the Section 3.2 gate where the director sees exactly what's
  about to be sent, including any such carried-over content.
- **Risk:** the Gmail scope quietly widens (more of the inbox, other
  labels) because "it would be more useful for summarization."
  **Prevention:** Section 3.3 states the scope boundary is locked and not
  this spec's to widen, and reiterates the standing CEO-level escalation
  rule (`CLAUDE.md` Section 26) by name so a future implementer or spec
  author can't quietly expand it.
- **Risk:** Contacts access, once granted, quietly becomes a broader data
  source than intended — a bulk export, a standing background scan, or a
  general "look someone up" feature untethered from a specific Calendar
  action. **Prevention:** Section 3.1 and `NOWH_Privacy_and_Governance.md`
  both state, explicitly and in the same terms, that this is a **targeted
  lookup only** — one name, triggered only to resolve one specific invitee
  for the Calendar action at hand, never a bulk export and never a standing
  scan. Same rigor as the Gmail-scope-creep entry above, stated with equal
  weight rather than as an afterthought because it's a newer surface.
- **Risk:** NOWH silently matches the wrong person from Contacts (same
  name, different person; a stale saved address) and an external send goes
  to the wrong recipient without the director noticing. **Prevention:** the
  Section 3.2 preview requirement — every Contacts-resolved email is
  labeled "found via your contacts" in the confirmation preview, giving the
  director a concrete chance to catch a wrong match before anything is
  sent. Ambiguous matches (Section 7) are never silently resolved; NOWH
  asks the director to disambiguate instead.
- **Risk:** a Contacts entry carries sensitive annotation beyond name and
  email (notes, phone number, physical address, or other fields the
  director may have added) and that content leaks into a Calendar preview
  or event where it doesn't belong. **Prevention:** NOWH's Contacts lookup
  is scoped to reading and surfacing **only the resolved name and email** —
  no other Contacts field is read into the conversation, the preview, or
  the event, unless the director explicitly asks for it (e.g., "what's
  their phone number?"). This mirrors the existing rule against folding
  Gmail summary content into a Calendar invite without the director asking
  for it.
- **Risk:** the Calendar approval gate is implemented as informal good
  judgment ("the AI should probably ask first") rather than a specific,
  testable contract, and erodes over time as the prompt is edited.
  **Prevention:** Section 3.2 specifies exact required preview contents,
  exact confirmation requirements, and explicit rules for partial credit
  and ambiguity — written so Claude Code can implement it as a literal
  instruction block and QA can test against Section 9's acceptance tests
  directly, not as vague guidance.

---

## 9. ACCEPTANCE TESTS

1. Asking NOWH to "schedule a call with [external funder] next Tuesday at
   2pm" produces a preview (event title, date/time, invitee name+email,
   explicit statement that this is about to be created/sent) and takes no
   Calendar action until the director responds affirmatively.
2. After the director confirms the above, the event is actually created on
   the director's primary Calendar with the stated invitee, and NOWH
   reports completion conversationally.
3. Asking NOWH to "block 2-3pm today for budget review" (no external
   invitee) creates the event immediately with only conversational
   confirmation after the fact — no formal preview/confirmation exchange
   is required.
4. Asking NOWH to reschedule an existing event that has an external
   invitee triggers a fresh preview and confirmation before the reschedule
   is applied — it is not treated as a low-stakes edit.
5. Mid-confirmation, if the director changes a detail (e.g., "actually make
   it 3pm"), NOWH re-previews the updated event and requires a fresh
   confirmation rather than proceeding on the original "yes."
6. If the director's reply to a preview is ambiguous or silent, NOWH does
   not proceed — it asks again rather than defaulting to yes.
7. If the director declines a preview or asks for a change, no event is
   created/modified, and NOWH revises and re-previews rather than silently
   dropping the request or proceeding anyway.
8. Asking "what's new in my inbox" returns a summary of unread, primary-
   inbox mail only — messages that are read, or in other labels/folders,
   do not appear.
9. Asking "what's new in my inbox" when there is no unread mail returns a
   plain statement that there's nothing new, not an empty or broken
   response.
10. No Gmail action ever drafts or sends anything — running any Gmail
    command leaves the inbox's read/unread state and message contents
    completely unchanged (verifying the read-only boundary holds).
11. With Calendar access not granted, attempting any Calendar command
    results in NOWH plainly stating Calendar isn't connected — it never
    reports a Calendar action as successful when it could not actually
    perform it.
12. With Gmail access not granted, asking for an inbox summary results in
    NOWH plainly stating Gmail isn't connected, rather than fabricating or
    silently failing.
13. An ambiguous invitee (NOWH cannot tell if they're internal or external)
    is treated as external and routed through the full approval gate,
    confirmed by NOWH asking the director to clarify rather than guessing.
14. A purely internal reminder (e.g., "remind me to call the accountant
    Friday") is created without triggering the formal preview/confirmation
    contract.
15. Asking NOWH to schedule a call with a named invitee who is *not* stated
    with an email (e.g., "schedule a call with Jordan Tuesday at 2pm")
    resolves the email via a targeted Google Contacts lookup, and the
    resulting confirmation preview names the resolution source (e.g.,
    "found via your contacts") rather than presenting it as if the director
    had typed it.
16. When no Contacts match is found for a named invitee, NOWH asks the
    director for the email directly rather than failing, guessing, or
    inviting a similarly-named wrong person.
17. When multiple Contacts matches exist for the same name, NOWH asks the
    director to disambiguate — showing what's available — rather than
    silently picking one.
18. With Google Contacts access not granted, attempting to schedule an
    event with a named (non-email-stated) invitee degrades gracefully: NOWH
    states it can't look up contacts right now and asks the director for
    the email directly, without blocking the rest of the Calendar action.

---

## 10. OPEN QUESTIONS

1. ~~**Live platform validation — PENDING.**~~ **PARTIALLY RESOLVED (CEO,
   2026-07-02):** confirmed that Claude Cowork's native Google Calendar
   connector does pause and request approval before sending an external
   invite (Test 1). This is a **redundant safety net on top of** — not a
   substitute for — the hard requirement in Section 3.2, which still
   governs the exact preview content, no-partial-credit, and no-default-yes
   rules regardless of platform behavior. **Still open:** whether the
   platform's own confirmation also fires for purely internal actions with
   no external invitee (Test 2) — this matters for whether NOWH's "internal
   actions stay fast" design (Section 3.1/3.2) will actually feel fast, or
   whether the platform adds its own friction there too regardless of what
   NOWH specifies. Pending CEO follow-up.
2. ~~**Exact mechanism for "reminders."**~~ **RESOLVED (Strategist,
   2026-07-02): accepted as an implementation-time decision.** Claude Code
   should use whichever mechanism (no-attendee event vs. native reminder
   feature) the connector actually exposes — doesn't affect the
   approval-gate design either way.
3. ~~**Invitee email resolution source.**~~ **RESOLVED (Strategist,
   2026-07-02): confirmed as a hard constraint, tightened directly into
   Section 3.1** (not left as an Open-Questions-only guardrail) — NOWH must
   never search Gmail or any other source to resolve an invitee's email; it
   only ever uses what the director directly states in conversation.
   **SUPERSEDED — see revised resolution below (same day, second
   revision).** ~~This resolution is no longer current.~~
   **RE-RESOLVED (CEO, 2026-07-02, "Google Contacts lookup approved for V1
   (new access surface)," `NOWH_Decision_Log.md`):** the "director-stated
   only" constraint above is reversed for Contacts specifically. NOWH may
   now resolve a named invitee's email via a **targeted, read-only Google
   Contacts lookup** — one name, triggered only for the specific invitee
   needed for the Calendar action at hand, never a bulk export or standing
   scan — as a second resolution source between "director states it" and
   "ask the director" (Section 3.1). **Gmail remains forbidden as a
   resolution source, unchanged.** The approval gate (Section 3.2) is
   unchanged in substance; it now requires the confirmation preview to name
   the resolution source when a match came from Contacts, so the director
   can catch a wrong or stale match before approving.
4. ~~**No approval audit trail in V1.**~~ **RESOLVED (Strategist,
   2026-07-02): accepted — no audit trail in V1.** Logged as a candidate V2
   requirement in `NOWH_Backlog.md` (natural fit alongside the Approval
   Queue workflow), not built now.
5. ~~**Command phrase set is proposed, not user-tested.**~~ **RESOLVED
   (Strategist, 2026-07-02): accepted as proposed**, same resolution as
   Morning Brief's equivalent open question.
6. **What disambiguating detail can NOWH actually show for multiple
   Contacts matches with the same name?** Section 7's multiple-match edge
   case says to "show what's available" (e.g., a second email, an org
   affiliation) but this spec does not know what fields Claude Cowork's
   Google Contacts connector actually surfaces or how reliably it
   distinguishes near-duplicate contacts. This is a genuine implementation
   unknown, not a product decision — flagged for Claude Code to resolve
   empirically during build (test against a real Contacts list with two
   same-named entries) rather than for the Strategist/CEO to pre-decide.
   Does not block acceptance: the fallback (ask the director directly) is
   already fully specified and safe regardless of what the connector
   exposes.

**Status:** all open questions resolved or explicitly deferred except #1
(pending the CEO's live test, to be closed out once reported) and #6 (a
build-time implementation unknown, not a product decision, that does not
block acceptance). Neither blocks acceptance — see Status line at the top
of this document.
