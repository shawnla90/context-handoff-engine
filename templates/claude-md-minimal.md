# Claude Code Instructions

## Session Workflow

### On Session Start
1. Read all unconsumed handoffs: `ls ~/.claude/handoffs/*.md 2>/dev/null | grep -v '_done.md$'`
2. After reading, mark each consumed: rename `file.md` to `file_done.md`
3. Clean up old consumed handoffs: `find ~/.claude/handoffs -name '*_done.md' -mtime +7 -delete 2>/dev/null`

### On Session End
Write handoff to `~/.claude/handoffs/YYYY-MM-DD_HHMMSS_<slug>.md` using the 5-section template:

```markdown
# Context Handoff
> Generated: YYYY-MM-DD HH:MM | Session: [brief label]

## What Was Done This Session
[Bullet list of completed work - files created, edited, fixed. Be specific with paths.]

## Current State
- **Git**: [branch, clean/dirty, last commit hash + message]
- **Uncommitted changes**: [list or "none"]
- **Blocked on**: [anything that couldn't be completed and why]

## Next Steps
[Ordered list of what the next session should pick up.]

## Key Decisions Made
[Architectural or approach decisions the next agent needs to know.]

## Files to Read First
[Top 3-5 files the next session should read to get oriented.]
```

Never overwrite another session's handoff. Each session gets its own file.
