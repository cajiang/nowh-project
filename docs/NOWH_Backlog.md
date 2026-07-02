# NOWH Backlog

Items that are valid but not being built now. Use review labels from
[CLAUDE.md](../CLAUDE.md) Section 18 (BLOCKER / IMPORTANT / ACCEPTABLE /
DEFER / REJECT) when triaging new ideas into this list.

## Deferred — Gmail (V1 scoped to summarize-only, 2026-07-02)

- **Drafting emails** — deferred beyond V1. Explicit CEO call: "let's keep
  to being able to summarize emails first."
- **Sending emails** — deferred further than drafting. CEO: "being able to
  send might be too advanced" for now. Will need its own approval-gate
  design when picked back up (same bar as external calendar invites).
- **Broader inbox scanning** (beyond unread/primary) — deferred; V1 is
  intentionally narrow per the privacy principle of preferring labeled/
  specific sources.

## Deferred — V1 Follow-ups

- **Approval audit trail.** V1 (`docs/specs/v1_calendar_gmail_operation_spec.md`
  Section 3.4 / Open Question 4) deliberately has no durable record of
  external-facing actions confirmed/sent — the approval gate is real-time
  conversational only. A logged history of what NOWH sent, when, and who
  confirmed it would be useful for director trust/review and is a natural
  precursor to V2's Approval Queue. Not built now.

## Deferred — V2: NOWH Workflow Layer (re-sequenced 2026-07-02)

The original six-workflow model, built on the NOWH Director Workspace
Google Sheet. Not abandoned — re-sequenced behind V1 (Calendar + Gmail
operation). Morning Brief is already specced and accepted
(`docs/specs/morning_brief_spec.md`); the rest are not yet specced.

- Meeting Prep
- Open Loops (as its own workflow, beyond what Morning Brief surfaces)
- Approval Queue (as its own workflow)
- Grantee Burden Check
- Privacy Review

## Deferred — Future Runtime Modules (post-V2)

- Funder Stewardship
- Board / Founder Prep
- Narrative & Consent
- Grants / Money / Compliance
- Relationship Memory (as a standalone module beyond what Meeting Prep needs)

## Deferred — Later Architecture Layer

- Google Apps Script automation/guardrail layer (only where Claude Cowork
  proves weak — not the first operating interface)

## Deferred — Paid Tooling

- None currently proposed. Do not add paid SaaS here without explicit user
  request per the Cost Principle.

## Open Ideas (not yet triaged)

- (none yet)
