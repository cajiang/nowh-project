# NOWH Privacy and Governance

## Privacy-First Principle

Privacy is a core product requirement. NOWH must protect: donor and funder
information, grantee information, youth and community stories, personal
contact information, grant amounts and payment records, board and founder
communications, internal strategy, sensitive relationship notes,
consent-limited narratives, compliance-sensitive information.

## Default Privacy Rules

- Collect only what is operationally useful
- Use least-privilege access
- Label sensitivity clearly
- Separate internal notes from approved external language
- Track approvals for sensitive use
- Do not expose private notes in external drafts
- Do not feed unnecessary private data into AI tools
- Do not scan broad data sources when narrower labeled sources are enough
- When in doubt, classify information as more sensitive

## Sensitivity Levels

- **Low:** general operational information
- **Medium:** relationship context or internal coordination
- **High:** donor/grantee financials, private concerns, board/founder strategy
- **Restricted:** youth stories, consent-limited narratives, personal
  information, sensitive donor or grantee context

## Trust-Based Nonprofit Guardrails

NOWH must support trust-based, grantee-centered nonprofit practice.

- Must **not** turn grantmaking into surveillance
- Must **not** push extra reporting burden onto grantees
- Must **not** score, rank, or judge grantees, donors, youth, communities,
  or organizations

May surface factual context: last contact date, open commitments, known
constraints, upcoming deadlines, existing notes, prior reports, missing
information, possible follow-up needs — but grantee silence or missing
information must be framed as a care-based prompt, not a compliance alarm.

- Preferred: "Consider a low-burden check-in."
- Avoid: "This grantee is underperforming."

## Human Approval Rules

NOWH may summarize, organize, surface, draft, and recommend next steps.
NOWH must **not** independently decide or execute sensitive actions.

Approval required for: funding decisions, grant renewals, spending,
funder-facing messages, donor-facing messages, grantee-facing messages,
board materials, founder use, public narratives, youth/community stories,
sensitive relationship notes, compliance-sensitive actions, and any
calendar event/invite that adds or notifies an external party (added
2026-07-02 with V1 Calendar operation — see `NOWH_Decision_Log.md`).

The system can prepare the director. It cannot replace the director.

## V1 Data Access Surfaces (added 2026-07-02)

- **Calendar:** full read/write on the director's own primary calendar
  (create, edit, delete events/reminders). External invitees require
  approval per the Human Approval Rules above before the invite is sent.
- **Gmail:** read + summarize only, scoped to **unread mail in the primary
  inbox** — not a full historical search, not other labels/folders. No
  drafting, no sending in V1 (deferred, see `NOWH_Backlog.md`). Matches
  "do not scan broad data sources when narrower labeled sources are
  enough."
- Any widening of either surface (more of the inbox, other calendars,
  drafting, sending) is CEO-level per the standing rule in `CLAUDE.md`
  Section 26 — not something a future spec can quietly expand.
- **Full approval-gate design accepted, 2026-07-02:**
  [specs/v1_calendar_gmail_operation_spec.md](specs/v1_calendar_gmail_operation_spec.md)
  Section 3.2. Key point: the gate is specified as NOWH's own conversational
  requirement (exact preview contents, exact confirmation rules, no
  partial-credit, no default-yes), independent of whatever Claude Cowork's
  underlying connector does by default — it does not rely on unverified
  platform behavior to hold. Invitee email resolution is similarly
  constrained: NOWH only ever uses what the director directly states in
  conversation, never a Gmail search or lookup (spec Section 3.1/10).

## Conflict Rules

- If a feature helps the director but burdens grantees → redesign it.
- If a workflow exposes private information unnecessarily → restrict it.
- If AI can draft but the content is sensitive → require approval.
- If a feature scores, ranks, or judges people or organizations → reject it.
- If a CRM-style feature turns grantees into pipeline objects → reject or
  reframe it.
- If privacy and convenience conflict → preserve privacy.
