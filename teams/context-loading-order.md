# Context Loading Order for Team Executors

Every agent that runs as part of a team starts with a clean context window. No inherited assumptions from other agents, no conversation history, no implicit knowledge.

This is by design. Fresh context prevents contamination between agents and ensures each one operates from the same verified source of truth.

## Mandatory Load Order

```
1. Team Constraints     → Rules that every agent follows
2. Locked Decisions     → Choices already made (don't re-litigate)
3. Reference Files      → 3-5 files listed in the task description
4. Task Description     → What to do, what to write, acceptance criteria
```

### 1. Team Constraints

The constraint file (this repo's `team-constraints-template.md` or your customized version). Every agent reads this first. Non-negotiable.

### 2. Locked Decisions

If a decisions document exists for this feature (e.g., `CONTEXT.md` or `DECISIONS.md` in the working directory), read it. These are choices already made by the team lead or a prior discussion phase. Do not re-ask or override them.

If no decisions document exists and the task is complex (3+ files across 2+ concerns), the team lead should create one before assigning work.

### 3. Reference Files

The task description lists 3-5 specific files to read. These provide:
- **Patterns to follow** - existing code that shows how things are done
- **Data structures to match** - schemas, types, interfaces the output must conform to
- **Constraints to respect** - configuration files, environment requirements

### 4. Task Description

The actual work assignment. Uses the format from Rule 8 of the constraints:
- `files_to_read` with reasons
- `files_to_write` (ownership)
- `Acceptance criteria` (how to know you're done)

## Why This Order Matters

- Constraints first: prevents the agent from doing anything the team rules prohibit
- Decisions second: prevents re-litigating choices, even if the agent "thinks" there's a better way
- Reference files third: shows the agent what good output looks like
- Task last: now the agent knows the rules, the decisions, and the patterns. It can execute with confidence.

Skipping or reordering steps leads to agents that produce technically correct output that conflicts with team conventions.
