# Scaling to Parallel Terminals

Running 2-6 Claude Code sessions simultaneously unlocks significant productivity. Here's what changes and what to watch for.

## Prerequisites

- Handoffs + memory working (see [getting-started.md](getting-started.md) and [adding-memory.md](adding-memory.md))
- 5 minutes

## What Works Out of the Box

**Handoffs are already parallel-safe.** Each session writes a timestamped file. No overwrites. When you end 4 sessions, you get 4 separate handoff files. The next session reads all of them.

**Memory is shared and read-only (mostly).** All sessions read from the same MEMORY.md. Writes are rare and typically non-conflicting (different topic files).

## What to Watch For

### 1. Git Conflicts

Two terminals on the same branch editing different files is fine. Two terminals editing the same file will conflict on commit.

**Solutions:**
- Assign different files to different terminals
- Use feature branches per terminal
- Commit frequently from each terminal

### 2. Lessons File Contention

If two sessions write to `tasks/lessons.md` simultaneously, one write may be lost.

**Mitigation:** Lessons are typically written after corrections, which happen in interactive sessions. It's rare for two sessions to receive corrections at the exact same time. If it happens, the lost lesson can be re-added.

### 3. Build Conflicts

Two terminals running builds simultaneously can conflict on build output directories.

**Solution:** Stagger builds. Or assign one terminal as the "build and deploy" terminal.

## Recommended Parallel Patterns

### The Research + Build Pattern
- Terminal 1: Research and exploration (read-only, subagents)
- Terminal 2: Active development (writing code)

### The Feature Split Pattern
- Terminal 1: Backend API work
- Terminal 2: Frontend UI work
- Terminal 3: Tests and documentation

### The Sprint Pattern
- Terminal 1-3: Each working on an independent feature/file set
- Terminal 4: Integration, build, deploy (starts after 1-3 finish)

## Verifying Parallel Safety

After running parallel sessions, check:

```bash
# How many handoffs were created?
ls -la ~/.claude/handoffs/*.md 2>/dev/null | grep -v '_done.md$' | wc -l

# Any handoffs from the same second? (would indicate a collision risk)
ls ~/.claude/handoffs/*.md 2>/dev/null | grep -v '_done.md$' | sort
```

Each session should have its own file with a unique timestamp.

## Next Steps

- [Team setup](team-setup.md) - add coordination rules for agents working together
