---
domain: troubleshooting
semantic_triggers: error, bug, issue, not working, problem, troubleshoot, debug, fix, help, broken, failure, CLINotFoundError, permission error, mcp connection, plugin error, skill not loading, crash, hang, slow
priority_sources:
  - https://code.claude.com/docs/en/troubleshooting
  - https://claudelog.com/
related_domains:
  - 05-mcp.md
  - 01-plugins.md
  - 03-skills.md
  - 04-hooks.md
---

# Troubleshooting Reference

## Quick Answer

Use `/bug` to report issues to Anthropic, `claude --debug` for verbose logging, and `/plugin` Errors tab for plugin diagnostics. Most issues are: PATH problems (CLINotFoundError), YAML syntax (skills/agents not loading), permissions (chmod +x), or context overflow (/clear). Check this guide before searching external sources.

## Diagnostic Commands

| Command | Purpose | When to Use |
|---------|---------|-------------|
| `/bug` | Report to Anthropic | Reproducible bugs |
| `claude --debug` | Verbose logging | Connection issues, startup problems |
| `claude --mcp-debug` | MCP verbose logs | MCP server issues |
| `/plugin` → Errors | Plugin diagnostics | Plugin failures |
| `/memory` | View loaded context | Memory issues |
| `/agents` | List agents | Agent not found |

## Common Errors & Solutions

### CLINotFoundError

**Symptoms:**
```
CLINotFoundError: Claude CLI not found
Command 'claude' not found
```

**Causes:**
- Claude Code not in PATH
- Installation incomplete
- Shell not reloaded after install

**Solutions:**
```bash
# Check if installed
which claude

# If not found, add to PATH
export PATH="$HOME/.local/bin:$PATH"

# Add to shell profile permanently
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# Reinstall if needed
npm install -g @anthropic-ai/claude-code
```

---

### MCP "Executable not found"

**Symptoms:**
```
MCP server failed to start
Error: spawn npx ENOENT
Executable not found: uvx
```

**Causes:**
- Required binary not installed
- Wrong command in MCP config
- Environment not set up

**Solutions:**
```bash
# Install Node.js (for npx)
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs

# Install Python uv (for uvx)
curl -LsSf https://astral.sh/uv/install.sh | sh

# Verify installation
which npx
which uvx

# Debug MCP
claude --mcp-debug
```

---

### Plugin Loading Errors

**Symptoms:**
```
Plugin failed to load
Invalid plugin.json
Plugin not found
```

**Diagnosis:**
```bash
# Check errors tab
/plugin
# Navigate to "Errors"

# Verify plugin structure
ls -la .claude-plugin/
cat .claude-plugin/plugin.json | jq .

# Check JSON syntax
python -m json.tool .claude-plugin/plugin.json
```

**Solutions:**
```bash
# Fix JSON syntax errors
# Ensure proper structure:
{
  "name": "plugin-name",
  "version": "1.0.0",
  "description": "Description"
}

# Reinstall plugin
/plugin uninstall {name}
/plugin install {name}
```

---

### Skill Not Loading

**Symptoms:**
```
Skill doesn't auto-invoke
SKILL.md ignored
Skill not found
```

**Diagnosis:**
```bash
# Check file location (MUST be SKILL.md, capital)
ls -la .claude/skills/my-skill/SKILL.md

# Check frontmatter syntax
head -20 .claude/skills/my-skill/SKILL.md

# Verify YAML is valid
cat .claude/skills/my-skill/SKILL.md | head -20
```

**Solutions:**
```bash
# Correct structure:
.claude/skills/my-skill/SKILL.md  # Capital SKILL.md

# Correct frontmatter:
---
name: my-skill
description: What this skill does
---

# Restart session after creating skill
```

---

### Agent Not Found

**Symptoms:**
```
Agent "x" not found
/agents shows nothing
Task tool can't find agent
```

**Diagnosis:**
```bash
# Check file location
ls -la .claude/agents/
ls -la ~/.claude/agents/

# Check frontmatter
cat .claude/agents/my-agent.md | head -20
```

**Solutions:**
```bash
# Correct location:
.claude/agents/my-agent.md   # Project scope
~/.claude/agents/my-agent.md # User scope

# Correct frontmatter:
---
name: my-agent
description: What this agent does
tools: Read, Grep, Glob
---
```

---

### Permission Errors

**Symptoms:**
```
Permission denied
EACCES
Cannot execute
```

**Solutions:**
```bash
# Make scripts executable
chmod +x .claude/hooks/my-hook.sh
chmod +x .claude/skills/my-skill/scripts/*.py

# Fix directory permissions
chmod -R 755 .claude/

# For system directories
sudo chown -R $USER ~/.claude/
```

---

### Hook Not Firing

**Symptoms:**
```
Hook defined but never runs
PreToolUse hook ignored
```

**Diagnosis:**
```bash
# Check hook configuration
cat .claude/settings.json | jq '.hooks'

# Test hook script manually
TOOL_NAME=Bash TOOL_INPUT="ls" ./my-hook.sh
echo $?
```

**Solutions:**
```bash
# Verify event name (case-sensitive)
# Valid: PreToolUse, PostToolUse, Stop, SubagentStart, 
#        SubagentStop, UserPromptSubmit, SessionStart, 
#        Notification, PreCompact

# Check matcher pattern
# Exact: "Bash"
# Wildcard: "*"
# Regex: "/^(Write|Edit)$/"

# Ensure exit codes are correct
# 0 = allow, 2 = block
```

---

### Context Window Full

**Symptoms:**
```
Response truncated
Claude seems to forget earlier context
Compaction happening frequently
```

**Solutions:**
```bash
# Clear context
/clear

# Start fresh session
exit
claude

# Use shorter CLAUDE.md (50-250 lines ideal)
wc -l CLAUDE.md

# Use subagents for heavy research
"Use a subagent to research this, keeping main context clean"

# Session management
# Keep sessions under 2-3 hours
```

---

### Authentication Errors

**Symptoms:**
```
Invalid API key
Authentication failed
401 Unauthorized
```

**Solutions:**
```bash
# Check API key is set
echo $ANTHROPIC_API_KEY | head -c 10

# Set API key
export ANTHROPIC_API_KEY=sk-ant-...

# Add to profile
echo 'export ANTHROPIC_API_KEY=sk-ant-...' >> ~/.bashrc

# For Bedrock/Vertex, check provider config
echo $CLAUDE_CODE_USE_BEDROCK
echo $CLAUDE_CODE_USE_VERTEX
```

---

### Slow Performance

**Symptoms:**
```
Responses very slow
Long pauses
Timeout errors
```

**Causes & Solutions:**
```bash
# 1. Extended thinking enabled unnecessarily
# Only use ultrathink when needed
# Check Tab key didn't enable thinking

# 2. Large context
/clear  # Reset context

# 3. Network issues
ping api.anthropic.com

# 4. Rate limiting
# Wait and retry
# Check usage at console.anthropic.com
```

---

### YAML/Frontmatter Errors

**Symptoms:**
```
Invalid frontmatter
YAML parse error
Agent/skill ignored
```

**Common mistakes:**
```yaml
# ❌ Wrong: Missing --- delimiters
name: my-agent
description: Does stuff

# ✅ Correct: With delimiters
---
name: my-agent
description: Does stuff
---

# ❌ Wrong: Bad indentation
---
name: my-agent
  description: Does stuff   # Wrong indent
---

# ✅ Correct: Consistent indentation
---
name: my-agent
description: Does stuff
---

# ❌ Wrong: Special characters unquoted
---
description: Uses @mentions and #tags
---

# ✅ Correct: Quote special characters
---
description: "Uses @mentions and #tags"
---
```

## Authoritative Sources

| Source | Type | URL |
|--------|------|-----|
| Troubleshooting Guide | Official Docs | https://code.claude.com/docs/en/troubleshooting |
| Claudelog FAQ | Community | https://claudelog.com/ |

## Environment Variables Reference

| Variable | Purpose | Example |
|----------|---------|---------|
| `ANTHROPIC_API_KEY` | API authentication | `sk-ant-...` |
| `CLAUDE_CODE_USE_BEDROCK` | Use AWS Bedrock | `1` |
| `CLAUDE_CODE_USE_VERTEX` | Use Google Vertex | `1` |
| `CLAUDE_CODE_USE_FOUNDRY` | Use Azure Foundry | `1` |
| `MAX_THINKING_TOKENS` | Thinking budget | `32000` |

## Log File Locations

| Log | Location | Contains |
|-----|----------|----------|
| Session log | `~/.claude/logs/` | Conversation history |
| Debug log | (with --debug) | Verbose output |
| MCP log | (with --mcp-debug) | MCP traffic |

## Reporting Bugs

### Using /bug Command
```
/bug
# Opens bug report form
# Includes session context automatically
```

### GitHub Issues
- URL: https://github.com/anthropics/claude-code/issues
- Include: Version, OS, steps to reproduce, error messages

### Discord Community
- Join: Claude Developers Discord
- Get help from community
- Share workarounds

## Quick Diagnostic Checklist

```
□ Is claude in PATH? (which claude)
□ Is API key set? (echo $ANTHROPIC_API_KEY | head -c 10)
□ Are files in correct location? (.claude/agents/, .claude/skills/)
□ Is YAML syntax valid? (check --- delimiters)
□ Are scripts executable? (chmod +x)
□ Is context fresh? (/clear)
□ Are plugins enabled? (/plugin list)
□ Is the right model selected? (/model)
```

## Related Domains

- **[MCP](05-mcp.md)** — MCP connection issues
- **[Plugins](01-plugins.md)** — Plugin errors
- **[Skills](03-skills.md)** — Skill loading issues
- **[Hooks](04-hooks.md)** — Hook debugging
- **[Memory](06-memory.md)** — Context issues
