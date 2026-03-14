# Agent Team Constraints

> Every teammate MUST read this file before making any changes.
> This is the management layer. Without it, parallel agents create chaos.

---

## Rule 1: File Ownership - One Writer Per File

No two agents may write to the same file in the same wave.

Before editing ANY file, the agent MUST:
1. Check the task list for who owns what
2. If another agent's task description mentions that file, DO NOT TOUCH IT
3. If unclear, message the team lead and wait for assignment

**Violations of this rule cause silent data loss.** The last write wins. There is no merge.

---

## Rule 2: Shared Decisions Log

When an agent makes an architectural decision (pattern choice, naming convention, data structure, import path), it MUST:

1. Record it in a message to the team lead with prefix `[DECISION]`
2. The decision is then available to all other agents via team communication

Examples of decisions that MUST be logged:
- "Using cursor-based pagination, not offset-based"
- "Import from `@app/shared/` not relative paths"
- "Using date format `2026-03-14` not `Mar 14, 2026`"

**If you make a decision and don't log it, another agent WILL make a conflicting one.**

---

## Rule 3: Read Before You Write

Every agent must read at least ONE existing example of whatever it's about to create or modify.

- Writing an API route? Read an existing route first.
- Adding a component? Read a sibling component first.
- Updating a config? Read the current config first.

The existing code IS the style guide.

---

## Rule 4: Wave Discipline

Work is organized in sequential waves. Each wave contains parallel tasks.

### Preferred: Dependency-Based Waves

1. List all tasks with their inputs (files read) and outputs (files written)
2. A task is BLOCKED if any input files are written by an incomplete task
3. Wave 1 = all tasks with zero blocked inputs
4. Wave 2 = tasks unblocked when Wave 1 completes
5. Continue until all tasks are assigned

### Fallback: Generic 4-Tier Structure

```
Wave 1: Foundation (data files, types, shared code)
  ↓ VERIFY before proceeding
Wave 2: Consumers (pages, components that import from Wave 1)
  ↓ VERIFY before proceeding
Wave 3: Integration (navigation, cross-links, exports)
  ↓ VERIFY before proceeding
Wave 4: Validation (build, test, deploy)
```

### Wave Rules
- Same wave agents CAN run in parallel (they touch different files)
- Different wave agents MUST run sequentially
- Between waves, verify output before launching the next wave

---

## Rule 5: Build Gate

No deploy agent may push until:
1. Build passes clean
2. Pre-push safety scan passes (if configured)
3. Team lead confirms all output is merged and verified

---

## Rule 6: Context Before Action

Every teammate receives:
1. This constraints file (non-negotiable rules)
2. A specific task description (what to do)
3. File references (what to read for patterns)
4. Clear ownership boundaries (what files this agent owns)

If any of these are missing, the agent MUST ask the team lead before proceeding.

---

## Rule 7: Scope Isolation

Agents fix ONLY issues caused by their own work in the current task.

- **Pre-existing bugs:** Log with `[PRE-EXISTING]` tag. Do NOT fix - it's out of scope.
- **3-attempt limit:** If a fix fails 3 times, document with `[STUCK]` tag and move on.
- **No scope creep:** "While I'm in here, I'll also fix..." is how agents spend 80% of time on 20% of value.

---

## Rule 8: Fresh Context Per Executor

Every executor starts with a clean context. No inherited assumptions.

### Load Order (mandatory, in this sequence):
1. This constraints file
2. Locked decisions document (if one exists)
3. 3-5 specific files listed in the task description
4. Task description

### Task Description Format

```
## Task: [Short title]

**files_to_read:**
- [path/to/file1] - [why: "pattern to follow"]
- [path/to/file2] - [why: "data structure to match"]

**files_to_write:**
- [path/to/output1]
- [path/to/output2]

**Acceptance criteria:**
- [ ] [Specific, testable criterion]
- [ ] [Another criterion]
- [ ] Build passes clean
```

### Atomic Commits

Each executor commits only its own files with a semantic prefix:
- `feat:` - new functionality
- `fix:` - bug fix
- `refactor:` - restructure without behavior change
- `docs:` - documentation only
- `chore:` - tooling, config, maintenance

Do NOT stage files owned by other agents. Do NOT use `git add -A`.

---

## Anti-Patterns (What NOT To Do)

1. **Two agents editing the same data file** - even different entries
2. **Agent guessing import paths** - read existing files to see how imports work
3. **Skipping the build check** - "it's a small change" is how broken deploys happen
4. **Agent making decisions without logging them** - context drift starts here
5. **Deploying without team lead sign-off** - the deploy agent waits for explicit approval
