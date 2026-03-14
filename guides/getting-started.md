# Getting Started

## Prerequisites

- Claude Code installed and working
- A git repository for your project
- 5 minutes

## Step 1: Create the handoffs directory

```bash
mkdir -p ~/.claude/handoffs
```

## Step 2: Add handoff instructions to your CLAUDE.md

If you don't have a `CLAUDE.md` in your project root, create one:

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/shawnla90/context-handoff-engine/main/templates/claude-md-minimal.md
```

If you already have a `CLAUDE.md`, add the session workflow section from [claude-md-minimal.md](../templates/claude-md-minimal.md).

## Step 3: Test it

1. Open a Claude Code session
2. Do some work
3. Type `/handoff` or ask Claude to "create a handoff"
4. Check that a file was created: `ls ~/.claude/handoffs/`
5. Open a new session - Claude should read the handoff automatically

## What Happens

- **Session end:** Claude writes a 5-section markdown file to `~/.claude/handoffs/` with a timestamp and slug
- **Session start:** Claude reads all `*.md` files in `~/.claude/handoffs/` that don't end in `_done.md`, prints a summary, then renames them with `_done` suffix
- **Cleanup:** Files ending in `_done.md` older than 7 days are deleted

## Next Steps

- [Adding memory](adding-memory.md) - persistent knowledge across sessions
- [Scaling to parallel terminals](scaling-to-parallel.md) - what changes when you run 4+ terminals
- [Team setup](team-setup.md) - multi-agent coordination
