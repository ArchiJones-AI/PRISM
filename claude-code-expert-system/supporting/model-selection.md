---
type: cross-cutting
purpose: Guide for selecting appropriate Claude model (Opus, Sonnet, Haiku)
last_updated: 2025-12
---

# Model Selection Guide

## Quick Decision

| Task Type | Model | Why |
|-----------|-------|-----|
| Daily coding, rapid iteration | **Sonnet** | 2x faster, 5x cheaper |
| Complex architecture, deep analysis | **Opus** | Superior reasoning |
| Simple completions, cost-sensitive | **Haiku** | Fastest, cheapest |

---

## Model Specifications

| Model | Alias | Cost (Input/Output per 1M tokens) | Speed | Context |
|-------|-------|-----------------------------------|-------|---------|
| **Claude Opus 4.5** | `opus` | $15 / $75 | Slowest | 200K |
| **Claude Sonnet 4.5** | `sonnet` | $3 / $15 | 2x Opus | 200K |
| **Claude Haiku 4.5** | `haiku` | $0.80 / $4 | Fastest | 200K |

### Relative Performance

```
Speed:     Haiku ████████████ > Sonnet ████████ > Opus ████
Cost:      Haiku █ < Sonnet ███ < Opus ███████████████
Quality:   Opus ████████████ > Sonnet ████████ > Haiku ████
```

---

## Detailed Recommendations

### Use Sonnet (Default) When...

✅ **Best for:**
- Daily development work
- Code generation and editing
- Bug fixing
- Refactoring
- Test writing
- Documentation
- Most routine tasks

**Why Sonnet:**
- Excellent coding performance (72.7% SWE-bench)
- 2x faster than Opus
- 5x cheaper than Opus
- "Pragmatic accelerator" — gets work done efficiently

**Example tasks:**
```
- "Implement this React component"
- "Fix this bug in the auth flow"
- "Write tests for this function"
- "Refactor this class to use composition"
```

---

### Use Opus When...

✅ **Best for:**
- Complex architectural decisions
- System design
- Deep code analysis
- Strategic planning
- Difficult debugging
- Security audits
- Migration planning
- When you need "eerie ability to connect disparate information"

**Why Opus:**
- Superior reasoning and synthesis
- Better at multi-step analysis
- More thorough exploration of trade-offs
- "Deliberative strategist" — thinks deeply

**Example tasks:**
```
- "Design the data architecture for real-time collaboration"
- "Analyse this legacy system and plan migration strategy"
- "Review this codebase for security vulnerabilities"
- "Plan how to scale this to 10x traffic"
```

**Cost consideration:** Reserve Opus for tasks where quality of insight justifies 5x cost.

---

### Use Haiku When...

✅ **Best for:**
- Simple completions
- High-frequency, low-stakes tasks
- Cost-sensitive scenarios
- Quick lookups
- Prompt-based hooks (fast evaluation)
- Batch processing simple items

**Why Haiku:**
- Fastest response time
- Lowest cost
- Good for simple, well-defined tasks
- Now supports Extended Thinking!

**Example tasks:**
```
- "Format this JSON"
- "Generate a simple regex for emails"
- "Explain this error message"
- "Convert this to TypeScript types"
```

**Limitation:** Struggles with complex multi-step reasoning.

---

## Opus Plan Mode

A special workflow using **Opus for planning, Sonnet for execution**:

```
┌─────────────────────────────┐
│    OPUS (Planning Phase)    │
│    ─────────────────────    │
│    • Deep analysis          │
│    • Architecture design    │
│    • Risk identification    │
│    • Strategic decisions    │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│   SONNET (Execution Phase)  │
│   ──────────────────────    │
│   • Implementation          │
│   • Testing                 │
│   • Iteration               │
│   • Rapid feedback          │
└─────────────────────────────┘
```

### How to use:
```bash
# Start with Opus for planning
/model opus
"Analyse this codebase and design the new auth system"

# Switch to Sonnet for implementation
/model sonnet
"Implement the auth system based on the plan"
```

---

## Switching Models

### Via Command
```bash
/model sonnet
/model opus
/model haiku
```

### Via CLI Flag
```bash
claude --model opus
claude --model claude-sonnet-4-5-20250929
```

### In Subagent Definition
```markdown
---
name: architect
description: Makes architectural decisions
model: opus
---
```

### In Agent SDK
```python
options = ClaudeAgentOptions(
    model="opus"
)
```

---

## Cost Optimisation Strategies

### 1. Default to Sonnet
```bash
# Set as default
/model sonnet

# Only switch to Opus for specific tasks
/model opus
"[Complex task]"
/model sonnet
```

### 2. Use Haiku for Hooks
```json
{
  "hooks": [
    {
      "event": "PreToolUse",
      "type": "prompt",
      "prompt": "Is this safe? Reply ALLOW or BLOCK."
    }
  ]
}
// Haiku used automatically for prompt hooks
```

### 3. Subagents for Heavy Research
```markdown
---
name: researcher
model: opus
tools: Read, Grep, Glob
---
# Opus used only for research phase
```

### 4. Monitor Usage
```bash
# Install cost tracker
npm install -g ccusage

# Check daily spend
ccusage --today
```

---

## Model Selection by Domain

| Domain | Recommended | Reasoning |
|--------|-------------|-----------|
| Plugins | Sonnet | Standard complexity |
| Subagents | Varies | Define per agent |
| Skills | N/A | Skills don't run separately |
| Hooks (prompt) | Haiku | Fast evaluation |
| MCP | Sonnet | Tool usage |
| Memory | Sonnet | Context loading |
| Plan Mode | Opus | Deep research |
| Headless | Sonnet | Efficiency |
| Thinking | Sonnet + ultrathink | Extended reasoning |
| Troubleshooting | Sonnet | Most issues |
| Complex debug | Opus | Deep analysis |

---

## Anti-Patterns

### ❌ Don't: Use Opus for Everything
```
# Expensive and slow
/model opus
"Fix this typo"  # Overkill!
```

### ❌ Don't: Use Haiku for Complex Tasks
```
# Will struggle
/model haiku
"Design microservices architecture"  # Too complex!
```

### ❌ Don't: Forget to Switch Back
```
/model opus
"[Complex task]"
# ... forgot to switch ...
"Fix this simple bug"  # Still using expensive Opus!
```

---

## Decision Framework

```
Is the task complex and strategic?
├── YES: Does it require deep reasoning?
│   ├── YES → OPUS
│   └── NO → SONNET
└── NO: Is it simple and well-defined?
    ├── YES: Is cost a concern?
    │   ├── YES → HAIKU
    │   └── NO → SONNET
    └── NO → SONNET (default)
```

---

## Quick Reference Card

| Scenario | Model |
|----------|-------|
| "Just do my daily work" | Sonnet |
| "I need to think through this carefully" | Opus |
| "Process 1000 simple items" | Haiku |
| "Design the system architecture" | Opus |
| "Implement this feature" | Sonnet |
| "Quick format/convert" | Haiku |
| "Debug this complex issue" | Opus |
| "Fix this straightforward bug" | Sonnet |
| "Hook evaluation" | Haiku (auto) |
| "Research phase of project" | Opus |
| "Implementation phase" | Sonnet |

---

*Default to Sonnet. Upgrade to Opus for complexity. Downgrade to Haiku for simplicity.*
