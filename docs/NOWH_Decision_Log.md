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
