---
domain: workflows
semantic_triggers: workflow, best practice, pattern, how to, setup, configure, strategy, approach, methodology, TDD, test-driven, explore plan code commit, multi-agent, parallel, git worktree, session, productivity, efficient
priority_sources:
  - https://www.anthropic.com/engineering/claude-code-best-practices
  - https://code.claude.com/docs/en/common-workflows
  - https://neon.com/blog/our-claude-code-cheatsheet
related_domains:
  - 07-plan-mode.md
  - 06-memory.md
  - 09-thinking.md
  - 02-subagents.md
---

# Workflows & Best Practices Reference

## Quick Answer

The **Explore → Plan → Code → Commit** workflow is Anthropic's recommended approach: research first (Plan Mode), design implementation, code incrementally, then commit. Use `/clear` between major tasks, limit sessions to 2-3 hours, and leverage subagents for parallel work. Context management is the key to productivity.

## The "Explore → Plan → Code → Commit" Workflow

This is the gold standard workflow recommended by Anthropic:

```
┌─────────────────────────────────────────────────────────────┐
│  PHASE 1: EXPLORE (Plan Mode)                               │
│  ─────────────────────────────                              │
│  • Read relevant files                                      │
│  • Use subagents to research                                │
│  • Understand architecture                                  │
│  • Map dependencies                                         │
│  • NO CODE CHANGES                                          │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  PHASE 2: PLAN (Plan Mode)                                  │
│  ────────────────────────                                   │
│  • Create implementation plan                               │
│  • Break into clear steps                                   │
│  • Identify risks                                           │
│  • Save plan to file                                        │
│  • Get buy-in (review plan before proceeding)               │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  PHASE 3: CODE (Edit Mode)                                  │
│  ─────────────────────────                                  │
│  • Implement ONE step at a time                             │
│  • Test after each step                                     │
│  • Refer back to plan                                       │
│  • Request checkpoint if needed                             │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  PHASE 4: COMMIT                                            │
│  ───────────────────                                        │
│  • Review all changes                                       │
│  • Run final tests                                          │
│  • Write descriptive commit message                         │
│  • Create PR (if applicable)                                │
└─────────────────────────────────────────────────────────────┘
```

### Why This Works
- **Prevents "jump to coding"** — Claude's default is to start coding immediately
- **Preserves context** — Research with subagents doesn't consume main context
- **Creates checkpoints** — Plan document serves as recovery point
- **Improves quality** — Thought-out approach vs. trial-and-error

## Context Management Strategy

### The 2-3 Hour Rule
- **Recommendation:** Keep sessions under 2-3 hours
- **Why:** Context window fills up, compaction loses detail
- **How:** Use `/clear` and start fresh for new tasks

### When to Use /clear
```bash
# ✅ Use /clear when:
- Switching to unrelated task
- Context feels cluttered
- Claude seems confused
- After completing a major feature
- Session has been running 2+ hours

# ❌ Don't use /clear when:
- Mid-task (unless stuck)
- You need previous context
- Debugging issue from earlier
```

### Context-Preserving Strategies
```bash
# Save important state to files
"Before I clear, save the current plan to docs/current-plan.md"

# Use subagents for heavy research
"Use a subagent to research the API documentation - keep the main context clean"

# Checkpoint conversation
"Summarise what we've accomplished so far"
/clear
"Continue from: [paste summary]"
```

## Test-Driven Development (TDD) Workflow

```
┌─────────────────────────────────────────────────────────────┐
│  1. WRITE FAILING TEST                                      │
│     "Write a test for [feature] that captures the          │
│      requirements. Don't implement yet."                    │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  2. VERIFY TEST FAILS                                       │
│     "Run the test. Confirm it fails for the right reason." │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  3. IMPLEMENT MINIMAL CODE                                  │
│     "Implement just enough to make the test pass.          │
│      No more, no less."                                     │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  4. VERIFY TEST PASSES                                      │
│     "Run the test. Confirm it passes."                      │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  5. REFACTOR                                                │
│     "Refactor the implementation. Tests must still pass."   │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
                    [Repeat]
```

### TDD Prompt
```
Let's implement [feature] using TDD:

1. Write a comprehensive test first
2. Run it - verify it fails
3. Implement minimum code to pass
4. Run test - verify it passes
5. Refactor if needed
6. Repeat for next requirement

Start with the test.
```

## Parallel Subagent Workflows

### When to Use Parallel Agents
- Independent research tasks
- Multiple file analyses
- Different perspectives on same problem
- Time-consuming operations

### Pattern: Parallel Research
```
I need to understand three subsystems. Run these in parallel:

1. Agent 1: Research the authentication system (src/auth/)
2. Agent 2: Research the database layer (src/db/)
3. Agent 3: Research the API routes (src/api/)

Each agent should produce a summary. Then synthesise the findings.
```

### Pattern: Parallel Review
```
Review this PR from three perspectives simultaneously:

1. Security agent: Focus on vulnerabilities
2. Performance agent: Focus on efficiency
3. Style agent: Focus on conventions

Consolidate into one review with priorities.
```

## Visual Iteration Workflow

For UI development:

```
┌─────────────────────────────────────────────────────────────┐
│  1. CREATE INITIAL UI                                       │
│     "Build a [component] with [requirements]"               │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  2. SCREENSHOT & REVIEW                                     │
│     Take screenshot, paste into Claude                      │
│     "Here's the current state. Issues: [list]"              │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  3. ITERATE                                                 │
│     "Fix [specific issues from screenshot]"                 │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
                    [Repeat until satisfied]
```

## Git Worktree Pattern

For multiple parallel features:

```bash
# Create worktrees for parallel development
git worktree add ../project-feature-a feature-a
git worktree add ../project-feature-b feature-b

# Run Claude Code in each worktree
cd ../project-feature-a && claude
# (separate terminal)
cd ../project-feature-b && claude
```

**Benefits:**
- Completely isolated contexts
- No branch switching
- Parallel feature development
- Clean git history

## Authoritative Sources

| Source | Type | URL |
|--------|------|-----|
| Best Practices | Blog | https://www.anthropic.com/engineering/claude-code-best-practices |
| Common Workflows | Docs | https://code.claude.com/docs/en/common-workflows |
| Neon Cheatsheet | Community | https://neon.com/blog/our-claude-code-cheatsheet |

## Prompt Templates

### Feature Development
```
I need to implement [feature description].

Let's use the Explore → Plan → Code → Commit workflow:

PHASE 1 - EXPLORE:
First, research the codebase to understand:
- How similar features are implemented
- What patterns to follow
- Dependencies involved

Use Plan Mode. Don't write any code yet.
```

### Bug Investigation
```
There's a bug: [description]

Workflow:
1. [Plan Mode] Investigate the likely cause
2. [Plan Mode] Form hypothesis
3. [Edit Mode] Add logging/debugging
4. [Test] Verify hypothesis
5. [Fix] Implement solution
6. [Verify] Confirm fix works
7. [Cleanup] Remove debug code
8. [Commit] Document the fix

Start with investigation.
```

### Refactoring
```
I need to refactor [component/system].

Workflow:
1. [Plan Mode] Understand current implementation
2. [Plan Mode] Design target state
3. [Ensure] Write/verify tests for current behaviour
4. [Incremental] Refactor in small steps
5. [After each step] Run tests
6. [Complete] Review full diff
7. [Commit] Clear commit messages for each logical change

Start with understanding current state.
```

### Code Review Preparation
```
Prepare my changes for code review:

1. Run all tests
2. Run linter
3. Check for debug code / console.logs
4. Review diff for sensitive data
5. Update documentation if needed
6. Write clear commit messages
7. Generate PR description

Report any issues found.
```

## Anti-Patterns to Avoid

### ❌ Don't: Jump Straight to Coding
```
# Bad
"Implement user authentication"
# Claude starts coding immediately without understanding context

# Good
"Let's implement user authentication. First, in Plan Mode, 
research how auth is done elsewhere in this codebase."
```

### ❌ Don't: Infinite Sessions
```
# Bad
Running same session for 8+ hours
Context becomes polluted, Claude gets confused

# Good
2-3 hour sessions
/clear between major tasks
Fresh start for new features
```

### ❌ Don't: One Giant Prompt
```
# Bad
"Build an entire e-commerce system with products, cart, 
checkout, payments, admin panel, reports..."

# Good
Break into phases:
1. First: Product catalog
2. Then: Shopping cart
3. Then: Checkout flow
[etc.]
```

### ❌ Don't: Skip Testing
```
# Bad
"Implement this feature"
[No tests]
[Ship it]

# Good
TDD or at minimum:
"After implementation, write tests that cover:
- Happy path
- Edge cases
- Error conditions"
```

## Session Checklist

### Start of Session
- [ ] `/init` to load fresh memory
- [ ] Verify correct project directory
- [ ] Check `/model` is appropriate
- [ ] Review what was accomplished last session

### During Session
- [ ] Use Plan Mode for research
- [ ] Leverage subagents for parallel work
- [ ] Test incrementally
- [ ] Save important state to files
- [ ] `/clear` between unrelated tasks

### End of Session
- [ ] Commit all work
- [ ] Document any incomplete work
- [ ] Save session summary
- [ ] Note next steps

## Related Domains

- **[Plan Mode](07-plan-mode.md)** — Explore/Plan phases
- **[Memory](06-memory.md)** — Context management
- **[Thinking](09-thinking.md)** — When to use ultrathink
- **[Subagents](02-subagents.md)** — Parallel execution
- **[Commands](12-commands-shortcuts.md)** — /clear, /init usage
