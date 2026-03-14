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

## Workflow Orchestration

### Plan Mode Default
- Enter plan mode for ANY non-trivial task (3+ steps or architectural decisions).
- If something goes sideways mid-execution, STOP and re-plan immediately.
- Plans should be fast and actionable - plan to build, not plan instead of building.

### Subagent Strategy
- Use subagents to keep main context window clean.
- Offload research, exploration, and parallel analysis to subagents.
- One task per subagent for focused execution.
- Before spawning 2+ subagents, consult the routing framework.

### Self-Improvement Loop
- After ANY correction from the user: update `tasks/lessons.md` with date, context, and lesson.
- Write rules that prevent the same mistake from recurring.
- Review `tasks/lessons.md` at session start.

### Verification Before Done
- Never mark a task complete without proving it works.
- Run tests, check logs, demonstrate correctness.
- For deploys: clean build + safety scan + sign-off.

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
- Key architectural decisions, file paths, project structure
- User preferences for workflow, tools, communication style
- Solutions to recurring problems

### What NOT to save:
- Session-specific context (use handoffs)
- Unverified or speculative information
- Anything that duplicates CLAUDE.md instructions

## Agent-to-Agent Handoffs

When handing off to a new agent (not a new session), produce a standalone context document:

| Section | Contents |
|---------|----------|
| Context | Who, what project, what we're building |
| What we accomplished | Completed tasks, deliverables, decisions |
| Key files | Absolute paths to outputs, inputs, references |
| Open questions / blocked | Anything unresolved or pending |
| Next steps | Specific actions for the receiving agent |
| Workflow hooks (optional) | Related commands, scripts to invoke |

Keep under 200 lines, token-efficient, file-anchored. No preamble.

## Agent Teams

When using multiple agents in parallel, load the team constraints file first. Key rules:
1. One writer per file per wave
2. Log all architectural decisions
3. Read before write - existing code is the style guide
4. Wave discipline - verify between phases
5. Build gate - no deploy without verification
6. Fresh context per executor

## Agent Routing

Before spinning up agents, evaluate the task across 5 dimensions:
- File count (1-2: solo, 3-4: subagents, 5+: team)
- Concern separation (same pattern: subagents, different expertise: team)
- Handoff requirement (none: subagents, linear: solo, fan-out: team)
- Review requirement (none: subagents, cross-review: team)
- Quality gate (style enforcement by another agent: team)

Route to the pattern with 3+ matches.

## Core Principles

- Simplicity first. Make every change as simple as possible.
- No laziness. Find root causes. No temporary fixes.
- Minimal impact. Only touch what's necessary.
- Plan fast, build fast, ship working code every session.
