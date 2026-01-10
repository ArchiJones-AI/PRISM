---
domain: plan-mode
semantic_triggers: plan mode, planning, research mode, Shift+Tab, explore, Plan subagent, read-only mode, codebase research, architecture planning, --permission-mode plan, opus plan mode, exploration, investigate, understand codebase
priority_sources:
  - https://code.claude.com/docs/en/common-workflows#how-to-use-plan-mode
  - https://www.anthropic.com/engineering/claude-code-best-practices
  - https://code.claude.com/docs/en/sub-agents
related_domains:
  - 09-thinking.md
  - 02-subagents.md
  - 13-workflows.md
  - 06-memory.md
---

# Plan Mode Reference

## Quick Answer

Plan Mode is **read-only research mode** — Claude can read files, search code, and run safe bash commands, but **cannot modify anything**. Perfect for exploring unfamiliar codebases, planning architecture, or understanding code before making changes. Activate with `Shift+Tab+Tab` or `--permission-mode plan`. Combine with Opus for deep strategic planning.

## Key Concepts

### What Plan Mode Does

| Allowed | Blocked |
|---------|---------|
| ✅ Read files | ❌ Write files |
| ✅ Search code (Grep, Glob) | ❌ Edit files |
| ✅ Run read-only bash | ❌ Create files |
| ✅ Web search | ❌ Delete files |
| ✅ Analyse and explain | ❌ Git commits |

### Activation Methods

| Method | How | Use Case |
|--------|-----|----------|
| **Keyboard** | `Shift+Tab+Tab` | Quick toggle during conversation |
| **CLI Flag** | `claude --permission-mode plan` | Start in plan mode |
| **Plan Subagent** | Automatic | Spawned for research tasks |

### Plan Mode vs Edit Mode

```
┌─────────────────────────────────────────────────────────────┐
│                      PLAN MODE                               │
│  "Let me understand this codebase first"                    │
│                                                             │
│  ✓ Read and analyse code                                    │
│  ✓ Create mental model of architecture                      │
│  ✓ Identify patterns and dependencies                       │
│  ✓ Plan implementation approach                             │
│  ✓ Estimate complexity and risks                            │
└─────────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│                      EDIT MODE                               │
│  "Now let me implement the changes"                         │
│                                                             │
│  ✓ Write and modify files                                   │
│  ✓ Create new components                                    │
│  ✓ Run tests                                                │
│  ✓ Commit changes                                           │
└─────────────────────────────────────────────────────────────┘
```

### Opus Plan Mode

A special configuration using **Opus for planning, Sonnet for execution**:

```
┌───────────────────────┐     ┌───────────────────────┐
│   OPUS (Planning)     │────▶│  SONNET (Execution)   │
│                       │     │                       │
│ • Deep analysis       │     │ • Fast implementation │
│ • Architecture design │     │ • Code generation     │
│ • Risk assessment     │     │ • Testing             │
│ • Strategic decisions │     │ • Iteration           │
└───────────────────────┘     └───────────────────────┘
```

**When to use Opus Plan Mode:**
- Complex architectural decisions
- Large-scale refactoring
- System design
- Migration planning

## Authoritative Sources

| Source | Type | URL |
|--------|------|-----|
| Plan Mode Documentation | Official Docs | https://code.claude.com/docs/en/common-workflows#how-to-use-plan-mode |
| Best Practices | Blog | https://www.anthropic.com/engineering/claude-code-best-practices |
| Sub-agents (Plan agent) | Official Docs | https://code.claude.com/docs/en/sub-agents |

## Common Patterns

### Explore → Plan → Code → Commit Workflow

This is Anthropic's recommended workflow:

```markdown
# Phase 1: EXPLORE (Plan Mode)
- Read relevant files
- Understand architecture
- Map dependencies
- Identify patterns
- DO NOT write code yet

# Phase 2: PLAN (Plan Mode)
- Create implementation plan
- Break into steps
- Identify risks
- Save plan to file

# Phase 3: CODE (Edit Mode)
- Implement one step at a time
- Test after each step
- Refer back to plan

# Phase 4: COMMIT
- Review all changes
- Write descriptive commit
- Create PR if needed
```

### Starting a New Feature
```
I need to add user notifications to this app. Let's use Plan Mode first.

EXPLORATION PHASE (read-only):
1. Find existing notification-related code
2. Understand the current architecture
3. Identify where notifications should integrate
4. Look for similar patterns in the codebase

Don't write any code yet—just create a plan document.
```

### Codebase Exploration
```
I'm new to this codebase. In Plan Mode, help me understand:

1. Overall architecture (what frameworks, patterns)
2. Directory structure (what's where)
3. Data flow (how requests are processed)
4. Key files I should know about
5. Testing approach

Create a summary document I can reference later.
```

### Architecture Planning
```
We need to migrate from REST to GraphQL. In Plan Mode:

1. Audit current REST endpoints
2. Map data models and relationships
3. Design GraphQL schema
4. Identify breaking changes
5. Plan migration phases
6. Estimate effort per phase

Save the plan to docs/graphql-migration-plan.md (just the content, I'll save it after review).
```

### Using Plan Subagent
```
Use the Plan subagent to analyse our authentication system:

1. Map all auth-related files
2. Document the current flow
3. Identify security concerns
4. Suggest improvements

This is research only—don't make any changes.
```

## Prompt Templates

### Initial Exploration
```
[Plan Mode] I just cloned this repository and need to understand it.

1. What does this project do?
2. What are the main technologies?
3. How is it structured?
4. What are the entry points?
5. How do I run it locally?

Read the relevant files and give me a comprehensive overview.
```

### Pre-Implementation Planning
```
[Plan Mode] Before I implement feature X, I need a plan.

Research:
1. What existing code relates to this feature?
2. What patterns does the codebase use?
3. What dependencies will I need?

Planning:
1. Break down into implementation steps
2. Identify potential challenges
3. Estimate complexity (1-5) for each step
4. Note any prerequisites

Output a structured implementation plan.
```

### Code Review Preparation
```
[Plan Mode] I need to review PR #123. Analyse:

1. What files were changed and why?
2. Do changes follow project patterns?
3. Are there any security concerns?
4. Is test coverage adequate?
5. Any performance implications?

Give me a review summary with specific concerns.
```

### Debugging Investigation
```
[Plan Mode] Users report slow page loads on /dashboard.

Investigate:
1. Trace the request flow
2. Identify database queries
3. Look for N+1 problems
4. Check for missing indexes
5. Find potential bottlenecks

Don't fix anything yet—just diagnose and report.
```

## Troubleshooting

### Plan Mode Not Activating
```bash
# Check current mode in UI (should show "Plan Mode" indicator)

# Try CLI flag
claude --permission-mode plan

# Keyboard shortcut
# Shift + Tab + Tab (not just Tab)
```

### Can't Switch to Edit Mode
```bash
# Shift + Tab + Tab again to toggle
# Or use /mode command if available
# Or start new session without --permission-mode flag
```

### Plan Subagent Not Working
- Plan subagent is built-in, no configuration needed
- Make sure you're explicitly requesting research/planning task
- Use "Plan subagent" or "use Plan mode" in your prompt

### Accidentally Modified Files in Plan Mode
- Plan Mode should block writes
- If writes happened, verify you're actually in Plan Mode
- Check `--permission-mode` flag value

## Best Practices

### When to Use Plan Mode

✅ **Use Plan Mode for:**
- Exploring unfamiliar codebases
- Understanding complex systems
- Architecture decisions
- Pre-implementation planning
- Code review preparation
- Debugging investigation
- Documentation research

❌ **Don't Use Plan Mode for:**
- Quick fixes you understand well
- Simple, isolated changes
- Tasks where exploration isn't needed
- Time-critical hotfixes

### Saving Plans

Plans are valuable—don't lose them:

```
After planning, save the plan:
1. Copy the plan content
2. Exit Plan Mode (Shift+Tab+Tab)
3. Create the file: /path/to/plan.md
4. Reference it during implementation
```

### Combining with Ultrathink

For complex planning, combine Plan Mode with extended thinking:

```
[Plan Mode + Ultrathink]

I need to design a new microservices architecture. 
Think deeply about:
1. Service boundaries
2. Communication patterns
3. Data consistency
4. Deployment strategy
5. Failure modes

Take your time to reason through the trade-offs.
```

## Related Domains

- **[Thinking](09-thinking.md)** — Combine ultrathink with Plan Mode
- **[Subagents](02-subagents.md)** — Plan subagent details
- **[Workflows](13-workflows.md)** — Explore → Plan → Code workflow
- **[Memory](06-memory.md)** — Saving plans for reference
