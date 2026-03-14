# Quick Reference: Signal to Route

| Signal | Route | Why |
|--------|-------|-----|
| "Add a config entry" | A - Single session | 1 file, no coordination |
| "Fix the auth bug" | A - Single session | Investigation + fix, sequential |
| "Build 5 pages that mirror the existing pattern" | B - Parallel subagents | Same pattern, independent outputs |
| "Create 3 independent data files" | B - Parallel subagents | No dependencies between outputs |
| "Research these 4 topics" | B - Parallel subagents | Independent research, parallel |
| "Build a feature with data, pages, nav, and deploy" | C - Team | Multiple concerns, handoffs needed |
| "Set up a content pipeline with writer and reviewer" | C - Team | Cross-review needed |
| "Update the API and push to staging" | A - Single session | Sequential, one concern |
| "Research this topic thoroughly" | A - Single + 1 subagent | Protect context window |
| "Refactor auth across frontend and backend" | C - Team | Different expertise per layer |

## Decision Announcement Template

Before executing, tell the user what you decided and why:

```
Routing: Pattern [A/B/C]
- Files to edit: [count]
- Concerns: [list]
- Handoffs needed: [yes/no]
- Review needed: [yes/no]
- Quality gate: [yes/no]
- Why this pattern: [1 sentence]
```

This gives the user a chance to override. If they say "use teams anyway," respect that.
