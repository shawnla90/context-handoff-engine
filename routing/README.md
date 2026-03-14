# Layer 6: Routing Decision Framework

Before spinning up agents or teams, run this decision framework. The goal is to match the execution pattern to the task, not default to the most complex option.

## The 5 Dimensions

### 1. File Count
How many files will be WRITTEN or EDITED (not read)?

- **1-2 files**: Single session
- **3-4 files**: Subagents
- **5+ files across different concerns**: Team

### 2. Concern Separation
Are the outputs independent or do they need coordination?

- **All outputs in closely related files**: Single session
- **Different files but same pattern**: Subagents (each mirrors pattern independently)
- **Different files AND different expertise needed**: Team

### 3. Handoff Requirement
Does any task need the OUTPUT of another task before it can start?

- **No handoffs needed**: Subagents
- **Linear handoff (A → B → C)**: Single session (sequential in one context is faster)
- **Fan-out handoff (A → B, C, D simultaneously)**: Team

### 4. Review Requirement
Does output need human-style judgment before the next step?

- **No review needed (mechanical output)**: Subagents
- **Self-review sufficient**: Single session
- **Cross-review needed**: Team

### 5. Quality Gate
Does the output require style compliance, brand checking, or quality enforcement?

- **No style requirements (code, config)**: Any pattern works
- **Style required, main session is writing**: Single session
- **Style required, another agent is writing**: Team with specialized roles

## Scoring

Count how many dimensions point to each pattern. Route to the one with 3+ matches.

### Pattern A: Single Focused Session
**When:** 3+ dimensions say "single session"

The main session handles everything. Use lightweight subagents for initial research, but all writing stays in the main session.

### Pattern B: Parallel Subagents
**When:** 3+ dimensions say "subagents"

Launch independent agents. Each gets a specific prompt, works in isolation, reports back. No inter-agent communication. Parent orchestrates.

### Pattern C: Agent Teams
**When:** 3+ dimensions say "team" AND at least one of:
- Multiple agents need to communicate with each other
- A shared task list with dependencies is needed
- Different agents need different roles with different context
- The task takes 15+ minutes with self-coordination needed

## Anti-Patterns

1. **Team for a 1-2 file edit.** Coordination overhead costs more than the work.
2. **Redundant research agents.** Don't spawn a team researcher AND a subagent researcher for the same topic.
3. **Team where you're the only writer.** Teammates become expensive advisors. Use subagents for research, write the code yourself.
4. **Subagents for sequential tasks.** If B needs A's output, running them as subagents adds overhead. Keep sequential work in one context.
5. **Defaulting to teams because the user said "agents."** Often they mean "parallelize where it makes sense," not "create a full team."

See [quick-reference.md](quick-reference.md) for a signal-to-route lookup table.
