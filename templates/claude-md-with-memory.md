# Claude Code Instructions

## Session Startup

1. Read all unconsumed handoffs: `ls ~/.claude/handoffs/*.md 2>/dev/null | grep -v '_done.md$'`
2. Read `tasks/lessons.md` for self-improvement rules - follow every one.
3. Run `git status` and `git log -1 --oneline` to know where you are.
4. After reading handoffs, mark each consumed: rename `file.md` to `file_done.md`
5. Clean up old consumed handoffs: `find ~/.claude/handoffs -name '*_done.md' -mtime +7 -delete 2>/dev/null`

## Session End

Write handoff to `~/.claude/handoffs/YYYY-MM-DD_HHMMSS_<slug>.md` using the 5-section template:

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
[Ordered list for the next session.]

## Key Decisions Made
[Decisions the next agent needs to know.]

## Files to Read First
[Top 3-5 files for orientation.]
```

Never overwrite another session's handoff.

## Self-Improvement Loop

- After ANY correction from the user: update `tasks/lessons.md` with date, context, and lesson.
- Write rules that prevent the same mistake from recurring.
- Ruthlessly iterate on lessons until mistake rate drops.
- Review `tasks/lessons.md` at session start (Step 2 above).

## Task Management

1. Write plan to `tasks/todo.md` with checkable items before starting.
2. Mark items complete as you go.
3. Add a review section when done.

## Auto Memory

You have a persistent auto memory directory. Its contents persist across conversations.

### How to save memories:
- MEMORY.md is always loaded (first 200 lines). Keep it concise.
- Create topic files for details and link to them from MEMORY.md.
- Update or remove memories that turn out to be wrong.
- Check for existing memories before writing new ones.

### What to save:
- Stable patterns confirmed across multiple interactions
- Key architectural decisions, important file paths, project structure
- User preferences for workflow, tools, and communication style
- Solutions to recurring problems

### What NOT to save:
- Session-specific context (use handoffs)
- Unverified or speculative information
- Anything that duplicates CLAUDE.md instructions

## Core Principles

- Simplicity first. Make every change as simple as possible.
- No laziness. Find root causes. No temporary fixes.
- Minimal impact. Only touch what's necessary.
