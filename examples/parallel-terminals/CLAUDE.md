# Claude Code Instructions

## Session Startup

1. Read all unconsumed handoffs: `ls ~/.claude/handoffs/*.md 2>/dev/null | grep -v '_done.md$'`
2. Read `tasks/lessons.md` for self-improvement rules - follow every one.
3. Run `git status` and `git log -1 --oneline` to know where you are.
4. After reading handoffs, mark each consumed: rename `file.md` to `file_done.md`
5. Clean up old consumed handoffs: `find ~/.claude/handoffs -name '*_done.md' -mtime +7 -delete 2>/dev/null`

## Session End

Write handoff to `~/.claude/handoffs/YYYY-MM-DD_HHMMSS_<slug>.md`.
Never overwrite another session's handoff.

## Self-Improvement Loop

- After ANY correction: update `tasks/lessons.md` with date, context, and lesson.
- Review `tasks/lessons.md` at session start.

## Task Management

1. Write plan to `tasks/todo.md` with checkable items before starting.
2. Mark items complete as you go.

## Auto Memory

Persistent memory at `~/.claude/projects/<project>/memory/`.
- MEMORY.md is always loaded (first 200 lines). Keep it concise.
- Create topic files for details, link from MEMORY.md.
