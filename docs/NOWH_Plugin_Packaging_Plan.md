# NOWH Plugin Packaging Plan

## Target

**NOWH Demo v2 Plugin** — a polished Claude Cowork plugin-style demo package
for a nontechnical nonprofit director on Mac OS with a fresh Claude Cowork
setup.

## Package Contents (target)

1. `NOWH_Director_OS_Plugin.zip`
2. A single **NOWH Director Workspace** Google Sheet template link
3. One setup phrase: "Start NOWH setup."

## Experience Goal

Install NOWH → connect Google → open Claude Cowork → type one command →
begin. No code, terminals, MCP, local servers, developer folders, schema
files, or complex configuration exposed to the director.

## Constraints

- Rely on Claude's native Google Workspace connection capabilities where
  available.
- Do not recreate MCP adapters, local servers, custom OAuth infrastructure,
  or heavy local apps unless explicitly instructed.
- No paid tools beyond the required Claude/Google subscriptions already
  in use.

## Command Surface (accruing as workflows are accepted)

- **V1 — Calendar + Gmail Operation** (accepted 2026-07-02, current build
  target): full command table in
  [specs/v1_calendar_gmail_operation_spec.md](specs/v1_calendar_gmail_operation_spec.md)
  Sections 3.1 (Calendar) and 3.3 (Gmail). **Setup for V1 is simpler than
  Morning Brief's** — no Sheet template to copy. The director connects
  Google Calendar and Gmail via Claude Cowork's own native connector
  settings (OAuth through Cowork directly), nothing NOWH-specific to set up
  beforehand. This is likely the actual first packaging target once V1 is
  built and proven, ahead of Morning Brief/V2.
  **New setup requirement (2026-07-02, from live capability testing):**
  setup instructions must direct the director to set the Google Calendar
  connector's write tools (create/edit/delete event) to **"Needs
  Approval,"** not "Always Allow," in Cowork's connector permission
  settings. This is defense-in-depth on top of NOWH's own approval-gate
  design (spec Section 3.2), not a substitute for it — testing showed the
  platform's default "Always Allow" behavior does not distinguish internal
  from external actions at all. See `NOWH_Decision_Log.md` for the full
  finding.
- **Morning Brief** (accepted 2026-07-02, parked as V2 — not current build
  target): "Run my morning brief," "What matters today," "Give me today's
  brief." Setup flow must ensure the Sheet has the five originated tabs
  (Entities, Touchpoints, Open Loops, Approval Queue, Daily Brief Items)
  before offering this as a working command — see
  [specs/morning_brief_spec.md](specs/morning_brief_spec.md) Section 5 item 8
  and Section 7 (missing-tab edge case) for the exact fallback behavior.

## Status

Not yet designed as a package. Ownership: **NOWH Architect** (merged from
the originally-planned Claude Cowork / Packaging Designer — see
[NOWH_Agent_Architecture.md](NOWH_Agent_Architecture.md) and
[NOWH_Decision_Log.md](NOWH_Decision_Log.md)), once enough of the six core
workflows are specced and ready to package together. Command surface notes
above accrue per-workflow in the meantime.
