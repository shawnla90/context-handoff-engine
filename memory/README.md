# Layer 2: Structured Memory Persistence

Memory files give Claude Code persistent knowledge across sessions - things that don't change between sessions but are critical context.

## The Problem

Claude Code's auto-memory directory (`~/.claude/projects/<project>/memory/MEMORY.md`) is powerful but has a hard limit: **only the first 200 lines are loaded into context**. Write 400 lines and half your knowledge silently disappears.

## The Architecture: Index + Topic Files

```
~/.claude/projects/<project>/memory/
├── MEMORY.md              # Index file (always loaded, keep under 200 lines)
├── identity.md            # Project identity, stakeholders, positioning
├── infrastructure.md      # Models, paths, services, API keys
├── completed-work.md      # Archive of finished initiatives
├── patterns.md            # Recurring solutions and conventions
└── debugging.md           # Solutions to problems you've hit before
```

### MEMORY.md (The Index)

This file is always loaded. It contains:
- Quick-reference facts (names, preferences, key paths)
- Links to topic files with brief descriptions
- Active priorities or pillars

It does NOT contain:
- Detailed notes (those go in topic files)
- Session-specific context (that goes in handoffs)
- Speculative conclusions from reading a single file

### Topic Files (On-Demand Detail)

Topic files hold the details. Claude reads them when relevant to the current task. They are NOT loaded automatically - the agent decides when to read them based on the index.

## What to Save

- **Stable patterns** confirmed across multiple interactions
- **Architectural decisions** and why they were made
- **User preferences** for workflow, tools, communication style
- **Solutions to recurring problems** (the fix, not the investigation)
- **Explicit user requests** ("always use bun", "never auto-commit")

## What NOT to Save

- Session-specific context (use handoffs for that)
- Information that might be incomplete
- Anything that duplicates existing CLAUDE.md instructions
- Speculative conclusions from reading a single file

## CLAUDE.md Integration

Add this to your `CLAUDE.md`:

```markdown
## Auto Memory

You have a persistent auto memory directory at `~/.claude/projects/<project>/memory/`.

### How to save memories:
- MEMORY.md is always loaded into context (first 200 lines only). Keep it concise.
- Create topic files for detailed notes and link to them from MEMORY.md.
- Update or remove memories that turn out to be wrong or outdated.
- Check for existing memories before writing new ones to avoid duplicates.

### What to save:
- Stable patterns confirmed across multiple interactions
- Key architectural decisions, important file paths, project structure
- User preferences for workflow, tools, and communication style
- Solutions to recurring problems

### What NOT to save:
- Session-specific context (use handoffs)
- Unverified or speculative information
- Anything that duplicates CLAUDE.md instructions
```

See [memory-index-template.md](memory-index-template.md) for a starting template.
