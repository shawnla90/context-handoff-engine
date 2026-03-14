# Layer 5: Multi-Agent Team Coordination

When multiple agents work on the same codebase simultaneously, you need explicit rules to prevent chaos. Without them, agents silently overwrite each other's files, make conflicting architectural decisions, and skip verification steps.

## Why Teams Need Extra Rules

Single sessions and subagents report to one parent. The parent coordinates. But when 3+ agents work in parallel with independent context windows, there is no implicit coordination. The last agent to write a file wins. Decisions made in one context window are invisible to others.

The constraint system makes the implicit explicit.

## The 9 Rules

### Rule 1: File Ownership - One Writer Per File

No two agents may write to the same file in the same wave. Before editing any file, check who owns it. If another agent's task mentions that file, don't touch it.

**Why:** The last write wins. There is no merge. Silent data loss.

### Rule 2: Shared Decisions Log

When an agent makes an architectural decision (naming convention, data structure, import path), it must log it so other agents can see it.

Examples of decisions that must be logged:
- "Using cursor-based pagination, not offset"
- "Import from `@app/shared/` not `../../../packages/`"
- "Date format: `2026-03-14` not `Mar 14, 2026`"

**Why:** If you make a decision and don't log it, another agent will make a conflicting one.

### Rule 3: Read Before Write

Every agent must read at least one existing example of whatever it's about to create or modify. The existing code is the style guide.

### Rule 4: Wave Discipline

Work is organized in sequential waves. Each wave contains parallel tasks.

```
Wave 1: Foundation (data files, types, shared code)
  ↓ VERIFY before proceeding
Wave 2: Consumers (pages, components that import from Wave 1)
  ↓ VERIFY before proceeding
Wave 3: Integration (navigation, cross-links, exports)
  ↓ VERIFY before proceeding
Wave 4: Validation (build, test, deploy)
```

Agents in the same wave can run in parallel (they touch different files). Agents in different waves run sequentially.

**Preferred:** When task dependencies are known, use dependency-based wave assignment instead - tasks run as early as their actual dependencies allow.

### Rule 5: Build Gate

No deploy until:
1. Build passes clean
2. All teammate output is verified
3. Team lead confirms

### Rule 6: Context Before Action

Every teammate receives: constraint rules, task description, file references, and ownership boundaries. If any are missing, ask before proceeding.

### Rule 7: Scope Isolation

Fix only issues caused by your own work. Pre-existing bugs get logged, not fixed. 3-attempt limit on any fix before escalating.

### Rule 8: Fresh Context Per Executor

Every executor starts with clean context. Load order:
1. Team constraints (these rules)
2. Locked decisions (if a decisions document exists)
3. Specific files listed in the task description
4. Task description

### Rule 9: Anti-Patterns

1. Two agents editing the same file - even different parts
2. Guessing import paths instead of reading existing files
3. Skipping the build check ("it's a small change")
4. Making decisions without logging them
5. Deploying without team lead sign-off

See [team-constraints-template.md](team-constraints-template.md) for a copy-paste ready version.
