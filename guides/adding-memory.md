# Adding Memory

Memory gives Claude persistent knowledge that doesn't change between sessions - project conventions, architectural decisions, user preferences, and recurring solutions.

## Prerequisites

- Handoffs already working (see [getting-started.md](getting-started.md))
- 10 minutes

## Step 1: Upgrade your CLAUDE.md

Replace your minimal CLAUDE.md with the memory-enabled version:

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/shawnla90/context-handoff-engine/main/templates/claude-md-with-memory.md
```

## Step 2: Set up the self-improvement files

```bash
mkdir -p tasks
touch tasks/lessons.md tasks/todo.md
```

Add headers to each file:

**tasks/lessons.md:**
```markdown
# Lessons Learned
> After every correction, add a lesson with date, context, and the rule.
> Review at session start. Every lesson is a rule.
---
```

**tasks/todo.md:**
```markdown
# Task Tracker
> Write plans with checkable items before starting work.
---
```

## Step 3: Set up the memory index

Claude Code auto-creates the memory directory at `~/.claude/projects/<project-hash>/memory/`. You can bootstrap it:

```bash
# Find your project's memory directory
MEMORY_DIR=$(find ~/.claude/projects -name "memory" -type d 2>/dev/null | head -1)

# If it doesn't exist yet, Claude will create it on first use
# You can also create the MEMORY.md template manually:
curl -o "$MEMORY_DIR/MEMORY.md" \
  https://raw.githubusercontent.com/shawnla90/context-handoff-engine/main/memory/memory-index-template.md
```

## Step 4: Seed your memory

Open a Claude Code session and tell it key facts about your project:

- "Remember that we always use bun instead of npm"
- "Remember that the main database is PostgreSQL on Supabase"
- "Remember that API routes use kebab-case"

Claude will write these to MEMORY.md and they'll persist across every future session.

## How Memory Differs from Handoffs

| | Handoffs | Memory |
|---|---|---|
| **What** | Session state (what changed, what's next) | Stable knowledge (conventions, decisions, preferences) |
| **Lifecycle** | Created at session end, consumed at next session start | Persistent until manually updated or removed |
| **Size** | Under 100 lines per handoff | MEMORY.md under 200 lines, topic files unlimited |
| **Loaded** | Read once at session start | MEMORY.md always loaded, topic files on-demand |

## The 200-Line Rule

Claude loads the first 200 lines of MEMORY.md into every session. Beyond that, content is silently truncated. Keep the index file lean and link to topic files for details.

## Next Steps

- [Scaling to parallel terminals](scaling-to-parallel.md)
- [Team setup](team-setup.md)
