# Layer 4: Agent-to-Agent Context

When handing off between agents (not sessions), the receiving agent needs a standalone document it can understand without any prior conversation.

## When to Use

- Spawning a subagent that needs context from the parent session
- Handing work from one specialized agent to another
- Starting a new chat thread that continues work from a previous one
- Passing context to a different AI tool or model entirely

## The 6-Section Format

Every agent handoff document has these sections:

| Section | Contents |
|---------|----------|
| **Context** | Who, what project, what we're building |
| **What we accomplished** | Completed tasks, deliverables, decisions made |
| **Key files** | Absolute paths to outputs, inputs, references |
| **Open questions / blocked** | Anything unresolved or pending human input |
| **Next steps** | Specific actions for the receiving agent |
| **Workflow hooks** (optional) | Related commands, scripts, or skills to invoke |

## Principles

1. **Standalone** - the receiving agent understands immediately with no prior context
2. **Token-efficient** - every section earns its place. Keep under 200 lines
3. **Actionable** - ends with specific next steps so the agent knows what to do
4. **File-anchored** - includes paths so the agent can read files without guessing

## How It Differs from Session Handoffs

Session handoffs (Layer 1) are for the same agent in a new session. They assume familiarity with the project.

Agent handoffs (Layer 4) are for a different agent that may know nothing about the project. They include more context and are designed to be pasted into a fresh context window.

See [agent-handoff-template.md](agent-handoff-template.md) for the full template.
