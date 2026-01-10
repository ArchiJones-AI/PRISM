---
type: quick-reference
purpose: Single-page cheatsheet for common Claude Code operations
last_updated: 2025-12
---

# Claude Code Quick Reference

## Essential Commands

| Command | Purpose |
|---------|---------|
| `/clear` | Reset context window |
| `/init` | Reload CLAUDE.md |
| `/model sonnet\|opus\|haiku` | Switch models |
| `/memory` | View loaded memory |
| `/agents` | Manage subagents |
| `/plugin` | Manage plugins |
| `/bug` | Report issue |

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Tab` | Toggle thinking |
| `Shift+Tab+Tab` | Toggle Plan Mode |
| `Escape` | Interrupt |
| `Double Escape` | Edit previous |
| `Ctrl+B` | Background task |
| `Ctrl+O` | Verbose mode |

---

## File Locations

| Type | Location |
|------|----------|
| Agents | `.claude/agents/*.md` |
| Skills | `.claude/skills/*/SKILL.md` |
| Commands | `.claude/commands/*.md` |
| Rules | `.claude/rules/*.md` |
| Memory | `CLAUDE.md` or `.claude/CLAUDE.md` |
| Hooks | `.claude/settings.json` |
| MCP | `.mcp.json` |
| Plugins | `.claude/plugins/` |

---

## Special Syntax

| Syntax | Purpose | Example |
|--------|---------|---------|
| `@` | Include file | `@src/auth.ts` |
| `!` | Run command | `!git diff` |
| `&` | Send to web | `& analyse this` |
| `$ARGUMENTS` | Command args | `/project:fix $ARGUMENTS` |

---

## Thinking Keywords

| Keyword | Effect (v2.0.0+) |
|---------|------------------|
| `ultrathink` | ✅ 32K tokens |
| `think hard` | ❌ Deprecated |
| `think harder` | ❌ Deprecated |
| `think` | ❌ Deprecated |

**Only `ultrathink` works since v2.0.0!**

---

## Model Quick Switch

```bash
/model sonnet   # Fast, cheap (default)
/model opus     # Deep analysis
/model haiku    # Fastest, cheapest
```

---

## Plugin Installation

```bash
# Add marketplace
/plugin marketplace add anthropics/claude-code

# Install plugin
/plugin install feature-dev
```

---

## Emergency Troubleshooting

| Problem | Solution |
|---------|----------|
| Context full | `/clear` |
| Claude confused | `/clear` then restart |
| Plugin error | `/plugin` → Errors tab |
| Agent not found | Check `.claude/agents/` |
| Skill not loading | Check YAML frontmatter |
| MCP failing | `claude --mcp-debug` |
| CLI not found | Check PATH |

---

## Component Quick Reference

| Need | Use |
|------|-----|
| Distribute setup | Plugin |
| User-triggered | Command |
| Isolated task | Subagent |
| Auto-knowledge | Skill |
| Automation | Hook |
| External API | MCP |

---

## Workflow Pattern

```
1. EXPLORE (Plan Mode)
   - Research codebase
   - No changes
   
2. PLAN (Plan Mode)
   - Design approach
   - Save to file
   
3. CODE (Edit Mode)
   - Implement step by step
   - Test incrementally
   
4. COMMIT
   - Review changes
   - Write good message
```

---

## Session Best Practices

- Keep sessions under 2-3 hours
- Use `/clear` between tasks
- Save important state to files
- Use subagents for heavy research
- Default to Sonnet model

---

## CLI Flags

| Flag | Purpose |
|------|---------|
| `-p "prompt"` | Headless mode |
| `--model X` | Set model |
| `--debug` | Verbose logging |
| `--mcp-debug` | MCP logging |
| `--permission-mode plan` | Plan mode |
| `--output-format stream-json` | JSON output |

---

## Hook Events

| Event | Can Block? |
|-------|------------|
| PreToolUse | ✅ |
| PostToolUse | ❌ |
| Stop | ✅ |
| SubagentStart | ❌ |
| SubagentStop | ❌ |
| UserPromptSubmit | ❌ |
| SessionStart | ❌ |
| Notification | ❌ |
| PreCompact | ❌ |

---

## Quick Diagnostics

```bash
# Check installation
which claude

# Check API key
echo $ANTHROPIC_API_KEY | head -c 10

# Debug mode
claude --debug

# Report bug
/bug
```

---

*Print this page for quick reference!*
