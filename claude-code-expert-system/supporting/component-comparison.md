---
type: cross-cutting
purpose: Decision guide for choosing between Claude Code extension mechanisms
last_updated: 2025-12
---

# Component Comparison — When to Use What

## Quick Decision Table

| Need | Use | Why |
|------|-----|-----|
| Distribute complete setup | **Plugin** | Bundle everything together |
| User-triggered workflow | **Command** | Explicit /command invocation |
| Parallel/isolated task | **Subagent** | Separate context window |
| Auto-invoked knowledge | **Skill** | Model decides when to use |
| Deterministic automation | **Hook** | Always runs at lifecycle event |
| External tool integration | **MCP** | Connect to databases, APIs, etc. |

---

## Comprehensive Comparison

| Aspect | Plugin | Command | Subagent | Skill | Hook | MCP |
|--------|--------|---------|----------|-------|------|-----|
| **Trigger** | Container | `/command` | Task tool / manual | Auto (model) | Lifecycle event | Tool call |
| **Context** | N/A | Same | Isolated | Same | N/A | N/A |
| **Purpose** | Distribution | Workflow | Delegation | Knowledge | Automation | Integration |
| **Persistence** | Installed | Session | Task duration | Session | Always | Session |
| **User control** | Enable/disable | Explicit call | Explicit call | None (auto) | None (auto) | Configure |
| **Complexity** | High | Low | Medium | Medium | Medium | High |

---

## Decision Flowchart

```
START: What do you need?
│
├─► Need to share setup with team?
│   └─► YES → Use PLUGIN (bundles everything)
│
├─► User should explicitly trigger?
│   └─► YES → Use COMMAND (/project:my-command)
│
├─► Need isolated context / parallel work?
│   └─► YES → Use SUBAGENT (.claude/agents/)
│
├─► Claude should "just know" this?
│   └─► YES → Use SKILL (.claude/skills/)
│
├─► Need deterministic automation?
│   └─► YES → Use HOOK (PreToolUse, PostToolUse, etc.)
│
├─► Need external service connection?
│   └─► YES → Use MCP (database, API, etc.)
│
└─► Not sure?
    └─► Start with COMMAND, evolve if needed
```

---

## Detailed Use Cases

### Use a PLUGIN when...

✅ **Do use for:**
- Distributing team configurations
- Packaging related commands + agents + skills + hooks
- Sharing with the community
- Complex setups that need multiple components

❌ **Don't use for:**
- Single, simple commands
- Personal one-off automation
- Quick experiments

**Example:** A "full-stack dev" plugin with code-explorer agent, frontend-design skill, lint hooks, and GitHub MCP.

---

### Use a COMMAND when...

✅ **Do use for:**
- Workflows you trigger explicitly
- Multi-step processes with known inputs
- Tasks that need `$ARGUMENTS`
- Operations you want to run on demand

❌ **Don't use for:**
- Things that should happen automatically
- Knowledge Claude should always have
- Parallel or isolated work

**Example:** `/project:fix-github-issue 1234` — explicitly fix a specific issue.

---

### Use a SUBAGENT when...

✅ **Do use for:**
- Heavy research that would pollute main context
- Parallel independent tasks
- Tasks needing different model (Opus for planning)
- Isolated work with specific tools
- Delegation of complete sub-tasks

❌ **Don't use for:**
- Quick lookups (just use main Claude)
- Tasks needing main conversation context
- Simple file reads

**Example:** A `code-reviewer` agent that reviews PRs in isolation, returns summary.

---

### Use a SKILL when...

✅ **Do use for:**
- Knowledge Claude should apply automatically
- Patterns/conventions for specific file types
- Best practices that should "just work"
- Documentation Claude needs on-demand

❌ **Don't use for:**
- User-triggered workflows (use Command)
- Tasks needing isolation (use Subagent)
- Deterministic automation (use Hook)

**Example:** A `documentation` skill that auto-activates when writing READMEs.

---

### Use a HOOK when...

✅ **Do use for:**
- Deterministic automation (always happens)
- Blocking dangerous operations
- Auto-formatting on save
- Notifications/logging
- Validation before actions

❌ **Don't use for:**
- Conditional logic based on content (use Skill)
- Interactive workflows (use Command)
- Anything needing conversation context

**Example:** `PreToolUse` hook that blocks `rm -rf /` commands.

---

### Use MCP when...

✅ **Do use for:**
- Connecting to databases
- External API integration
- Third-party services (Slack, GitHub, Figma)
- Custom tools with complex logic

❌ **Don't use for:**
- Simple file operations (built-in tools)
- Things Claude can do natively
- Local-only functionality

**Example:** PostgreSQL MCP server for database queries.

---

## Common Misconceptions

### "Skills are just fancy CLAUDE.md sections"

**Wrong.** Skills use progressive disclosure (only loaded when relevant), can include scripts, and auto-invoke. CLAUDE.md is always loaded upfront.

| Aspect | CLAUDE.md | Skill |
|--------|-----------|-------|
| Loading | Always (startup) | On-demand |
| Size impact | Direct context cost | Minimal until invoked |
| Scripts | No | Yes (scripts/ directory) |
| Trigger | None (passive) | Auto-invoked by model |

---

### "Subagents are just prompts in files"

**Wrong.** Subagents run in completely isolated context windows, can use different models, and don't see the main conversation.

| Aspect | Command | Subagent |
|--------|---------|----------|
| Context | Same conversation | Isolated |
| Model | Same as main | Can be different |
| Parallelism | No | Yes (Ctrl+B) |
| Tool access | Inherits | Explicitly defined |

---

### "Hooks are like skills for automation"

**Wrong.** Hooks are deterministic (always fire at events). Skills are AI-decided (model chooses when relevant).

| Aspect | Hook | Skill |
|--------|------|-------|
| Trigger | Deterministic (lifecycle) | AI-decided |
| Execution | Shell command or prompt | Context injection |
| Can block | Yes (exit code 2) | No |
| Purpose | Automation | Knowledge |

---

## Migration Patterns

### Command → Skill

When your command should auto-invoke:

```markdown
# Before: .claude/commands/doc-style.md
Apply our documentation style guidelines...

# After: .claude/skills/doc-style/SKILL.md
---
name: doc-style
description: Applies documentation style guidelines when writing docs
---
# Documentation Style
[Same content, now auto-invokes]
```

### Command → Plugin

When you want to distribute your command:

```bash
# Before: .claude/commands/deploy.md

# After: Plugin structure
my-deploy-plugin/
└── .claude-plugin/
    ├── plugin.json
    └── commands/
        └── deploy.md
```

### Multiple Commands → Subagent

When commands should run in isolation:

```markdown
# Before: Multiple related commands

# After: .claude/agents/code-quality.md
---
name: code-quality
description: Runs all code quality checks
tools: Bash, Read, Grep
---
Run lint, tests, and security checks.
Report consolidated results.
```

---

## Component Hierarchy

```
PLUGIN (container)
├── COMMANDS (user-triggered workflows)
├── SUBAGENTS (isolated AI tasks)
├── SKILLS (auto-invoked knowledge)
├── HOOKS (deterministic automation)
└── MCP (external integrations)
```

A plugin can contain any combination of the other components.

---

## Quick Reference

| I want to... | Use |
|--------------|-----|
| Share my setup with the team | Plugin |
| Create a reusable workflow | Command |
| Delegate a task with fresh context | Subagent |
| Add knowledge Claude uses automatically | Skill |
| Auto-format code on every save | Hook |
| Connect to my database | MCP |
| Block dangerous commands | Hook |
| Run parallel code reviews | Subagents |
| Apply coding standards automatically | Skill |
| Create a /deploy command | Command |

---

*When in doubt, start simple (Command), then evolve based on needs.*
