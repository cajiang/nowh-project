# NOWH Current Status

> Update this section at the end of every session. For the always-current
> cross-session handoff, see [NOWH_Strategist_Continuity.md](NOWH_Strategist_Continuity.md)
> — this file is a general summary, that one is the living operating record.

## Phase

**V1 Definition — Pre-Build.** V1 was redefined 2026-07-02: NOWH operates
Google Calendar (full read/write, including external invites with an
approval gate) and Gmail (summarize-only, unread/primary inbox) through
Claude Cowork. No custom Sheet data layer required for V1. The original
six-workflow Sheet-based system is not discarded — re-sequenced as **V2**,
built on top of V1 once proven.

## Completed

- [x] Project folder created, fully separate from PM_OS/RHEN-OS
- [x] `CLAUDE.md` strategist prompt + full Operating Loop/Escalation
      Protocol locked (Sections 1–30)
- [x] 12 canonical `docs/` files scaffolded and maintained
- [x] GitHub repo live at https://github.com/cajiang/nowh-project (public),
      `gh` CLI authenticated, local-only git identity, `.gitignore` in place
- [x] Specialist agent roster set to three (Architect, Privacy & Governance
      Reviewer, QA/Demo Readiness Reviewer). **Architect built** as a real
      Claude Code subagent at `.claude/agents/nowh-architect.md`
- [x] **V2 groundwork:** Morning Brief spec accepted
      (`docs/specs/morning_brief_spec.md`) — 5-tab Sheet schema originated
      (Entities, Touchpoints, Open Loops, Approval Queue, Daily Brief
      Items). Not the current build target, but carries forward as V2's
      starting point.
- [x] **V1 scope locked** (2026-07-02, see Decision Log): Calendar full
      operation + external invites with approval gate; Gmail
      summarize-only, unread/primary inbox; drafting and sending deferred.

## Active

- [ ] **V1 technical spec not yet written.** Next real step: delegate the
      Calendar + Gmail operation spec to the Architect, same rigor as
      Morning Brief (context intake, propagation, privacy control,
      acceptance tests).

## Not Started

- V1 technical spec (Calendar + Gmail operation)
- Privacy & Governance Reviewer and QA/Demo Readiness Reviewer subagent
  charters
- Any actual build — no Calendar/Gmail integration exists yet, no Sheet
  exists in any real Google account
- Plugin packaging (depends on V1 being built and proven first)

## Known Constraints

- This session's working directory is not the NOWH folder, so `.claude/agents/`
  subagents aren't invokable by name via the Agent tool here — worked
  around via `general-purpose` with the charter pasted in. See
  `NOWH_Strategist_Continuity.md` "Operational Note" for the fix.
- No live Google Sheets/Calendar/Gmail/Claude Cowork connection is
  available from this session — actual building and testing of V1 needs
  the CEO's own environment.

## Immediate Next Task

Delegate the V1 Calendar + Gmail operation spec to the Architect, per the
locked scope in `CLAUDE.md` Section 6 and `NOWH_Privacy_and_Governance.md`
"V1 Data Access Surfaces."
