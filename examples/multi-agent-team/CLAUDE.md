# Claude Code Instructions

## Session Startup

1. Read all unconsumed handoffs: `ls ~/.claude/handoffs/*.md 2>/dev/null | grep -v '_done.md$'`
2. Read `tasks/lessons.md` for self-improvement rules - follow every one.
3. Run `git status` and `git log -1 --oneline` to know where you are.
4. After reading handoffs, mark each consumed: rename `file.md` to `file_done.md`
5. Clean up old consumed handoffs: `find ~/.claude/handoffs -name '*_done.md' -mtime +7 -delete 2>/dev/null`

## Workflow Orchestration

### Plan Mode Default
- Enter plan mode for ANY non-trivial task (3+ steps or architectural decisions).
- If something goes sideways, STOP and re-plan.

### Subagent Strategy
- Use subagents to keep main context window clean.
- One task per subagent for focused execution.
- Before spawning 2+ subagents, consult the routing framework.

### Self-Improvement Loop
- After ANY correction: update `tasks/lessons.md` with date, context, and lesson.
- Review `tasks/lessons.md` at session start.

### Verification Before Done
- Never mark a task complete without proving it works.
- Run tests, check logs, demonstrate correctness.
- For deploys: clean build + safety scan.

## Agent Teams

When using multiple agents in parallel, follow `.claude/teams/TEAM-CONSTRAINTS.md`:
- One writer per file per wave
- Log all decisions
- Read before write
- Wave discipline with verification gates
- Build gate before deploy
- Fresh context per executor

## Agent Routing

Before spawning agents, evaluate the 5 dimensions:
- File count, concern separation, handoff requirement, review requirement, quality gate
- Route to the pattern with 3+ matches (solo / subagents / team)

## Session End

Write handoff to `~/.claude/handoffs/YYYY-MM-DD_HHMMSS_<slug>.md`.
Never overwrite another session's handoff.
