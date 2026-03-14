# Layer 1: Parallel-Safe Session Handoffs

Session handoffs solve the most basic problem: Claude Code starts every session with zero memory of what happened before.

## The 4 Operations

### 1. Write

At the end of a session, write a handoff to a timestamped file:

```bash
mkdir -p ~/.claude/handoffs
```

File: `~/.claude/handoffs/YYYY-MM-DD_HHMMSS_<slug>.md`

The slug is a 2-4 word kebab-case label for what the session did. Examples:
- `2026-03-14_093022_auth-flow-refactor.md`
- `2026-03-14_141500_api-pagination-fix.md`

Each session writes its own file. No overwrites. No conflicts.

### 2. Read

On session start, find all unconsumed handoffs:

```bash
ls ~/.claude/handoffs/*.md 2>/dev/null | grep -v '_done.md$'
```

Read each one. Print a brief summary:

```
## Active Handoffs
- [2026-03-14 09:30] auth-flow-refactor - JWT middleware added, tests passing
- [2026-03-14 14:15] api-pagination-fix - Cursor-based pagination on /users endpoint
```

### 3. Consume

After reading a handoff, mark it done by renaming:

```bash
mv ~/.claude/handoffs/2026-03-14_093022_auth-flow-refactor.md \
   ~/.claude/handoffs/2026-03-14_093022_auth-flow-refactor_done.md
```

Only mark as done AFTER reading and confirming context is loaded.

### 4. Clean

Periodically remove old consumed handoffs (older than 7 days):

```bash
find ~/.claude/handoffs -name '*_done.md' -mtime +7 -delete 2>/dev/null
```

## Why Timestamps Instead of a Single File

With a single file (`~/.claude/context-handoff.md`), two terminals finishing at the same time will overwrite each other. The last write wins. Context from one session is silently lost.

With timestamped files, every session's context survives. The next session reads all of them.

## CLAUDE.md Integration

Add this to your `CLAUDE.md` to enable handoffs:

```markdown
## Session Workflow

### On Session Start
1. Read all unconsumed handoffs: `ls ~/.claude/handoffs/*.md 2>/dev/null | grep -v '_done.md$'`
2. After reading, mark each consumed: rename `file.md` to `file_done.md`
3. Clean up old consumed handoffs: `find ~/.claude/handoffs -name '*_done.md' -mtime +7 -delete 2>/dev/null`

### On Session End
Write handoff to `~/.claude/handoffs/YYYY-MM-DD_HHMMSS_<slug>.md` with the 5-section template.
Never overwrite another session's handoff.
```

See [handoff-template.md](handoff-template.md) for the 5-section document format.
