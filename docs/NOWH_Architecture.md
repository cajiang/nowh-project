# NOWH Architecture

## Locked Architecture

> Claude Cowork runs the work.
> Google stores the truth.
> NOWH defines the rules.

### Operating Interface — Claude Cowork

The user-facing operating interface. The director opens Claude Cowork,
enters the NOWH environment, and runs workflows conversationally.

Example commands:
- "Run my morning brief."
- "Prepare me for this meeting."
- "What needs my approval?"
- "What am I forgetting?"
- "Check if we already know this before asking the grantee."

### System of Record — Google Workspace

The durable system of record: Google Sheets, Google Docs, Google Drive,
Gmail, Google Calendar.

### Package Target — NOWH Demo v2 Plugin

A polished Claude Cowork plugin-style demo package for a nontechnical
nonprofit director on Mac OS with a fresh Claude Cowork setup. Relies on
Claude's native Google Workspace connection capabilities where available.

**Do not** recreate MCP adapters, local servers, custom OAuth infrastructure,
or heavy local apps for the demo unless explicitly instructed.

### Optional Later Layer — Google Apps Script

May be added later as an automation or guardrail layer where Claude Cowork
is weak. Not the first operating interface.

## Cost Principle

Cost is a product constraint. The demo director should not pay for anything
beyond the required AI subscription. Default approved tools: Claude Cowork,
Google Workspace (Sheets, Docs, Drive, Gmail, Calendar). Avoid paid SaaS
recommendations unless explicitly requested.

## Demo Target Package

1. `NOWH_Director_OS_Plugin.zip`
2. A single **NOWH Director Workspace** Google Sheet template link
3. One setup phrase: "Start NOWH setup."

Experience goal: install NOWH → connect Google → open Claude Cowork → type
one command → begin. No code, terminals, MCP, local servers, developer
folders, schema files, or complex configuration exposed to the director.

Full packaging plan: [NOWH_Plugin_Packaging_Plan.md](NOWH_Plugin_Packaging_Plan.md)
Full data schema: [NOWH_Data_Schema.md](NOWH_Data_Schema.md)

## Repository & Version Control

NOWH has its own dedicated GitHub repo (`nowh-project`), fully separate
from any other project on the user's GitHub account — no shared history or
remote. The repo is **public**.

**What lives in the repo:** structure, docs, code, workflow definitions,
agent behavior/charters, plugin packaging.

**What never lives in the repo:** real nonprofit data — donor info,
grantee info, financials, private relationship notes, youth/community
stories. Real data always lives in each director's own linked Google
Workspace (Sheets, Docs, Drive, Calendar) via connectors, never in NOWH's
codebase or git history. This is a structural guarantee, not just policy —
NOWH doesn't have a local data store to leak from. `.gitignore` covers
credentials/secrets/env files as defense in depth.

**Commit cadence:** commit locally after each meaningful unit of work; push
to the public GitHub remote at milestones.
