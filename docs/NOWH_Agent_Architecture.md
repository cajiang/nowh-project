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

## Custom NOWH Build Agents (only four, first)

### 1. NOWH Privacy & Governance Reviewer
Reviews privacy, sensitive data, approvals, grantee burden, consent, and
no-surveillance rules. Charter not yet written in full — draft when first
invoked.

### 2. NOWH Google Workspace Designer
Designs the Google Sheet schema, Drive folders, Docs templates, Gmail
labels, Calendar conventions, and permissions model. Charter not yet
written in full.

### 3. NOWH Claude Cowork / Packaging Designer
Designs the Claude Cowork plugin experience, command library, Skill/Plugin
structure, setup flow, and Mac-friendly demo. Charter not yet written in
full.

### 4. NOWH QA / Demo Readiness Reviewer
Tests whether the system works for a nontechnical nonprofit director using
Mac OS and Claude Cowork. Charter not yet written in full.

## Status

None of the four custom agents have been instantiated yet. Full charters
(role, inputs, outputs, review labels used, escalation rules) should be
written when each is first needed, per the Default Build Sequence.

## Future Runtime Modules (not build agents — eventual product features)

Director Briefing, Open Loops, Approval Gatekeeper, Relationship Memory,
Privacy Review, Grantee Burden Check, Funder Stewardship, Board/Founder
Prep, Narrative & Consent, Grants/Money/Compliance.
