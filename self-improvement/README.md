# Layer 3: Self-Improvement Loop

The self-improvement loop turns one-time corrections into permanent rules. Without it, you correct Claude on Monday and it makes the same mistake on Wednesday.

## The Cycle

```
User corrects agent
       ↓
Agent writes lesson to tasks/lessons.md
       ↓
Lesson includes: date, context, and a rule
       ↓
On next session start, agent reads all lessons
       ↓
Agent follows every rule going forward
       ↓
Mistake rate drops over time
```

## How It Works

### 1. After ANY correction

When the user corrects you - whether it's a code pattern, a preference, a naming convention, or a workflow issue - immediately write a lesson:

```markdown
## 2026-03-14: Always use cursor-based pagination

**Context:** Built offset-based pagination for /users endpoint. User pointed out this breaks with concurrent writes and large datasets.

**Rule:** Use cursor-based pagination for all list endpoints. Offset-based pagination is only acceptable for static, small datasets.
```

### 2. On session start

Read `tasks/lessons.md` before doing anything else. Every lesson is a rule. Follow them all.

### 3. Ruthless iteration

If the same type of mistake recurs, the lesson wasn't specific enough. Update it with more detail, more examples, or a stronger rule.

## What Makes a Good Lesson

**Good:** Specific, actionable, includes context and a clear rule.

```markdown
## 2026-03-14: Shared package dependency management
- Never add heavy SDK packages as direct dependencies in shared packages.
  They corrupt the lockfile and break CSS generation across all apps.
  Use peerDependencies instead.
- After npm install changes, verify the site visually. A clean build
  doesn't mean the site looks right.
```

**Bad:** Vague, no context, no actionable rule.

```markdown
## 2026-03-14: Be careful with dependencies
- Dependencies can cause issues. Check things.
```

## CLAUDE.md Integration

Add this to your `CLAUDE.md`:

```markdown
## Self-Improvement Loop

- After ANY correction from the user: update `tasks/lessons.md` with date, context, and lesson.
- Write rules that prevent the same mistake from recurring.
- Review `tasks/lessons.md` at session start.
```

## Task Tracking

The `tasks/todo.md` file works alongside lessons:

1. Write plan to `tasks/todo.md` with checkable items before starting
2. Mark items complete as you go
3. Add a review section when done

Lessons capture what went wrong. Todos track what to do. Together they give the agent both direction and guardrails.

See [lessons-template.md](lessons-template.md) and [todo-template.md](todo-template.md) for starting templates.
