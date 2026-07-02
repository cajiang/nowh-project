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

## Status

Not yet designed. Ownership: **NOWH Claude Cowork / Packaging Designer**
agent (see [NOWH_Agent_Architecture.md](NOWH_Agent_Architecture.md)), once
the six core workflows are specced and ready to package.
