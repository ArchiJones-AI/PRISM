---
domain: subagents
semantic_triggers: sub-agent, subagent, agent, agents, .claude/agents/, Task tool, parallel agents, agent delegation, spawn agent, background agent, custom agent, plan subagent, explore subagent, agent context, agent isolation, async agent, /agents, verify agent, agent frontmatter, agent model, agent tools
priority_sources:
  - https://code.claude.com/docs/en/sub-agents
  - https://www.anthropic.com/engineering/claude-code-best-practices
  - https://github.com/anthropics/claude-code/tree/main/plugins
related_domains:
  - 03-skills.md
  - 01-plugins.md
  - 04-hooks.md
  - 10-agent-sdk.md
---

# Subagents Reference

## Quick Answer

Subagents are **specialised AI agents that run in isolated context windows**. They're perfect for parallel processing, delegating specific tasks, or when you need fresh context without polluting the main conversation. Define them as Markdown files in `.claude/agents/` with YAML frontmatter specifying tools, model, and permissions. Invoke manually or let Claude spawn them via the Task tool.

## Key Concepts

### Core Architecture
- **Context Isolation** — Each subagent has its own context window, separate from main conversation
- **Tool Inheritance** — By default, subagents can't use tools; specify allowed tools explicitly
- **Model Selection** — Can use different models (sonnet, opus, haiku) per agent
- **No Nesting** — Subagents cannot spawn other subagents (prevents infinite recursion)

### File Structure
```
.claude/
└── agents/
    ├── code-reviewer.md      # Project-specific agent
    ├── security-auditor.md   # Another project agent
    └── ...

~/.claude/
└── agents/
    ├── general-helper.md     # Global agent (all projects)
    └── ...
```

### Agent Definition Format
```markdown
---
name: code-reviewer
description: Reviews code for bugs, style issues, and best practices
tools: Read, Grep, Glob, Bash
model: sonnet
permissionMode: default
skills: code-review, security-guidance
---

You are a meticulous code reviewer. Your job is to:

1. Identify bugs and logic errors
2. Check for security vulnerabilities
3. Suggest performance improvements
4. Ensure code follows project conventions

Be thorough but constructive. Explain WHY something is problematic, not just WHAT.
```

### YAML Frontmatter Schema

| Field | Required | Values | Description |
|-------|----------|--------|-------------|
| `name` | Yes | string | Agent identifier |
| `description` | Yes | string | What the agent does (shown in selection UI) |
| `tools` | No | comma-separated | Allowed tools: Read, Write, Edit, Bash, Glob, Grep, etc. |
| `model` | No | sonnet, opus, haiku, inherit | Which model to use (default: inherit) |
| `permissionMode` | No | default, plan, bypassPermissions | Permission level |
| `skills` | No | comma-separated | Skills to load for this agent |

### Built-in Agents

| Agent | Purpose | When to Use |
|-------|---------|-------------|
| **Plan** | Research and planning without modifications | Architecture decisions, codebase exploration |
| **Explore** | Deep codebase analysis | Understanding unfamiliar code |
| **Verify** | Validation and testing | Confirming changes work correctly |

### Execution Modes

| Mode | How | Behaviour |
|------|-----|-----------|
| **Foreground** | Normal invocation | Blocks main conversation, shows output |
| **Background** | `Ctrl+B` | Runs async, returns when complete |
| **Parallel** | Multiple Task tool calls | Multiple agents run simultaneously |

## Authoritative Sources

| Source | Type | URL |
|--------|------|-----|
| Sub-agents Documentation | Official Docs | https://code.claude.com/docs/en/sub-agents |
| Best Practices | Engineering Blog | https://www.anthropic.com/engineering/claude-code-best-practices |
| Official Plugins (examples) | GitHub | https://github.com/anthropics/claude-code/tree/main/plugins |
| Full Stack Explanation | Community | https://alexop.dev/posts/understanding-claude-code-full-stack/ |

## Common Patterns

### Creating a Code Reviewer Agent
```markdown
# Save to: .claude/agents/code-reviewer.md
---
name: code-reviewer
description: Reviews code changes for bugs, security issues, and style
tools: Read, Grep, Glob, Bash
model: sonnet
---

You are an expert code reviewer. For each review:

1. **Security**: Check for injection, auth issues, data exposure
2. **Bugs**: Look for null refs, off-by-one, race conditions
3. **Performance**: Identify N+1 queries, unnecessary loops
4. **Style**: Ensure consistency with project conventions

Format your review as:
- 🔴 Critical: Must fix before merge
- 🟡 Warning: Should fix, not blocking
- 🟢 Suggestion: Nice to have

End with an overall assessment: APPROVE, REQUEST_CHANGES, or NEEDS_DISCUSSION.
```

### Creating a Documentation Agent
```markdown
# Save to: .claude/agents/doc-writer.md
---
name: doc-writer
description: Writes and updates documentation based on code changes
tools: Read, Write, Glob
model: sonnet
skills: docx
---

You are a technical writer. When asked to document code:

1. Read the relevant source files
2. Understand the public API and key concepts
3. Write clear, example-rich documentation
4. Update README.md and any relevant docs/

Use the project's existing documentation style. Include:
- Quick start examples
- API reference
- Common use cases
- Troubleshooting section
```

### Parallel Agent Workflow
```
I need to review this PR thoroughly. Please spawn three parallel agents:

1. **Security Agent**: Check for vulnerabilities (use security-auditor)
2. **Performance Agent**: Profile and identify bottlenecks
3. **Style Agent**: Ensure code follows our conventions

Consolidate their findings into a single review.
```

### Agent with Custom Model
```markdown
# Save to: .claude/agents/architect.md
---
name: architect
description: Makes high-level architectural decisions using Opus
tools: Read, Grep, Glob
model: opus
permissionMode: plan
---

You are a senior software architect. You:

1. Analyse system-wide implications of changes
2. Consider scalability, maintainability, security
3. Reference established patterns (SOLID, DDD, etc.)
4. Provide concrete recommendations with trade-offs

Think deeply before responding. Quality over speed.
```

## Prompt Templates

### Create Custom Agent
```
Create a subagent called "test-writer" that:
1. Reads source files to understand functionality
2. Generates comprehensive unit tests
3. Uses Jest/pytest conventions based on project
4. Achieves high coverage of edge cases

Save it to .claude/agents/test-writer.md with appropriate tools and model.
```

### Use Agent for Task
```
Use the code-reviewer agent to review the changes in src/auth/.
Focus particularly on security implications.
```

### Parallel Agent Execution
```
I need a comprehensive analysis of this codebase. Run these agents in parallel:
1. Explore agent: Map the architecture
2. Security agent: Identify vulnerabilities  
3. Performance agent: Find bottlenecks

Synthesise findings when all complete.
```

## Troubleshooting

### Agent Not Found
```bash
# Check agent files exist
ls -la .claude/agents/
ls -la ~/.claude/agents/

# View available agents
/agents
```

### Agent Can't Use Tools
- Ensure tools are listed in frontmatter: `tools: Read, Write, Bash`
- Check tool names are exact (case-sensitive)
- Verify `permissionMode` allows tool usage

### Agent Context Issues
- Subagents don't see main conversation context
- Pass necessary information explicitly in the prompt
- Use skills to provide persistent knowledge

### Agent Running Wrong Model
- Check `model:` field in frontmatter
- Valid values: `sonnet`, `opus`, `haiku`, `inherit`
- `inherit` uses the main conversation's model

### Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| "Agent not found" | File not in correct location | Check `.claude/agents/` path |
| "Invalid frontmatter" | YAML syntax error | Validate YAML structure |
| "Tool not available" | Tool not in allowed list | Add to `tools:` frontmatter |
| "Permission denied" | permissionMode restriction | Adjust `permissionMode:` or approve manually |

## Hooks Integration

Subagents trigger lifecycle hooks:
- **SubagentStart** — Fires when agent is spawned
- **SubagentStop** — Fires when agent completes

```json
{
  "hooks": [
    {
      "event": "SubagentStop",
      "type": "command",
      "command": "echo 'Agent $AGENT_NAME completed' >> agent.log"
    }
  ]
}
```

## Related Domains

- **[Skills](03-skills.md)** — Load skills into agents via `skills:` frontmatter
- **[Plugins](01-plugins.md)** — Plugins can bundle custom agents
- **[Hooks](04-hooks.md)** — SubagentStart/SubagentStop events
- **[Agent SDK](10-agent-sdk.md)** — Build agents programmatically
- **[Workflows](13-workflows.md)** — Using agents in development workflows
