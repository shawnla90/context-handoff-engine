---
name: context-handoff
description: Generate a context handoff document at the end of a session so the next session can pick up where this one left off. Auto-invoked when ending significant work, or manually via /handoff.
---

# Context Handoff

Generate a structured handoff document that preserves session context for the next agent. Supports parallel sessions - multiple handoffs coexist without overwriting each other.

## When to Invoke

- End of any session that made meaningful changes
- When context window is getting large and a fresh session is needed
- User says `/handoff`, "wrap up", "hand off", or "create a handoff"

## Output Location

Write to: `~/.claude/handoffs/<timestamp>_<slug>.md`

- **Format:** `YYYY-MM-DD_HHMMSS_<slug>.md`
- **Slug:** 2-4 word kebab-case label for the session's work
- **Example:** `~/.claude/handoffs/2026-03-14_143022_auth-refactor.md`

Create the directory if it doesn't exist:
```bash
mkdir -p ~/.claude/handoffs
```

**IMPORTANT:** Never overwrite or delete other handoff files. Each session writes its own file.

## Handoff Template

```markdown
# Context Handoff
> Generated: YYYY-MM-DD HH:MM | Session: [brief label]

## What Was Done This Session
[Bullet list of completed work with file paths.]

## Current State
- **Git**: [branch, clean/dirty, last commit hash + message]
- **Uncommitted changes**: [list or "none"]
- **Blocked on**: [anything unfinished and why]

## Next Steps
[Ordered list for the next session. Include file paths and specific instructions.]

## Key Decisions Made
[Architectural or approach decisions the next agent needs to know about.]

## Files to Read First
[Top 3-5 files the next session should read to get oriented, in priority order.]
```

## Rules

1. **Be specific** - file paths, commit hashes, line numbers. Vague handoffs are useless.
2. **Don't summarize the whole repo** - only what changed THIS session.
3. **Include blockers** - if something failed or was skipped, say why.
4. **Keep it under 100 lines** - this is a handoff, not a novel.
5. **Always run `git status` and `git log -1 --oneline`** before writing.
6. **Never overwrite another session's handoff.**
7. **Consume on read.** Mark handoffs `_done` after reading.
8. **Auto-clean.** Delete `_done` handoffs older than 7 days.
