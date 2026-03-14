# Claude Code Instructions

## Session Workflow

### On Session Start
1. Read all unconsumed handoffs: `ls ~/.claude/handoffs/*.md 2>/dev/null | grep -v '_done.md$'`
2. After reading, mark each consumed: rename `file.md` to `file_done.md`
3. Clean up old consumed handoffs: `find ~/.claude/handoffs -name '*_done.md' -mtime +7 -delete 2>/dev/null`

### On Session End
Write handoff to `~/.claude/handoffs/YYYY-MM-DD_HHMMSS_<slug>.md`.
Never overwrite another session's handoff.
