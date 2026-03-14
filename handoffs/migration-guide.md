# Migration Guide: Single File to Parallel-Safe Handoffs

## The Old Way

You had a single file, probably one of:

- `~/.claude/context-handoff.md`
- `.claude/context-handoff.md`
- `HANDOFF.md` in your project root

Every session overwrote it. If you ran two terminals, the last one to finish won.

## The New Way

Timestamped files in a directory. No overwrites.

```
~/.claude/handoffs/
├── 2026-03-14_093022_auth-refactor.md
├── 2026-03-14_141500_api-fix.md
├── 2026-03-13_160000_deploy-staging_done.md    # consumed
└── 2026-03-12_090000_db-migration_done.md      # consumed, will be cleaned
```

## Migration Steps

### 1. Create the directory

```bash
mkdir -p ~/.claude/handoffs
```

### 2. Move your existing handoff (optional)

If you have a current handoff file you want to preserve:

```bash
mv ~/.claude/context-handoff.md ~/.claude/handoffs/$(date +%Y-%m-%d_%H%M%S)_migrated-handoff.md
```

### 3. Update your CLAUDE.md

Replace any single-file handoff references:

**Before:**
```markdown
## Session End
Write context to `~/.claude/context-handoff.md`
```

**After:**
```markdown
## Session End
Write handoff to `~/.claude/handoffs/YYYY-MM-DD_HHMMSS_<slug>.md`
Never overwrite another session's handoff.
```

And add the session start protocol:

```markdown
## Session Start
1. Read all unconsumed handoffs: `ls ~/.claude/handoffs/*.md 2>/dev/null | grep -v '_done.md$'`
2. After reading, mark each consumed: rename `file.md` to `file_done.md`
3. Clean up old consumed handoffs: `find ~/.claude/handoffs -name '*_done.md' -mtime +7 -delete 2>/dev/null`
```

### 4. Legacy compatibility

Keep your old file path in the session start check if other tools reference it:

```bash
# Check legacy location too
test -f ~/.claude/context-handoff.md && echo "Legacy handoff exists"
```

The old file won't be overwritten by the new system. It just sits there until you manually remove it.

## Verification

After migrating, run two sessions simultaneously. End both. Check that two separate files exist:

```bash
ls -la ~/.claude/handoffs/*.md | grep -v '_done.md$'
```

You should see two files with different timestamps. That's how you know it's working.
