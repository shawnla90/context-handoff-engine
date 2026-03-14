# Team Setup

Teams are for when multiple Claude Code agents need to coordinate on the same feature - different agents with different roles working in parallel with shared rules.

## Prerequisites

- Handoffs + memory working
- Understanding of the routing framework (see [routing/](../routing/))
- 15 minutes

## When to Use Teams vs. Subagents

Run the [routing decision framework](../routing/README.md) first. Teams are the right choice when:
- 5+ files across different concerns
- Different agents need different expertise (content vs. code vs. config)
- Cross-review is needed (one agent checks another's work)
- Fan-out handoffs (A completes, then B/C/D run in parallel)

If the task scores as Pattern A (solo) or Pattern B (subagents), don't set up a team.

## Step 1: Create the constraints file

```bash
mkdir -p .claude/teams
curl -o .claude/teams/TEAM-CONSTRAINTS.md \
  https://raw.githubusercontent.com/shawnla90/context-handoff-engine/main/teams/team-constraints-template.md
```

## Step 2: Customize for your project

Edit `.claude/teams/TEAM-CONSTRAINTS.md` to add:
- Your project's build command (Rule 5)
- Your project's file structure (helps agents understand ownership)
- Any project-specific anti-patterns

## Step 3: Create a decisions document (optional but recommended)

For complex features, create a `CONTEXT.md` in the feature's working directory:

```markdown
# Feature: [Name]

## Decisions (locked)
- Using cursor-based pagination (not offset)
- Cache TTL: 5 minutes
- Auth: JWT with 1-hour expiry

## Open Questions
- Should we support v1 and v2 simultaneously?
```

This prevents agents from re-litigating choices.

## Step 4: Assign tasks with the standard format

Each agent needs:
1. The constraints file
2. The decisions document (if it exists)
3. 3-5 specific files to read
4. A task description with acceptance criteria

Use this format:

```markdown
## Task: Build metrics API endpoint

**files_to_read:**
- src/app/api/users/route.ts - pattern to follow
- prisma/schema.prisma - data model
- .claude/teams/TEAM-CONSTRAINTS.md - rules

**files_to_write:**
- src/app/api/metrics/route.ts
- tests/api/metrics.test.ts

**Acceptance criteria:**
- [ ] GET /api/metrics returns paginated results
- [ ] Cursor-based pagination (not offset)
- [ ] Tests pass
- [ ] Build passes clean
```

## Step 5: Run in waves

```
Wave 1: Data layer + shared types
  ↓ verify: types compile, schemas valid
Wave 2: API routes + components
  ↓ verify: endpoints return expected data
Wave 3: Integration (navigation, exports)
  ↓ verify: full user flow works
Wave 4: Build + deploy
```

Each agent in a wave runs in parallel. Between waves, the team lead (you or the main session) verifies output.

## Verification

After a team run:
- Check that no files were written by multiple agents (look at git blame)
- Check that all decisions were logged
- Run the full build
- Review the handoffs from each agent
