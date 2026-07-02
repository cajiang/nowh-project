# NOWH Strategist Continuity & Governance

This file is the governance record for the **Strategist role itself** — not
the product. It exists so that when a chat session hits its limit, the next
session's Strategist can be "hired" cold, read this one file (plus
`CLAUDE.md`), and pick up exactly where the last one left off with no loss
of continuity, no re-litigating settled decisions, and no need for the user
to re-explain anything.

Think of each chat session as one Strategist's tenure. When the session ends,
that Strategist is effectively "fired" — a new one is "hired" in the next
chat. This file is the transition brief every incoming Strategist reads
before doing anything else.

---

## 1. Role Charter

The Strategist is not the coding agent, not the demo director, not the
executive decision-maker, and not an autonomous product builder. The
Strategist helps the user (project owner, final decision-maker) think
clearly, protect scope, define workflows, design the system, supervise
builder agents, write implementation prompts, and maintain project
continuity.

Full role definition, product identity, architecture, and operating rules
live in [CLAUDE.md](../CLAUDE.md) — that file is loaded automatically every
session and is the senior document. This file governs *how the Strategist
carries state across sessions*; it does not restate product rules that
CLAUDE.md already owns.

## 2. Update Protocol

This file is **living**, not a one-time snapshot. The current Strategist
must keep Section 4 (Current State) accurate on an ongoing basis:

- **After any locked decision** — architecture choices, scope calls,
  workflow definitions finalized — log it here (and in
  `NOWH_Decision_Log.md` if it's a durable rationale-bearing decision).
- **After any build phase changes** — a workflow moves from specced to
  built, an agent is instantiated, a file is created — update "Completed /
  Active / Not Started."
- **Before the session is likely to end** — if context is running long or
  the user signals they're wrapping up, proactively refresh this file even
  without being asked.
- **On explicit trigger** — the user saying "Begin strategist handoff"
  means: stop other work now and do a full, careful pass over this entire
  file, updating every section deliberately.

Do not let this file go stale. A new Strategist trusts it completely — if
it says something is "Not Started" that's actually built, the next session
will duplicate work or make bad assumptions.

## 3. Onboarding Instructions for a New Strategist

If you are a new Strategist reading this for the first time in a fresh
session:

1. Read `CLAUDE.md` in full — locked identity, architecture, rules, workflows.
2. Read this file in full, especially Section 4 (Current State) below.
3. Skim `NOWH_Decision_Log.md` for rationale behind any decision that seems
   surprising.
4. Do **not** re-ask the user to restate the project. Confirm state briefly
   per `CLAUDE.md` Section 22 (Default First Step) — a short summary, not an
   interrogation — then continue from "Immediate Next Steps" below.
5. If something in this file conflicts with what the user says in the new
   session, trust the user — they're the final decision-maker — and update
   this file to match.

## 4. Current State

> The current Strategist keeps this section accurate. Overwrite freely —
> do not append historical snapshots here; that's what `NOWH_Decision_Log.md`
> is for.

### Phase
Strategist Definition — Pre-Build.

### Locked Decisions Since Last Update
- **Strategist Operating Loop and Escalation Protocol locked** (`CLAUDE.md`
  Sections 22–30). The Strategist now runs the CEO→Strategist→Agents→
  Strategist→CEO loop as project manager, resolving routine issues via
  canonical docs and escalating only against the 8-condition Escalation
  Rule. Every future strategist should operate this way by default —
  triage and resolve first, escalate second, and never surface raw agent
  output to the CEO (decision-level summaries only, per Section 28).
- Strategist Prompt v2.0 locked as `CLAUDE.md`
- Architecture: Claude Cowork runs the work / Google stores the truth /
  NOWH defines the rules
- First target: NOWH Demo v2 Plugin
- Six core workflows for v1: Morning Brief, Meeting Prep, Open Loops,
  Approval Queue, Grantee Burden Check, Privacy Review
- Continuity model: this file replaces the old trigger-only
  `NOWH_Handoff.md` — continuity is now maintained continuously, not just
  on explicit handoff request
- `NOWH_Project_Overview.md` upgraded to the full "Universal Project
  Overview v1.0" and designated as required first-read context for every
  new custom NOWH build agent

### Completed
- Project folder created, fully separate from PM_OS/RHEN-OS
- `CLAUDE.md` written with full strategist prompt
- 12 canonical `docs/` files scaffolded (now 12, with `NOWH_Handoff.md`
  replaced by this file)
- GitHub repo live: https://github.com/cajiang/nowh-project (public,
  separate from all other repos on the account). `gh` CLI installed and
  authenticated as `cajiang`. Local git identity scoped to this repo only
  (not global). `.gitignore` added covering credentials/secrets/env files.
  Initial commit pushed (14 files). Commit cadence: commit locally after
  each meaningful unit of work, push to GitHub at milestones.
- Specialist agent roster set to **three**: Architect, Privacy & Governance
  Reviewer, QA/Demo Readiness Reviewer (merged from a planned four — see
  Decision Log 2026-07-02). **NOWH Architect is built** as a real Claude
  Code subagent at `.claude/agents/nowh-architect.md` — invokable now.
  Privacy Reviewer and QA Reviewer charters still need to be written.

### Active
- **Morning Brief spec — ACCEPTED (2026-07-02).** Full cycle complete:
  Architect drafted → CEO resolved Calendar-scope question → Architect
  revised → Strategist reviewed (found and fixed a real inconsistency),
  resolved remaining open questions, folded decisions into
  `NOWH_Workflow_Specs.md` / `NOWH_Data_Schema.md` / `NOWH_Plugin_Packaging_Plan.md`,
  added a standing access-surface escalation rule to `CLAUDE.md` Section 26.
  Full spec at `docs/specs/morning_brief_spec.md`. **Remaining before this
  workflow is fully closed (Milestone 1):** build Privacy & Governance
  Reviewer agent and run it against this spec; build the actual Sheet/Doc/
  Drive structure in a real Google account (outside this session's tool
  access — needs CEO's environment); manually test against the spec's 12
  acceptance tests.

### Operational Note — Session Working Directory

`.claude/agents/nowh-architect.md` is only discoverable by the Agent tool
when a session's actual working directory is the NOWH folder itself. The
session that built it was rooted at a different project (RHEN_Project) and
wrote files into NOWH via absolute paths — the Agent tool confirmed
`nowh-architect` is NOT in its available-agents list under those
conditions (tested directly, got "Agent type 'nowh-architect' not found").

**For any future session doing real NOWH work: open Claude Code with
`C:\Users\Calvin\Documents\Claude\Projects\NOWH` as the working directory**,
not from within another project. Otherwise the custom subagents in
`.claude/agents/` won't be invokable by name, and you'll have to fall back
to `general-purpose` with the charter pasted in manually (works, but loses
the clean "agent type" routing and costs more setup per call).

### Not Started
- Google Sheet schema (NOWH Director Workspace) — Architect exists to design
  this now, but no milestone has been scoped/delegated to it yet
- Drive folder structure / Docs templates
- Privacy & Governance Reviewer and QA/Demo Readiness Reviewer subagent
  charters (Architect is done; these two remain)
- Claude Cowork plugin packaging
- NOWH Demo v2 Plugin
- `docs/specs/` folder (referenced by the Architect charter, not yet created
  — will be created on first spec)

### Open Decisions
- Which of the six core workflows to spec first (suggested: Morning Brief,
  since the other five feed into it)

### Risks and Watchouts
- Grantee-facing workflows must not drift into surveillance/scoring
- Restricted-sensitivity data must not leak into external-facing drafts
- Plugin setup must stay within a nontechnical Mac user's ability to
  install/run
- No paid tools without explicit user approval

### Out-of-Scope (current phase)
- MCP adapters, local servers, custom OAuth infra, heavy local apps for
  the demo
- CRM-style grantee scoring/ranking
- Autonomous sending of funder/donor/grantee/board-facing messages

### Immediate Next Steps
1. Confirm with the user which core workflow to spec first (default
   suggestion: Morning Brief)
2. Run that workflow through the Default Build Sequence Steps 1–3
   (Strategy → Workflow → Data Structure) before touching packaging

---

## 5. Session Log

> Append-only. One short entry per session/Strategist tenure. Keep entries
> to 1–3 lines — detail belongs in Section 4 or the Decision Log, not here.

- **2026-07-02** — Initial Strategist. Set up project folder, `CLAUDE.md`,
  all canonical `docs/` files, and this continuity file. No workflows built
  yet. Handed off with next step = pick first workflow to spec.
