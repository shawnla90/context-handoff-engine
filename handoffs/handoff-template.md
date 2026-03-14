# Handoff Template

Copy this template for every session handoff. All 5 sections are required.

---

```markdown
# Context Handoff
> Generated: YYYY-MM-DD HH:MM | Session: [brief label]

## What Was Done This Session
[Bullet list of completed work. Be specific with file paths.]

- Created `src/auth/middleware.ts` - JWT validation middleware
- Updated `src/routes/users.ts` - added pagination (cursor-based)
- Fixed failing test in `tests/auth.test.ts` - mock was stale

## Current State
- **Git**: main, clean, `a1b2c3d feat: add JWT auth middleware`
- **Uncommitted changes**: none
- **Blocked on**: waiting for API key from staging environment

## Next Steps
1. Add rate limiting to auth middleware (`src/auth/middleware.ts`)
2. Write integration tests for pagination (`tests/routes/users.test.ts`)
3. Deploy to staging once API key arrives

## Key Decisions Made
- Using cursor-based pagination instead of offset (scales better with large datasets)
- JWT tokens expire after 1 hour, refresh tokens after 7 days
- Auth middleware runs on all routes except `/health` and `/login`

## Files to Read First
1. `src/auth/middleware.ts` - new auth layer, entry point for the feature
2. `src/routes/users.ts` - pagination implementation
3. `tests/auth.test.ts` - test patterns for auth
```

---

## Rules

1. **Be specific** - file paths, commit hashes, line numbers. Vague handoffs are useless.
2. **Don't summarize the whole repo** - only include what changed THIS session.
3. **Include blockers** - if something failed or was skipped, say why.
4. **Keep it under 100 lines** - this is a handoff, not a novel.
5. **Always capture git state** - run `git status` and `git log -1 --oneline` before writing.
6. **Never overwrite another session's handoff.** Each session gets its own file.
7. **Consume on read.** Mark handoffs `_done` after reading them.
8. **Auto-clean.** Delete `_done` handoffs older than 7 days.
