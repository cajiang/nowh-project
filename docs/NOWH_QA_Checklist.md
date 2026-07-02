# NOWH QA Checklist

Test every workflow and the packaged demo against:

- [ ] Nontechnical Mac user setup — no code/terminal/MCP knowledge required
- [ ] Google connector access — connects cleanly via Claude's native
      Google Workspace integration
- [ ] Morning Brief quality
- [ ] Meeting Prep quality
- [ ] Open Loops quality
- [ ] Approval gate behavior — sensitive actions correctly held for human
      approval, nothing sent/executed autonomously
- [ ] Privacy behavior — sensitivity levels respected, no restricted data
      leaking into external-facing drafts
- [ ] Grantee burden behavior — no redundant asks, care-based framing used
- [ ] No unauthorized sending or external sharing

## Closure Standard (apply to every task, not just final QA)

A task is complete only when:

- Scope was followed
- Output is reviewable
- Workflow is understandable
- Data structure is documented
- Privacy risks are identified and controlled
- Approval gates are present where required
- No unnecessary grantee burden was added
- No paid tools were introduced without approval
- Test cases or manual validation steps pass
- Unexpected files, tabs, fields, automations, or permissions are explained
- Next boundary is clear: stop, revise, test, package, document, or build
  next phase

If code, plugin files, or package files are involved, also require: clear
file summary, explanation of what changed, how to install it, how to test
it, known limitations, rollback/removal instructions where appropriate.

## Status

No workflows built yet — nothing to QA. This checklist applies once the
first workflow (likely Morning Brief) is built.
