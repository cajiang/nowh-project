# NOWH Agent Architecture

> **Onboarding rule:** every new custom NOWH agent's system prompt/charter
> must include, or point to, [NOWH_Project_Overview.md](NOWH_Project_Overview.md)
> as required first-read context. It is the self-contained briefing on what
> NOWH is, why it exists, and what it must never become — agents should not
> be expected to infer that from their narrow task alone.

## Principle

Use Claude Code's built-in agents where possible. Only create custom agents
where NOWH-specific judgment is required. Do not create a large agent swarm
early — keep strategist and architecture supervisor roles in the strategist
chat and project files for now.

## Built-in Claude Code Agents

1. **Explore** — inspect files and understand project structure without editing.
2. **Plan** — plan implementation before changes.
3. **general-purpose** — file creation, edits, packaging, implementation.

## Custom NOWH Build Agents (three)

> Originally scoped as four agents (a separate Google Workspace Designer and
> Claude Cowork/Packaging Designer). Merged into a single **NOWH Architect**
> on 2026-07-02 — see [NOWH_Decision_Log.md](NOWH_Decision_Log.md). NOWH's
> "stack" is thin enough (no backend, no database) that data-shape design
> and interface-shape design are two views of one spec, not two domains.

### 1. NOWH Architect
Owns technical design and specification: Google Sheet schema, Drive
folders, Docs templates, Gmail/Calendar conventions, Claude Cowork
command/plugin contracts, and propagation impact across the six core
workflows. Translates Strategist/CEO-approved requirements into
unambiguous specs for Claude Code to implement. Does not implement.
Reports to the Strategist; does not report directly to the CEO.

**Built** — full charter at
[`.claude/agents/nowh-architect.md`](../.claude/agents/nowh-architect.md).
Adapted from a RHEN Technical Architect charter pattern (context intake
requirement, locked-stack table, 10-section spec format, propagation
checklist, anti-bloat rule, escalation triggers) but rewritten entirely for
NOWH's actual architecture — no database, no backend language, Google
Workspace + Claude Cowork only.

### 2. NOWH Privacy & Governance Reviewer
Reviews privacy, sensitive data, approvals, grantee burden, consent, and
no-surveillance rules. Charter not yet written in full — draft when first
invoked.

### 3. NOWH QA / Demo Readiness Reviewer
Tests whether the system works for a nontechnical nonprofit director using
Mac OS and Claude Cowork. Charter not yet written in full.

## Status

Architect is built and invokable. Privacy & Governance Reviewer and QA/Demo
Readiness Reviewer charters are not yet written — draft each when first
needed, per the Default Build Sequence.

## Future Runtime Modules (not build agents — eventual product features)

Director Briefing, Open Loops, Approval Gatekeeper, Relationship Memory,
Privacy Review, Grantee Burden Check, Funder Stewardship, Board/Founder
Prep, Narrative & Consent, Grants/Money/Compliance.
