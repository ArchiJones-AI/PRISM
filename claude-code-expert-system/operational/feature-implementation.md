---
domain: feature-implementation
type: operational
semantic_triggers: create plugin, build plugin, develop, implement, new plugin,
  add skill, add hook, make extension, build feature, step by step
cross_layer_refs:
  - reference/domains/01-plugins.md
  - reference/domains/03-skills.md
  - reference/domains/04-hooks.md
  - reference/domains/05-mcp.md
related_operational:
  - problem-diagnosis.md
last_updated: 2025-01
---

# Feature Implementation — Claude Code Expert System

How do I create a new plugin? How do I add a skill or hook? This document provides step-by-step workflows for implementing Claude Code extension features including plugins, skills, hooks, and MCP integrations.

Key terms: create, build, implement, develop, plugin, skill, hook, MCP, step-by-step, workflow

---

## Plugin Development Workflow

### Purpose

Guide users through creating a complete Claude Code plugin from scratch.

### When to Use

Use this workflow when:
- Building a new plugin for distribution
- Bundling multiple skills, hooks, or commands together
- Creating shareable functionality for the marketplace

### Process Steps

**Step 1: Create Plugin Directory Structure**

Create the plugin directory with required files. A minimal Claude Code plugin needs:

```
my-plugin/
├── plugin.json          # Plugin manifest (required)
├── skills/              # Skill definitions (optional)
├── hooks/               # Hook implementations (optional)
└── commands/            # Custom commands (optional)
```

Run: `mkdir -p my-plugin/{skills,hooks,commands}`

**Step 2: Create Plugin Manifest**

Create `plugin.json` with required metadata. Every Claude Code plugin must have a manifest:

```json
{
  "name": "my-plugin",
  "version": "1.0.0",
  "description": "What this plugin does",
  "skills": ["skills/*.md"],
  "hooks": ["hooks/*.sh"]
}
```

**Step 3: Implement Components**

Add skills, hooks, or commands based on your plugin's purpose:

| Component | When to Add | File Location |
|-----------|-------------|---------------|
| Skills | User-invocable capabilities | skills/*.md |
| Hooks | Automated lifecycle actions | hooks/*.sh |
| Commands | CLI operations | commands/*.md |

See reference/domains/03-skills.md and reference/domains/04-hooks.md for component details.

**Step 4: Test Locally**

Test the plugin before publishing. Install locally with:

```bash
claude /plugin install ./my-plugin
```

Verify all components work as expected.

**Step 5: Publish (Optional)**

To share on the marketplace, follow publishing guidelines in reference/domains/01-plugins.md.

### Output

Completed plugin ready for local use or marketplace distribution.

---

## Skill Implementation Workflow

### Purpose

Create a new skill that Claude can invoke during conversations.

### Process Steps

**Step 1: Define Skill Purpose**

Determine what the skill should do and when it should activate. Skills are for repeatable, user-invocable capabilities.

**Step 2: Create Skill File**

Create a markdown file in your skills directory:

```markdown
# skill-name

[Description of what this skill does]

## Triggers

- User asks to [action]
- User mentions [keywords]

## Instructions

[What Claude should do when this skill activates]
```

**Step 3: Register in Plugin**

Add to plugin.json skills array or place in ~/.claude/skills/ for global availability.

**Step 4: Test Activation**

Test that the skill activates on expected triggers and performs correctly.

---

## Hook Implementation Workflow

### Purpose

Create automated actions that run at specific lifecycle events.

### Process Steps

**Step 1: Choose Hook Type**

Select the lifecycle event for your hook:

| Hook Type | Fires When |
|-----------|------------|
| PreToolUse | Before tool execution |
| PostToolUse | After tool execution |
| Stop | On conversation end |

**Step 2: Create Hook Script**

Create executable script in hooks directory. The hook receives context via environment variables.

**Step 3: Configure Trigger**

Set the hook to fire on specific tools or events in plugin.json.

**Step 4: Test Thoroughly**

Hooks run automatically—test edge cases and error handling carefully.

---

## Related Operational Knowledge

- **problem-diagnosis.md** — When implementation doesn't work as expected
- **workflow-execution.md** — Integrating with plan-code-commit workflows

## Related Reference Knowledge

- **reference/domains/01-plugins.md** — Plugin concepts and marketplace
- **reference/domains/03-skills.md** — Skill definition syntax
- **reference/domains/04-hooks.md** — Hook types and configuration
- **reference/domains/05-mcp.md** — MCP server integration
