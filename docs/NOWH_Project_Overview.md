# NOWH Universal Project Overview v1.0

## Nonprofit Organization Workflow Hub

> **Purpose of this file:** this is the self-contained briefing document for
> anyone or anything joining NOWH work without full session context —
> most importantly, **every new custom NOWH build agent** (Privacy &
> Governance Reviewer, Google Workspace Designer, Cowork/Packaging Designer,
> QA/Demo Readiness Reviewer) should be pointed to this file as part of its
> first read, alongside its own charter in
> [NOWH_Agent_Architecture.md](NOWH_Agent_Architecture.md). It stands on its
> own — a reader shouldn't need to open every other canonical file to
> understand what NOWH is and why it exists.
>
> For locked strategist rules, review labels, and the build sequence, see
> [CLAUDE.md](../CLAUDE.md). For cross-session continuity, see
> [NOWH_Strategist_Continuity.md](NOWH_Strategist_Continuity.md).

---

## 1. What NOWH Is

NOWH stands for:

**Nonprofit Organization Workflow Hub**

NOWH is a work-in-progress **agentic operating system for nonprofit directors**.

Its purpose is to help directors of nonprofit organizations pursue their missions by reducing operational overwhelm, organizing relationship and decision context, protecting sensitive information, avoiding unnecessary burden on grantees and community partners, and keeping human approval at the center of sensitive nonprofit work.

NOWH is being designed first around the Executive Director seat at the Jeremy Lin Foundation as a reference case, but the long-term goal is to support directors of small and lean nonprofit organizations more broadly.

---

## 2. Why NOWH Exists

Lean nonprofit directors often carry too much context in their heads.

They are responsible for:

* Funder and donor stewardship
* Grantee relationships
* Board preparation
* Founder coordination
* Team coordination
* Grant and money tracking
* Compliance awareness
* Sensitive storytelling
* Relationship memory
* Daily prioritization
* Mission execution

In small organizations, almost everything routes through the director.

The problem is not only that the director has many tasks.

The deeper problem is that every task carries hidden context:

* Who cares about this?
* What did we promise?
* What has already been said?
* What is sensitive?
* What requires approval?
* What should not be asked of a grantee?
* What can wait?
* What cannot slip?

NOWH exists to reduce that cognitive burden.

---

## 3. What NOWH Is Not

NOWH is not a CRM.

NOWH is not a donor database.

NOWH is not a grantee portal.

NOWH is not a generic project management tool.

NOWH is not an autonomous AI decision-maker.

NOWH is not a system for ranking, scoring, or surveilling grantees.

NOWH is not intended to add more reporting burden to grassroots organizations.

NOWH is an operating system for the director's work.

It helps the director remember, prepare, decide, approve, and protect.

---

## 4. Core Product Belief

The central belief behind NOWH is:

> Nonprofit directors do not need more dashboards. They need mission-aware operating support that remembers context, protects relationships, and prepares decisions without taking control away from the human.

NOWH should help a director walk into every conversation already briefed, every decision with the right context, and every sensitive action with a clear approval gate.

---

## 5. Locked Architecture

The locked architecture is:

> Claude Cowork runs the work.
> Google stores the truth.
> NOWH defines the rules.

### Claude Cowork

Claude Cowork is the operating interface.

The director interacts with NOWH conversationally inside Claude Cowork.

Example commands:

* "Run my morning brief."
* "Prepare me for this meeting."
* "What needs my approval?"
* "What am I forgetting?"
* "Check if we already know this before asking the grantee."

### Google Workspace

Google Workspace is the system of record.

NOWH uses:

* Google Sheets for structured data
* Google Docs for outputs
* Google Drive for documents and templates
* Gmail for communication context
* Google Calendar for time and meeting context

### NOWH Plugin

The first shipping target is a polished **NOWH Demo v2 Plugin** for Claude Cowork.

The demo should be usable by a nontechnical nonprofit director on Mac OS with a fresh Claude Cowork setup.

The director should not need to understand code, terminal commands, local servers, MCP adapters, or complex configuration.

---

## 6. Current Demo Target

The current target is:

**NOWH Demo v2 Plugin**

The demo director should receive:

1. `NOWH_Director_OS_Plugin.zip`
2. A single **NOWH Director Workspace** Google Sheet template link
3. One setup phrase: "Start NOWH setup."

The desired experience is:

> Install NOWH, connect Google, open Claude Cowork, type one command, and begin.

---

## 7. Core Demo Workflows

The first demo should support six workflows.

### 1. Morning Brief

The director asks:

> "Run my morning brief."

NOWH should surface:

* What needs decision today
* What needs response today
* What meetings need prep
* What is sensitive
* What is overdue
* What can wait
* What funder or grantee relationship needs attention
* What requires approval
* What might create grantee burden

### 2. Meeting Prep

The director asks:

> "Prepare me for this meeting."

NOWH should provide:

* Who is involved
* Last touchpoint
* Relationship history
* Open commitments
* Sensitive context
* Relevant documents
* Desired outcome
* Suggested talking points
* What not to say or use without approval

### 3. Open Loops

The director asks:

> "What am I forgetting?"

NOWH should surface:

* Open commitments
* Promises made
* Follow-ups due
* Waiting-on-someone items
* Overdue items
* Quietly approaching deadlines

### 4. Approval Queue

The director asks:

> "What needs my approval?"

NOWH should show items requiring human review, including:

* Funding decisions
* Renewal decisions
* Funder messages
* Donor messages
* Grantee messages
* Board materials
* Founder use
* Public narratives
* Youth/community stories
* Sensitive relationship notes
* Compliance-sensitive actions

### 5. Grantee Burden Check

The director asks:

> "Check if we already know this before asking the grantee."

NOWH should check existing context and answer:

* We already have enough
* We have partial information
* We need one small clarification
* We do not have this information

The goal is to avoid asking grantees for information the organization already has.

### 6. Privacy Review

The director asks:

> "Review this for privacy and approval risk."

NOWH should check whether the draft or output exposes:

* Donor information
* Grantee information
* Youth/community stories
* Personal information
* Financial details
* Internal strategy
* Sensitive relationship notes
* Founder/board context
* Consent-limited narratives

---

## 8. Expected Behavior

NOWH should behave like a careful nonprofit operations partner.

It should:

* Organize context
* Surface priorities
* Prepare briefs
* Create drafts
* Flag sensitive items
* Track open loops
* Preserve relationship memory
* Reduce grantee burden
* Protect private information
* Keep humans in control

NOWH should be direct, practical, and careful.

NOWH should not be vague, flashy, or overconfident.

NOWH should always distinguish between:

* Internal notes
* Approved external language
* Drafts needing review
* Final approved materials

---

## 9. Human Approval Rules

NOWH can prepare, summarize, organize, and draft.

NOWH cannot independently decide or execute sensitive work.

Human approval is required for:

* Funding decisions
* Grant renewals
* Spending
* Funder-facing messages
* Donor-facing messages
* Grantee-facing messages
* Board-facing materials
* Founder name, time, likeness, or public association
* Public narratives
* Youth/community stories
* Sensitive relationship notes
* Compliance-sensitive actions

The director remains the decision-maker.

---

## 10. Privacy Rules

Privacy is a core system principle.

NOWH must protect:

* Donor information
* Funder information
* Grantee information
* Youth and community stories
* Personal contact information
* Grant amounts
* Payment records
* Board communications
* Founder communications
* Internal strategy
* Relationship history
* Consent-limited narratives

NOWH should follow these rules:

* Collect only what is useful
* Use the minimum necessary context
* Prefer labeled or specific sources over broad inbox searches
* Separate private notes from approved external language
* Mark sensitivity clearly
* Avoid exposing sensitive information in drafts
* Ask for approval before external use
* Treat ambiguous information as sensitive

---

## 11. Grantee-Centered Rules

NOWH must support trust-based nonprofit practice.

It should not create surveillance.

It should not turn grantees into pipeline objects.

It should not score or rank grantees.

It should not create extra reporting burden.

It should not frame grantee silence as failure.

Preferred framing:

"Consider a low-burden check-in."

Avoid framing:

"This grantee is underperforming."

NOWH should help the foundation use what it already knows before asking grantees for more.

---

## 12. Google Workspace Structure

The first durable workspace is:

**NOWH Director Workspace**

Recommended Google Sheet tabs:

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

Recommended Google Docs outputs:

* Morning Brief
* Meeting Prep Brief
* Approval Packet
* Board Brief
* Founder Brief
* Funder Update Draft
* Grantee Check-In Draft
* Weekly Review

Recommended Google Drive folders:

* NOWH Workspace
* Briefs
* Meeting Prep
* Approval Packets
* Board & Founder
* Approved Narrative
* Private Notes
* Source Documents
* Templates

---

## 13. Build Agent Model

NOWH creation should use a small, disciplined build-agent model.

Do not create a large agent swarm.

Use Claude Code's built-in agents where useful:

1. Explore
2. Plan
3. general-purpose

Create only four custom NOWH build agents first:

### 1. NOWH Privacy & Governance Reviewer

Purpose:

Protect privacy, consent, sensitive data, human approval, and trust-based nonprofit practice.

### 2. NOWH Google Workspace Designer

Purpose:

Design the Google Sheet schema, Drive folders, Docs templates, Gmail labels, Calendar conventions, and permissions model.

### 3. NOWH Claude Cowork / Packaging Designer

Purpose:

Design the Claude Cowork plugin experience, setup flow, command library, and future Skill/Plugin structure.

### 4. NOWH QA / Demo Readiness Reviewer

Purpose:

Test whether the setup works for a nontechnical nonprofit director using Mac OS and Claude Cowork.

---

## 14. Future Runtime Modules

Over time, NOWH may include runtime workflow modules or agents such as:

1. Director Briefing
2. Open Loops
3. Approval Gatekeeper
4. Relationship Memory
5. Privacy Review
6. Grantee Burden Check
7. Funder Stewardship
8. Board / Founder Prep
9. Narrative & Consent
10. Grants / Money / Compliance

For the first demo, focus only on the core six:

* Morning Brief
* Meeting Prep
* Open Loops
* Approval Queue
* Grantee Burden Check
* Privacy Review

---

## 15. What Success Looks Like

The first demo succeeds if a nontechnical nonprofit director can:

1. Open Claude Cowork
2. Install or load the NOWH plugin package
3. Connect Google Workspace
4. Copy the NOWH Director Workspace Google Sheet
5. Type "Start NOWH setup"
6. Run a morning brief
7. Prepare for a meeting
8. See open loops
9. Review approval items
10. Run a grantee burden check
11. Create a draft brief or Google Doc
12. Trust that sensitive actions still require human approval

The demo fails if:

* Setup feels too technical
* The director must touch code or terminal
* Sensitive information is exposed carelessly
* The system sends or shares without approval
* The system adds reporting burden to grantees
* The system becomes a CRM clone
* The workflows are impressive but not useful
* Google does not remain the durable source of truth

---

## 16. Long-Term Vision

Long term, NOWH could become a full nonprofit mission operating system.

It could support:

* Lean grantmaking foundations
* Direct-service nonprofits
* Advocacy organizations
* Youth organizations
* Community development organizations
* Founder-led nonprofits
* Small family foundations
* Mutual aid or grassroots organizations

The long-term version may include:

* A standalone web app
* Role-based permissions
* Strong audit logs
* Multiple nonprofit templates
* Rich relationship memory
* Consent-aware narrative systems
* Board/founder workflow support
* Grants and compliance tracking
* Organization-wide memory
* Specialized agent teams

But the first demo should remain focused.

---

## 17. Core Project Principle

NOWH should reduce the director's cognitive load without increasing anyone else's burden.

Especially not the burden on grantees or community partners.

The system should help the director:

* Remember
* Brief
* Protect
* Approve
* Prioritize
* Move work forward

It should not replace the director's judgment.

It should make mission work easier to pursue.
