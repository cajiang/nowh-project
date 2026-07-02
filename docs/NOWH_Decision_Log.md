# NOWH Decision Log

Every major decision is logged here with rationale. Check this file if
anything is unclear before making assumptions.

---

### 2026-07-02 — Project separation

**Decision:** NOWH is a standalone project folder
(`C:\Users\Calvin\Documents\Claude\Projects\NOWH\`), fully separate from
PM_OS and RHEN-OS. No shared files, memory, or conventions.
**Rationale:** User is running multiple simultaneous projects and requires
strict isolation between them.

### 2026-07-02 — Strategist Prompt v2.0 locked

**Decision:** Adopted NOWH Strategist Prompt v2.0 in full as `CLAUDE.md`,
establishing the strategist persona, locked architecture, cost principle,
demo target, six core workflows, privacy rules, approval rules, build agent
plan, and canonical project files.
**Rationale:** User-authored, considered spec — treat as locked unless the
user revises it directly.

### 2026-07-02 — Strategist continuity model

**Decision:** Replaced the trigger-only `NOWH_Handoff.md` with
`docs/NOWH_Strategist_Continuity.md`, a living governance file the current
Strategist keeps continuously up to date (not just when "Begin strategist
handoff" is said). Every new session reads `CLAUDE.md` then this file before
doing anything else.
**Rationale:** User treats each chat session as a Strategist's tenure that
ends at the session limit — the next session's Strategist needs to be
"hired" cold and pick up with zero continuity loss, without the user having
to re-explain project state each time.

### 2026-07-02 — Universal Project Overview adopted as agent onboarding doc

**Decision:** Replaced the short draft `NOWH_Project_Overview.md` with the
user-authored "NOWH Universal Project Overview v1.0" — a comprehensive,
self-contained briefing (what NOWH is/isn't, why it exists, architecture,
demo target, six workflows, privacy/approval/grantee rules, workspace
structure, agent model, success criteria, long-term vision). Designated as
required first-read context for every new custom NOWH build agent, alongside
its own charter. `NOWH_Agent_Architecture.md` updated to reference it.
**Rationale:** Custom agents get spun up with narrow task charters and would
otherwise lack broader product context (privacy stance, grantee-centered
framing, what NOWH must never become) needed to make good judgment calls.

### 2026-07-02 — Live capability test confirms platform-level approval gate

**Finding:** CEO ran the manual capability test in Claude Cowork (Test 1 —
external Calendar invite). Result: Claude Cowork's native Google Calendar
connector does pause and ask for approval before sending an external
invite. This is a **redundant safety net**, not a replacement for the hard
requirement already specced in `v1_calendar_gmail_operation_spec.md`
Section 3.2 — NOWH's own explicit preview/confirmation contract still
governs exact behavior (preview contents, no-partial-credit, no-default-
yes), so the spec's correctness never depended on this finding either way.
Good outcome: belt-and-suspenders, not a single point of failure.
**Still open:** whether the platform's confirmation also fires for
purely-internal actions (Test 2 result not yet reported) — matters for
whether NOWH's "internal actions stay fast" design actually holds in
practice, since a platform-level confirmation on every action (not just
external ones) would add friction NOWH's own design doesn't call for.

### 2026-07-02 — V1 spec Contacts-lookup revision accepted

**Decision:** Accepted the Architect's revision of
`docs/specs/v1_calendar_gmail_operation_spec.md` adding Google Contacts
lookup (Sections 3.1, 3.2, 5, 7, 8, 9, 10 revised in place, per the
"Google Contacts lookup approved for V1" decision above). Reviewed the
full file directly, not just the agent's self-report.

**Found and fixed a second real inconsistency** (same category as the
Morning Brief stale-Gmail-line catch, and something I should have caught
in the *first* V1 review, not just this one): Section 6 (Director Friction
Impact) had a leftover line saying NOWH should "resolve from context
already available in conversation or Gmail" — directly contradicting the
hard, repeated rule elsewhere in the same document that Gmail is forbidden
as an invitee-resolution source. Section 6 wasn't in either revision's
scope (I only asked the Architect to touch 3.1/3.2/5/7/8/9/10), so this
line survived both passes. Fixed directly. **Process note for future
Strategist reviews:** grep the full document for a term under a hard
constraint (here, "Gmail") before accepting, not just the sections a
revision was scoped to touch — a stale reference can hide anywhere.

Everything else in the revision held up well: the approval-gate contract
correctly stayed source-agnostic (Contacts-resolved emails get a "found
via your contacts" label in the preview, but the confirmation requirement
itself didn't weaken), and new edge cases/acceptance tests (no match,
ambiguous match, Contacts access not granted) are all consistent with the
existing graceful-degradation pattern.

**Remaining before V1 is fully closed:** CEO's live capability test result
(Open Question 1); Privacy & Governance Reviewer pass; real implementation
and testing.

### 2026-07-02 — Google Contacts lookup approved for V1 (new access surface)

**Decision:** CEO approved allowing NOWH to look up the director's saved
Google Contacts to resolve a name (e.g., "Jordan") to an email address,
reversing the just-accepted V1 spec's constraint that invitee emails could
only come from what the director directly typed/pasted in conversation.
**Scope, as approved:** targeted, read-only name→email lookup only,
triggered only when NOWH needs to resolve a specific named invitee for a
Calendar action — not a bulk contacts export, not proactive browsing, not
a standing data source NOWH scans. Contacts is a new V1 access surface,
same category as Calendar and Gmail.
**What does NOT change:** the approval-gate requirement (spec Section 3.2)
is source-agnostic — an external invite still requires the director's
explicit confirmation regardless of whether the email came from Contacts
or was typed directly. Contacts lookup changes *resolution*, not the
approval bar. The revised spec must make the resolution source visible in
the confirmation preview (e.g., "found via your contacts") so the director
can catch a wrong/stale match before confirming, since NOWH is now making
an autonomous match rather than only ever using director-stated info.
**Rationale:** CEO call — real friction reduction (director doesn't retype
emails for people they've already saved), weighed against a new access
surface. Consistent with the standing rule (`CLAUDE.md` Section 26):
approved explicitly, not assumed.
**Follow-up required:** Architect must revise
`docs/specs/v1_calendar_gmail_operation_spec.md` Sections 3.1, 7, 8, 9, 10
to add Contacts lookup, preserving Section 3.2 unchanged in substance
(only extending what the preview must show).

### 2026-07-02 — V1 Calendar + Gmail Operation spec accepted

**Decision:** Accepted `docs/specs/v1_calendar_gmail_operation_spec.md`
after reviewing it directly. This is the first spec covering the actual
current build target (V1), delegated in parallel with the CEO's live
Cowork capability test rather than blocking on it — the spec's approval
gate (Section 3.2) is designed as a hard NOWH-specified conversational
requirement independent of platform behavior, so it's correct regardless
of what the test finds.

Found one thing worth tightening before acceptance (not a bug like Morning
Brief's stale-line issue, more a looseness risk): Section 3.1's invitee
email-resolution language mentioned "Gmail contact context" in a way that
could be misread as authorizing a Gmail contacts search — tightened
in-place to make the constraint (director-stated email only, never a
Gmail/contacts lookup) a hard requirement in the design itself, not just a
note in Open Questions.

Resolved 4 of 5 open questions directly (reminder mechanism = implementation
detail, invitee resolution = tightened into the spec itself, no audit
trail = accepted and logged to `NOWH_Backlog.md` as a V2-adjacent
follow-up, command phrases = accepted as proposed). **Open Question 1
(live platform validation) remains genuinely open** — pending the CEO's
test result, to be recorded once available; does not block acceptance.

Folded accepted decisions into `NOWH_Backlog.md` (audit trail follow-up),
`NOWH_Plugin_Packaging_Plan.md` (V1 command surface, noted as the likely
actual first packaging target ahead of Morning Brief), and
`NOWH_Privacy_and_Governance.md` (linked the accepted approval-gate design
under "V1 Data Access Surfaces").

**Still open before V1 is fully closed:** CEO's live capability test result
(Open Question 1); Privacy & Governance Reviewer pass (agent still not
built); actual implementation and testing in a real Google/Cowork
environment.

### 2026-07-02 — Major re-sequencing: V1 redefined as Calendar + Gmail operation

**Decision:** CEO reassessed and redefined V1. Instead of shipping the
six-workflow Sheet-based system as the first deliverable, **V1 is now:
NOWH operates the director's Google Calendar and Gmail through Claude
Cowork, with no custom Sheet data layer required.**

- **Calendar (V1): full operation** — create, edit, delete events/meetings,
  set reminders, on the director's own calendar. **Includes inviting
  external people** (funders, grantees, board, vendors) — explicit CEO
  choice, not a default. **New approval gate added:** any calendar
  event/invite that adds or notifies an external party requires explicit
  director approval before it's sent — same bar as an external email.
  Codified in `CLAUDE.md` Section 11 and `NOWH_Privacy_and_Governance.md`.
- **Gmail (V1): summarize only, narrow scope** — highlight/summarize new
  (unread) mail in the **primary inbox only**, not a full historical
  search. Explicitly no drafting, no sending in V1 — CEO: "let's keep to
  being able to summarize emails first" after initially considering
  draft+send. Both deferred to `NOWH_Backlog.md`.
- **Prior work (Morning Brief spec, 5-tab Sheet schema) is kept, not
  discarded** — re-sequenced as **V2: NOWH Workflow Layer**, built on top
  of the V1 Calendar/Gmail foundation once it's proven, not competing with
  it.

**Rationale:** The original six-workflow plan required proving workflow
logic on top of an unbuilt, untested custom Sheet schema. This re-sequencing
tests the actual hardest unknown first — can Claude Cowork reliably operate
Calendar and Gmail at all — before investing further in NOWH-specific
relationship/approval logic that depends on that foundation working.
Reduces risk, and is more testable/concrete as an actual V1.

**Escalation note:** this reopens locked decisions from Sections 4/6/7 and
grants access far beyond the standing rule added earlier today (Calendar
write + external invites, well past the narrow read-only grant). Handled
correctly as a direct CEO decision, not a Strategist-resolved issue — this
is exactly what Section 23 (CEO Role: "choosing between architecture
paths," "changing the demo target") and the Section 26 standing rule
require.

**Updated:** `CLAUDE.md` Sections 4, 6, 7, 11; `NOWH_Privacy_and_Governance.md`
(new "V1 Data Access Surfaces" section); `NOWH_Backlog.md` (Gmail
draft/send deferral, V2 workflow layer).

### 2026-07-02 — Morning Brief spec accepted; standing access-surface escalation rule added

**Decision:** Accepted `docs/specs/morning_brief_spec.md` after reviewing the
Calendar-access revision directly (not just the Architect's self-report).
Found and fixed one real inconsistency before acceptance: Section 8 still
claimed "no Gmail/Calendar access in this v1 slice" despite the same
section's own new Calendar-access-surface analysis three lines below —
corrected in place rather than sent back for a full round-trip, per
Strategist "fix now" authority (`CLAUDE.md` Section 26). Resolved the
remaining 7 open questions directly (Entities-vs-three-tabs accepted,
staleness windows accepted as placeholders, Drive sharing deferred to
Privacy Reviewer, duplicate-row handling accepted out of scope, command
phrases accepted as proposed, operational-reality doc deferred, multiple-
editor question already out of scope) — recorded inline in the spec's own
Open Questions section so its record stays accurate, not just in this log.
Folded the accepted schema/workflow/command-surface decisions into
`NOWH_Workflow_Specs.md`, `NOWH_Data_Schema.md`, and
`NOWH_Plugin_Packaging_Plan.md`. Also **codified Open Question #9 as a
standing rule in `CLAUDE.md` Section 26**: any new data source or any
widening of an already-approved source's fields/scope is always CEO-level,
full stop — a prior narrow grant is not blanket permission to widen later.
**Rationale:** This is the first full spec-to-acceptance cycle through the
Strategist Operating Loop — worth doing rigorously as the reference case
for every future spec review. The standing rule prevents exactly the kind
of quiet scope creep Meeting Prep will be tempted toward (it will want more
than title+time from Calendar).
**Still open before this workflow is fully closed:** Privacy & Governance
Reviewer pass (agent not yet built), actual Sheet/Doc/Drive creation in a
real Google account (outside this session's tool access), manual test
against the spec's 12 acceptance tests.

### 2026-07-02 — Morning Brief: Calendar read access approved for v1

**Decision:** CEO approved Google Calendar read access for Morning Brief's
"Meetings Needing Prep" section, rather than staying Sheet-only for v1 as
originally specced. Removes the friction of the director manually logging
today's meetings as Touchpoints rows, at the cost of a new access surface
(NOWH can now see Calendar event data, not just what's in the Sheet).
**Rationale:** CEO call — presented as a scope/privacy tradeoff per the
Escalation Rule (`CLAUDE.md` Section 26, condition 6: changes the
nontechnical director's UX and the workflow's access surface). Resolved
in favor of less director friction over minimal-surface-area conservatism.
**Follow-up required:** Architect must revise `docs/specs/morning_brief_spec.md`
to define exactly what Calendar data is read (which calendar, which
fields — event title/time only, or description/attendees too), sensitivity
handling for that data, and edge cases (no matching Entity for an
attendee, recurring/declined/tentative events, multiple calendars). This
scope decision does not itself authorize unscoped Calendar access — the
revised spec still needs Strategist acceptance before build.

### 2026-07-02 — Architect role adopted, merged into Google Workspace + Packaging Designers

**Decision:** CEO shared a drafted "RHEN Technical Architect" charter as a
pattern reference. Adopted the pattern for NOWH but restructured the agent
roster: merged the previously-planned **Google Workspace Designer** and
**Claude Cowork / Packaging Designer** into a single **NOWH Architect**
agent that owns the full technical spec (data shape + interface shape +
propagation) before Claude Code implements. NOWH's specialist roster drops
from a planned four to three: Architect, Privacy & Governance Reviewer,
QA/Demo Readiness Reviewer. Built the Architect as a real Claude Code
subagent at `.claude/agents/nowh-architect.md`, adapted in full for NOWH's
architecture (no database/backend — Google Workspace + Claude Cowork only;
no separate "Engineer" role — Claude Code's `general-purpose` fills that
function; spec output lands in `docs/specs/`, not directly in canonical
files — Strategist reviews and folds accepted decisions in).
**Rationale:** CEO explicitly chose "merge" over "add as 5th agent" or
"rigor-only, no restructure" when presented with the tradeoffs — RHEN's
Architect literally does the work NOWH had split across two designer
agents, and NOWH's thin stack doesn't justify two separate design agents.
Reducing to three agents also better matches the "no agent swarm" principle
already locked in Section 12/25.
**Noted gap:** NOWH has no equivalent of RHEN's `PM_OPERATIONAL_REALITY.md`
(documented failure modes per workflow). Flagged in the Architect charter
as something to raise via Open Questions if a milestone needs it — not
built yet, not blocking.

### 2026-07-02 — Strategist Operating Loop and Escalation Protocol locked

**Decision:** Adopted the full NOWH Strategist Operating Loop and Escalation
Protocol into `CLAUDE.md` (new Sections 22–30: Operating Loop, CEO Role,
Strategist Role, Agent Role, Issue Handling and Escalation, Strategist
Delegation Standard, CEO Summary Standard, Default First Step, Core Rule).
The Strategist now operates as project manager/execution supervisor, not a
passive advisor: converts CEO milestones into scoped work, delegates to
Claude Code and the four specialist agents, resolves ordinary issues using
canonical docs without CEO involvement, and escalates only against the
8-condition Escalation Rule (docs don't answer it, locked direction, privacy/
trust/grantee-burden tradeoff, demo scope change, new cost, nontechnical-UX
change, reopening a locked decision, meaningful mission risk). CEO updates
must be decision-level summaries, not raw agent output.
**Rationale:** CEO explicitly wants to set direction and approve major
decisions without having to manage every agent conversation, field name, or
minor fix — the Strategist should triage and resolve routine issues
independently using the canonical docs as the source of truth.
**Applied immediately:** the stale `NOWH_Handoff` reference in the pasted
protocol (superseded by `NOWH_Strategist_Continuity.md`, see prior decision)
was resolved silently per this same protocol — Section 20's canonical list
already had the correct name, so no escalation was needed.

### 2026-07-02 — GitHub repo: separate, public, structure-only

**Decision:** NOWH gets its own dedicated GitHub repo (`nowh-project`),
fully separate from the user's other GitHub projects — no shared history
or remote. Repo is **public**. Local commits happen after each meaningful
unit of work; pushes to the public remote happen at milestones. The repo
holds structure, code, workflow definitions, and agent behavior only —
**never** sensitive nonprofit data. Real data (donor/grantee/financial info)
always lives in each director's own linked Google Workspace (Sheets, Docs,
Drive, Calendar), never in NOWH's codebase or git history. Added
`.gitignore` covering credentials/secrets/env files as defense in depth.
**Rationale:** Since NOWH's data model never stores real data locally
(it accesses each user's own Google account via connectors), there's no
structural path for PII to enter the repo — public visibility is safe under
this model. Public was still a deliberate choice (not default) since this is
a pre-launch product; private was offered and declined. Milestone-based
pushes keep the public commit history meaningful.

### 2026-07-02 — Locked architecture: Claude Cowork + Google Workspace

**Decision:** "Claude Cowork runs the work. Google stores the truth. NOWH
defines the rules." No MCP adapters, local servers, custom OAuth
infrastructure, or heavy local apps for the demo unless explicitly instructed.
Google Apps Script is an optional later layer, not the first operating
interface.
**Rationale:** Nontechnical Mac-based nonprofit director must be able to
install and run the demo with minimal setup burden; cost and complexity are
product constraints for a nonprofit-facing tool.
