---
domain: thinking
semantic_triggers: think, thinking, ultrathink, think hard, think harder, megathink, extended thinking, thinking budget, reasoning, deep analysis, MAX_THINKING_TOKENS, verbose mode, Ctrl+O, reasoning mode, deep thought
priority_sources:
  - https://code.claude.com/docs/en/common-workflows
  - https://www.anthropic.com/engineering/claude-code-best-practices
related_domains:
  - 07-plan-mode.md
  - 13-workflows.md
  - 02-subagents.md
---

# Extended Thinking Reference

## Quick Answer

Extended thinking triggers **deep reasoning mode** where Claude spends more tokens "thinking" before responding. **CRITICAL:** Since v2.0.0, only `ultrathink` reliably triggers extended thinking (32K tokens). Keywords like "think hard" are deprecated. Toggle thinking visibility with `Ctrl+O` (verbose mode). Use ultrathink for complex architecture, debugging, and strategic decisions—but sparingly, as it burns API budget.

## Key Concepts

### ⚠️ IMPORTANT: v2.0.0 Breaking Change

| Keyword | Pre-v2.0.0 | Post-v2.0.0 |
|---------|------------|-------------|
| `think` | ~4K tokens | ❌ Deprecated |
| `think hard` | ~10K tokens | ❌ Deprecated |
| `think harder` | ~20K tokens | ❌ Deprecated |
| `ultrathink` | ~32K tokens | ✅ **Only working keyword** |
| `megathink` | Not official | ❌ Never worked |

**If you need extended thinking, use `ultrathink` explicitly.**

### Token Budgets

| Mode | Approximate Tokens | Cost Impact | Use Case |
|------|-------------------|-------------|----------|
| Standard | ~1-2K | Low | Simple tasks |
| Ultrathink | ~32K | High | Complex reasoning |

### How Thinking Works

```
Standard Response:
User Question → Brief Analysis → Answer

Ultrathink Response:
User Question → Deep Analysis (hidden) → 
  ├─ Consider multiple approaches
  ├─ Weigh trade-offs
  ├─ Anticipate edge cases
  ├─ Validate reasoning
  └─ Synthesise conclusion
→ Comprehensive Answer
```

### Viewing Thinking Output

| Method | How | Shows |
|--------|-----|-------|
| **Verbose Mode** | `Ctrl+O` | Grey italic thinking text |
| **Tab Toggle** | `Tab` key | Toggle thinking on/off |
| **Environment** | `MAX_THINKING_TOKENS=32000` | Override token budget |

## Authoritative Sources

| Source | Type | URL |
|--------|------|-----|
| Common Workflows | Official Docs | https://code.claude.com/docs/en/common-workflows |
| Best Practices | Blog | https://www.anthropic.com/engineering/claude-code-best-practices |

## Common Patterns

### When to Use Ultrathink

✅ **Use ultrathink for:**
- Complex architecture decisions
- Multi-step debugging
- System design
- Migration planning
- Security analysis
- Performance optimisation strategy
- Untangling legacy code
- Code review of critical systems

❌ **Don't use ultrathink for:**
- Simple code generation
- Formatting/linting
- Straightforward bug fixes
- Routine tasks
- Quick questions

### Architecture Decision
```
ultrathink

I need to design the data layer for a real-time collaborative editor.
Requirements:
- Multiple users editing simultaneously
- Conflict resolution
- Offline support
- 10,000+ concurrent users

Consider:
1. CRDT vs OT approaches
2. Storage architecture
3. Sync protocols
4. Scaling strategy

Take your time to reason through the trade-offs before recommending an approach.
```

### Complex Debugging
```
ultrathink

We have a race condition that only occurs in production under high load.
Symptoms:
- Occasional duplicate records
- Inconsistent state between services
- Only happens ~1 in 10,000 requests

Relevant code: [paste code]

Analyse potential causes, considering:
- Concurrency issues
- Transaction boundaries
- Cache invalidation
- Message ordering

Think through each possibility systematically.
```

### Security Review
```
ultrathink

Review this authentication system for security vulnerabilities.

[paste auth code]

Consider:
- OWASP Top 10
- JWT handling
- Session management
- Input validation
- Error messages (info leakage)
- Timing attacks
- Rate limiting

Be thorough and think through attack vectors.
```

### Migration Planning
```
ultrathink

We need to migrate from monolith to microservices.
Current system: [description]
Team size: 8 developers
Timeline: 6 months

Plan a migration strategy considering:
1. Service boundaries
2. Data decomposition
3. Migration sequence
4. Risk mitigation
5. Rollback procedures
6. Team capacity

Think deeply about the sequencing and dependencies.
```

## Prompt Templates

### Enable Ultrathink for Task
```
ultrathink

[Your complex task here]

Take your time to think through this thoroughly. Consider multiple approaches, weigh trade-offs, and validate your reasoning before responding.
```

### Combine with Plan Mode
```
[Plan Mode + Ultrathink]

Before implementing this feature, I need a comprehensive plan.

ultrathink

Research the codebase and design an implementation approach for [feature].
Consider architecture, edge cases, testing strategy, and rollout plan.
```

### Override Token Budget
```bash
# In terminal
export MAX_THINKING_TOKENS=50000
claude

# Then in conversation
ultrathink
[Your complex task]
```

## Troubleshooting

### Thinking Not Activating
```bash
# Verify keyword
# Only "ultrathink" works since v2.0.0

# Check verbose mode to see thinking
# Press Ctrl+O

# Try explicit instruction
"Use extended thinking mode (ultrathink) to analyse this problem deeply."
```

### Can't See Thinking Output
```
# Toggle verbose mode
Ctrl+O

# Or Tab key to toggle thinking visibility
Tab
```

### Thinking Too Slow/Expensive
- Only use ultrathink when complexity warrants it
- For simpler tasks, omit the keyword
- Break large tasks into smaller pieces
- Use Plan Mode for research (doesn't need ultrathink)

### Common Errors

| Issue | Cause | Solution |
|-------|-------|----------|
| Thinking not triggered | Wrong keyword | Use exactly `ultrathink` |
| Response seems shallow | v2.0.0 breaking change | Update to `ultrathink` |
| Very slow response | Extended thinking working | Wait, or reduce scope |
| High API costs | Frequent ultrathink use | Reserve for complex tasks |

## Best Practices

### Priming for Deep Thought
Add context that encourages thorough analysis:

```
ultrathink

Before answering, consider:
- What are the key constraints?
- What are the trade-offs?
- What could go wrong?
- What are the alternatives?
- How would an expert approach this?

[Your question]
```

### Combining with Subagents
For very complex tasks, use ultrathink with subagents:

```
ultrathink

This is a complex analysis. I want you to:

1. Use the Plan subagent to research the codebase
2. Think deeply about what you find
3. Identify the key architectural patterns
4. Propose improvements

Take your time on the analysis phase.
```

### Knowing When to Stop
Ultrathink isn't always better:

```
# Simple task - DON'T use ultrathink
"Add a loading spinner to the button"

# Complex task - DO use ultrathink  
"Design the state management architecture for real-time collaboration"
```

### Cost Management
```
# Check thinking usage
# Look for thinking output in verbose mode (Ctrl+O)
# Long grey italic sections = high token usage

# Reduce costs by:
# 1. Only using ultrathink when needed
# 2. Breaking large tasks into phases
# 3. Using Plan Mode for research (cheaper)
# 4. Reviewing thinking output to ensure value
```

## Environment Configuration

### MAX_THINKING_TOKENS
```bash
# Set custom thinking budget
export MAX_THINKING_TOKENS=50000

# Or in shell profile
echo 'export MAX_THINKING_TOKENS=32000' >> ~/.bashrc
```

### Per-Session Override
```bash
# Start Claude with specific budget
MAX_THINKING_TOKENS=64000 claude
```

## Related Domains

- **[Plan Mode](07-plan-mode.md)** — Combine with ultrathink for deep planning
- **[Workflows](13-workflows.md)** — When to use extended thinking
- **[Subagents](02-subagents.md)** — Using thinking with agents
- **[Model Selection](../cross-cutting/model-selection.md)** — Opus + ultrathink for maximum depth
