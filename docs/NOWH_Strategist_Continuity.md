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
**V1 Definition — Pre-Build.** (Re-sequenced 2026-07-02 — read this whole
section before assuming anything from an earlier session still applies.)

### Locked Decisions Since Last Update
- **MAJOR: V1 redefined as Calendar + Gmail operation (2026-07-02).** Not
  the six-workflow Sheet system — that's now V2. See full rationale and
  scope in `NOWH_Decision_Log.md` ("Major re-sequencing: V1 redefined as
  Calendar + Gmail operation"). Summary:
  - **Calendar V1: full operation** on the director's own calendar
    (create/edit/delete events, reminders), **including external
    invitees** (funders, grantees, board, vendors). New approval gate:
    any event/invite touching an external party needs director approval
    before it sends — codified in `CLAUDE.md` Section 11.
  - **Gmail V1: summarize only**, unread mail in the primary inbox only.
    No drafting, no sending — both explicitly deferred (CEO walked back
    an initial draft+send idea to summarize-only). See `NOWH_Backlog.md`.
  - **Morning Brief spec + 5-tab schema are NOT discarded** — kept as V2,
    built on top of V1 once proven, not competing with it.
  - `CLAUDE.md` Sections 4, 6, 7, 11 rewritten accordingly.
    `NOWH_Privacy_and_Governance.md` got a new "V1 Data Access Surfaces"
    section. Don't trust an old session's assumption that Morning Brief is
    "next" — it isn't; the V1 Calendar/Gmail spec is next.
- **Standing rule (still active):** any new data source or widened access
  surface (Gmail, more of the inbox, more calendars, drafting, sending)
  is CEO-level by default, full stop — `CLAUDE.md` Section 26. This rule
  is *why* the V1 redefinition above was handled as a CEO decision, not
  something the Strategist resolved alone.
- **Strategist Operating Loop and Escalation Protocol locked** (`CLAUDE.md`
  Sections 22–30). The Strategist runs the CEO→Strategist→Agents→
  Strategist→CEO loop as project manager, resolving routine issues via
  canonical docs and escalating only against the 8-condition Escalation
  Rule. Triage and resolve first, escalate second, decision-level
  summaries only (Section 28) — never raw agent output to the CEO.
- Architecture: Claude Cowork runs the work / Google stores the truth /
  NOWH defines the rules — unchanged, still applies to both V1 and V2.
- Continuity model: this file is living, updated continuously, not just
  on explicit handoff request.
- `NOWH_Project_Overview.md` is required first-read context for every new
  custom NOWH build agent (still accurate for V1 — the product identity
  and privacy/approval principles didn't change, only the build sequence).

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
- **V1 spec — ACCEPTED (2026-07-02), now including a Contacts-lookup
  revision.** Full spec at `docs/specs/v1_calendar_gmail_operation_spec.md`.
  CEO approved Google Contacts as a new, targeted read-only invitee-
  resolution source (name→email only, never bulk/standing scan) — Gmail
  remains forbidden as a resolution source, unchanged. Reviewed the full
  revised file directly both times (not just agent self-reports); caught
  and fixed two real inconsistencies across the two review passes (a stale
  "no Calendar access" line in the first V1 draft, and a stale "resolve
  from Gmail" line in Section 6 that survived both revisions because
  Section 6 wasn't in either revision's explicit scope). **Process
  learning: grep the full document for terms under a hard constraint
  before accepting a revision, not just the sections the revision was
  scoped to touch.**
  **Open Question 1 resolved, corrected from an earlier misread.** Live
  test showed a confirmation only on the *first* Calendar write action, not
  subsequent ones — this is Cowork's generic per-tool permission setting
  (Always Allow / Needs Approval / Blocked), not a content-aware gate. Once
  a tool is Always Allow, the platform will not stop an external send.
  **Section 3.2's design was already built to be the sole enforcement
  regardless of platform behavior — this finding validates that caution,
  doesn't require a design change.** New action item: setup must instruct
  the director to set Calendar write tools to "Needs Approval" as
  defense-in-depth (`NOWH_Plugin_Packaging_Plan.md`, updated). I initially
  logged the first two-word report ("it gave approval gates") as a
  confirmed redundant safety net without asking for detail first — corrected
  once the fuller picture came in; lesson logged in the Decision Log.
  **Remaining before V1 is fully closed:** record the CEO's test finding;
  build Privacy & Governance Reviewer agent and run it against this spec;
  actually implement and test in a real Google/Cowork environment (outside
  this session's tool access).
- Research finding worth remembering: **do not use Composio or any
  third-party MCP/connector middleware** to connect Calendar/Gmail — some
  online guides suggest it, but it violates the locked "no MCP adapters"
  architecture and would require a nontechnical director to configure a
  server URL. Use only Claude Cowork's native, first-party Google
  Workspace connector (no API keys, no dev setup, director just
  authenticates directly with Google).
- **V2 (Morning Brief) is parked, not abandoned.** Spec accepted at
  `docs/specs/morning_brief_spec.md`, 5-tab schema in
  `NOWH_Data_Schema.md`. Don't resume building it until V1 is proven —
  but don't let a future session think it needs to be re-specced from
  scratch either.

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
- V1 Calendar + Gmail operation spec (the actual current priority)
- Privacy & Governance Reviewer and QA/Demo Readiness Reviewer subagent
  charters (Architect is done; these two remain)
- Any real build — no Calendar/Gmail integration exists, no Sheet exists
  in any real Google account
- Claude Cowork plugin packaging (depends on V1 being built and proven)

### Open Decisions
- None blocking right now — V1 scope is fully locked (Calendar
  full-operation + external-invite approval gate; Gmail summarize-only,
  unread/primary inbox). Next open decision will likely surface once the
  Architect drafts the V1 spec (e.g., exact Calendar event fields,
  reminder mechanics, email summarization format).

### Risks and Watchouts
- Calendar external invites are a real new capability — the approval gate
  (director must approve before an invite reaches an external party) is
  not optional, do not let a build spec soften it
- Do not let Gmail scope creep past unread/primary inbox without a fresh
  CEO decision (standing rule, `CLAUDE.md` Section 26)
- Grantee-facing workflows (V2, when resumed) must not drift into
  surveillance/scoring
- Restricted-sensitivity data must not leak into external-facing drafts
- No paid tools without explicit user approval

### Out-of-Scope (V1)
- Gmail drafting and sending (deferred, see `NOWH_Backlog.md`)
- MCP adapters, local servers, custom OAuth infra, heavy local apps
- CRM-style grantee scoring/ranking
- The V2 Sheet-based workflow layer (parked, not cancelled)

### Immediate Next Steps
1. Delegate the V1 Calendar + Gmail operation spec to the Architect (or
   `general-purpose` with the charter pasted in, if still working from a
   session not rooted at the NOWH folder — see Operational Note below)
2. Strategist reviews the spec directly (not just the agent's self-report)
   before accepting, same process used for Morning Brief
3. Build Privacy & Governance Reviewer agent — needed before any spec with
   this much new access-surface exposure should be considered fully closed

---

## 5. Session Log

> Append-only. One short entry per session/Strategist tenure. Keep entries
> to 1–3 lines — detail belongs in Section 4 or the Decision Log, not here.

- **2026-07-02** — Initial Strategist. Set up project folder, `CLAUDE.md`,
  all canonical `docs/` files, and this continuity file. No workflows built
  yet. Handed off with next step = pick first workflow to spec.
- **2026-07-02 (same day, continued session)** — Set up GitHub repo, locked
  the Operating Loop/Escalation Protocol, built the Architect agent
  (merging two planned agents into one), fully specced and accepted
  Morning Brief (5-tab schema + Calendar read access) — then the CEO
  reassessed and **redefined V1 as Calendar + Gmail operation**, parking
  Morning Brief as V2. Handed off with next step = delegate the V1 spec to
  the Architect.
