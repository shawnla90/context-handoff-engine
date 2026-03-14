# Agent Context Handoff Template

Copy this template when producing a standalone context document for a new agent.

---

```markdown
# Agent Context Handoff

## Context
- **Project:** [name and brief description]
- **Stakeholder:** [who requested the work]
- **Current phase:** [what stage we're in]

## What We Accomplished
- [Completed task 1 - be specific with files and outcomes]
- [Completed task 2]
- [Key decision: chose X over Y because Z]

## Key Files
- `path/to/main-output.ts` - [what it is, why it matters]
- `path/to/config.json` - [current state, any changes made]
- `path/to/reference.md` - [context document, read before modifying]

## Open Questions / Blocked
- [Question 1: waiting for human input on X]
- [Blocker: API key for staging not yet provisioned]
- [Decision needed: should we support both v1 and v2 simultaneously?]

## Next Steps
1. [Specific action 1 - include file paths]
2. [Specific action 2 - include acceptance criteria]
3. [Specific action 3 - include any constraints]

## Workflow Hooks
- Run `npm test` after modifying any file in `src/auth/`
- Build check: `npm run build` must pass before any commit
- Related script: `scripts/seed-data.sh` populates test database
```

---

## Rules

- **No preamble.** Start with content, not "This document summarizes..."
- **Use absolute or repo-relative paths.** The receiving agent shouldn't have to guess.
- **Include decisions.** The biggest value of a handoff is preventing the next agent from re-litigating choices already made.
- **Keep under 200 lines.** If you need more, the scope is too large for a single handoff.
