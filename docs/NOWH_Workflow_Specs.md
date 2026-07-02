# NOWH Workflow Specs

The first demo focuses on six core workflows. Each entry below is the locked
purpose/behavior from the strategist prompt; detailed specs (trigger, input,
output, approval, data touched) should be filled in per the Default Build
Sequence Step 2 (Workflow) before each is built.

## 1. Morning Brief

**Purpose:** Answer "What matters today?"

**Includes:** decisions needed, responses needed, meetings requiring
preparation, sensitive items, open loops, funder follow-ups, grantee care
prompts, board/founder prep, money or compliance deadlines, items that can
wait, items requiring approval.

**Status:** **Accepted, 2026-07-02 — full technical spec at
[specs/morning_brief_spec.md](specs/morning_brief_spec.md).** Originates the
first slice of the NOWH Director Workspace schema (Entities, Touchpoints,
Open Loops, Approval Queue, Daily Brief Items — see
[NOWH_Data_Schema.md](NOWH_Data_Schema.md)). Reads today's events live from
the director's primary Google Calendar (title + start/end time only, never
persisted) in addition to the Sheet. Trigger phrases: "Run my morning
brief," "What matters today," "Give me today's brief." Output: a new Google
Doc per run in `NOWH Workspace/Briefs/`, plus a short conversational
summary. Read-only and internal-only — cannot approve, send, or share
anything. Not yet built (no actual Sheet/Doc/Drive structure exists) or
reviewed by the Privacy & Governance Reviewer.

## 2. Meeting Prep

**Purpose:** Prepare the director for funder, grantee, board, founder,
staff, or partner meetings.

**Includes:** who is involved, last touchpoint, relationship history, open
commitments, sensitive context, relevant documents, desired outcome, what
not to say or use without approval.

**Status:** Not yet specced.

## 3. Open Loops

**Purpose:** Answer "What am I forgetting?"

**Tracks:** promises, follow-ups, unanswered items, waiting-on-someone
items, overdue commitments, quietly approaching deadlines.

**Status:** Not yet specced.

## 4. Approval Queue

**Purpose:** Keep sensitive work under human control.

**Approval required for:** funding decisions, renewal decisions, spending
decisions, funder-facing messages, donor-facing messages, grantee-facing
messages, board-facing materials, founder name/time/likeness/public
association, youth/community stories, public narratives, sensitive
relationship notes, compliance-sensitive actions.

**Status:** Not yet specced.

## 5. Grantee Burden Check

**Purpose:** Prevent asking grantees for information the org already has.

**Checks:** prior emails, prior reports, meeting notes, public materials,
existing grantee profile, prior funder updates, prior asks made to the
grantee.

**Answers:** we already have enough / we have partial information / we need
one small clarification / we do not have this information.

**Status:** Not yet specced.

## 6. Privacy Review

**Purpose:** Protect donor, grantee, youth/community, board, founder,
staff, and financial information.

**Reviews every sensitive output for:** private information exposure,
internal notes leaking into external language, unapproved narrative use,
missing consent, grantee burden risk, donor confidentiality,
founder/board sensitivity, financial or compliance sensitivity.

**Status:** Not yet specced.

---

## Future Modules (post-demo)

Director Briefing, Open Loops, Approval Gatekeeper, Relationship Memory,
Privacy Review, Grantee Burden Check, Funder Stewardship, Board/Founder
Prep, Narrative & Consent, Grants/Money/Compliance — see
[NOWH_Backlog.md](NOWH_Backlog.md).
