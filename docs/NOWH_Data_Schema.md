# NOWH Data Schema

## System of Record

**NOWH Director Workspace** — a Google Sheet. This is the first durable data
layer.

### Initial Tabs (proposed, not yet finalized)

1. Entities
2. Grantees
3. Funders / Donors
4. Touchpoints
5. Open Loops
6. Approval Queue
7. Grants & Money
8. Board & Founder
9. Narrative & Consent
10. Daily Brief Items

Column-level schema for each tab has not been designed yet. This is the
responsibility of the **NOWH Google Workspace Designer** agent (see
[NOWH_Agent_Architecture.md](NOWH_Agent_Architecture.md)) and should be done
per the Default Build Sequence Step 3 (Data Structure) for each workflow as
it's built — not all at once.

## Google Docs (outputs)

Morning briefs, meeting prep briefs, approval packets, board briefs, founder
briefs, funder update drafts, grantee check-in drafts, weekly reviews.

## Google Drive (storage)

Source documents, templates, approved materials, private notes, board/founder
materials, narrative assets, demo workspace files.

## Gmail & Calendar

Used as live work context, with conservative privacy defaults (see
[NOWH_Privacy_and_Governance.md](NOWH_Privacy_and_Governance.md)).

## Status

Not yet built. No sheet, docs, or Drive structure exist yet.
