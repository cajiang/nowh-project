# NOWH Data Schema

## System of Record

**NOWH Director Workspace** — a Google Sheet. This is the first durable data
layer.

### Tabs — Originated (column-level schema accepted, 2026-07-02)

Originated by the Morning Brief spec ([specs/morning_brief_spec.md](specs/morning_brief_spec.md)
Section 3.1) — full column definitions, types, required flags, sensitivity
levels, and valid-value enums live there; this file tracks status only, not
duplicate schema detail.

1. **Entities** — thin universal lookup/join tab (id, name, type, status,
   default sensitivity, notes). Deliberately not three separate tabs
   (Grantees/Funders-Donors/Board & Founder) — see Open Question #1 in the
   spec, resolved as accepted. Later workflows needing per-type depth
   (Grantee Burden Check, Funder Stewardship, Board/Founder Prep) extend
   this via Entity ID join, not redesign.
2. **Touchpoints** — contact history, feeds staleness/care-prompt detection.
3. **Open Loops** — commitments, follow-ups, deadlines. Shared with the
   future standalone Open Loops workflow.
4. **Approval Queue** — pending human-approval items. Action Type enum
   matches the locked approval-required list in `CLAUDE.md` Section 11 and
   `NOWH_Privacy_and_Governance.md` exactly.
5. **Daily Brief Items** — director-authored one-off notes, self-expiring.

### Tabs — Not Yet Originated

Grantees, Funders/Donors, Board & Founder, Grants & Money, Narrative &
Consent — deferred until a workflow that actually needs their depth is
specced (Grantee Burden Check, Funder Stewardship, Board/Founder Prep,
Narrative & Consent). Will extend `Entities` via Entity ID join, not
duplicate it.

Schema design work is now the **NOWH Architect**'s responsibility (merged
from the originally-planned Google Workspace Designer — see
[NOWH_Agent_Architecture.md](NOWH_Agent_Architecture.md) and
[NOWH_Decision_Log.md](NOWH_Decision_Log.md)), done per the Default Build
Sequence Step 3 for each workflow as it's built — not all at once.

## Google Docs (outputs)

Morning briefs, meeting prep briefs, approval packets, board briefs, founder
briefs, funder update drafts, grantee check-in drafts, weekly reviews.

## Google Drive (storage)

Source documents, templates, approved materials, private notes, board/founder
materials, narrative assets, demo workspace files.

## Gmail & Calendar

Used as live work context, with conservative privacy defaults (see
[NOWH_Privacy_and_Governance.md](NOWH_Privacy_and_Governance.md)).

**Calendar — first real usage, 2026-07-02 (Morning Brief):** director's
primary calendar only, read-only, title + start/end time only (no
attendees, description, location). Live read at brief-generation time,
never persisted. See spec Section 3.2.1 for full rationale. Any future
widening of this scope (new fields, more calendars) or any Gmail access at
all requires its own CEO-level escalation per the standing rule in
`CLAUDE.md` Section 26 — a prior narrow grant is not blanket permission.

**Gmail — not yet used by any workflow.**

## Status

Schema originated for Morning Brief (see above) and accepted by the
Strategist, 2026-07-02. Actual Sheet/Docs/Drive structure not yet built in
any real Google account — spec is ready to build, pending the Privacy &
Governance Reviewer pass.
