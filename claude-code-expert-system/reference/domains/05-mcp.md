---
domain: mcp
semantic_triggers: MCP, Model Context Protocol, mcp server, mcp.json, claude mcp add, external tools, tool integration, mcp client, mcp configuration, mcp debug, puppeteer, github mcp, postgres mcp, slack mcp, figma mcp, filesystem mcp, memory mcp, mcp passthrough
priority_sources:
  - https://code.claude.com/docs/en/mcp
  - https://mcpez.com/claude-code
  - https://www.anthropic.com/engineering/claude-code-best-practices
related_domains:
  - 01-plugins.md
  - 04-hooks.md
  - 15-troubleshooting.md
---

# MCP (Model Context Protocol) Reference

## Quick Answer

MCP (Model Context Protocol) connects Claude Code to **external tools and services** like databases, APIs, file systems, and third-party apps. Claude Code operates as both MCP **client** (connecting to servers) and **server** (exposing its tools). Configure MCP servers in `.mcp.json` or settings files, then Claude can use those tools natively.

## Key Concepts

### Architecture
```
┌─────────────────┐     ┌─────────────────┐
│  Claude Code    │────▶│  MCP Server     │
│  (MCP Client)   │     │  (e.g., GitHub) │
└─────────────────┘     └─────────────────┘
        │
        ▼
┌─────────────────┐
│  Other Clients  │
│  (MCP Client)   │
└─────────────────┘
        │
        ▼
┌─────────────────┐
│  Claude Code    │
│  (MCP Server)   │
└─────────────────┘
```

### Claude Code's Dual Role
| Role | Function |
|------|----------|
| **MCP Client** | Connects to external MCP servers (GitHub, Postgres, Slack, etc.) |
| **MCP Server** | Exposes Claude Code's tools to other MCP clients |

**Note:** When Claude Code runs as MCP server, it does NOT pass through its client connections (no MCP passthrough).

### Configuration Locations

| Location | Scope | When to Use |
|----------|-------|-------------|
| `.mcp.json` | Project (shared) | Check into git, team-wide MCP servers |
| `.claude/settings.local.json` | Project (personal) | Your personal project MCP servers |
| `~/.claude/settings.local.json` | User-wide | MCP servers for all projects |

### Common MCP Servers

| Server | Purpose | Install Command |
|--------|---------|-----------------|
| **filesystem** | Read/write files outside project | `claude mcp add filesystem -s user -- npx -y @anthropic-ai/mcp-server-filesystem /path` |
| **puppeteer** | Browser automation, screenshots | `claude mcp add puppeteer -s user -- npx -y @anthropic-ai/mcp-server-puppeteer` |
| **github** | GitHub API integration | `claude mcp add github -s user -- npx -y @anthropic-ai/mcp-server-github` |
| **postgres** | PostgreSQL database access | `claude mcp add postgres -s local -- npx -y @anthropic-ai/mcp-server-postgres $DATABASE_URL` |
| **slack** | Slack messaging | `claude mcp add slack -s user -- npx -y @anthropic-ai/mcp-server-slack` |
| **figma** | Figma design access | `claude mcp add figma -s user -- npx -y @anthropic-ai/mcp-server-figma` |
| **memory** | Persistent key-value store | `claude mcp add memory -s user -- npx -y @anthropic-ai/mcp-server-memory` |
| **google-drive** | Google Drive files | `claude mcp add gdrive -s user -- npx -y @anthropic-ai/mcp-server-google-drive` |

## Authoritative Sources

| Source | Type | URL |
|--------|------|-----|
| MCP Documentation | Official Docs | https://code.claude.com/docs/en/mcp |
| MCP Server Directory | Community | https://mcpez.com/claude-code |
| Best Practices | Blog | https://www.anthropic.com/engineering/claude-code-best-practices |
| MCP Protocol Spec | GitHub | https://github.com/anthropics/mcp |

## Common Patterns

### Adding GitHub MCP Server
```bash
# User-wide installation
claude mcp add github -s user -- npx -y @anthropic-ai/mcp-server-github

# Set GitHub token (required)
export GITHUB_TOKEN=ghp_your_token_here

# Or add to .env
echo "GITHUB_TOKEN=ghp_your_token_here" >> ~/.env
```

### Adding PostgreSQL for a Project
```bash
# Local scope (this project only)
claude mcp add postgres -s local -- npx -y @anthropic-ai/mcp-server-postgres "postgresql://user:pass@localhost:5432/mydb"
```

### Shared Team Configuration (.mcp.json)
```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-postgres"],
      "env": {
        "DATABASE_URL": "${DATABASE_URL}"
      }
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

### Debugging MCP Connections
```bash
# Start Claude Code with MCP debug mode
claude --mcp-debug

# This shows:
# - Which servers are starting
# - Connection errors
# - Tool registration
# - Message exchanges
```

### Custom MCP Server (Python)
```python
# my-mcp-server.py
from mcp import MCPServer, tool

server = MCPServer("my-custom-server")

@tool
def get_weather(city: str) -> str:
    """Get current weather for a city."""
    # Your implementation
    return f"Weather in {city}: Sunny, 22°C"

@tool  
def search_internal_docs(query: str) -> str:
    """Search internal documentation."""
    # Your implementation
    return f"Results for '{query}': ..."

if __name__ == "__main__":
    server.run()
```

```bash
# Add custom server
claude mcp add my-tools -s local -- python my-mcp-server.py
```

### MCP in Plugins
Plugins can include MCP configurations:
```
my-plugin/
├── .claude-plugin/
│   └── plugin.json
└── .mcp.json          # Plugin's MCP servers
```

When plugin is enabled, its MCP servers auto-start.

## Prompt Templates

### Set Up Database Access
```
Set up MCP to connect to our PostgreSQL database:
- Host: localhost:5432
- Database: myapp_dev
- User: from $PGUSER env var
- Password: from $PGPASSWORD env var

Add it as a local scope MCP server so the whole team can use it.
Create the .mcp.json file to check into git.
```

### Configure Browser Automation
```
I need Claude Code to be able to:
1. Take screenshots of web pages
2. Fill out forms
3. Click buttons and navigate

Set up the Puppeteer MCP server with appropriate configuration.
```

### Debug MCP Issues
```
My GitHub MCP server isn't working. Help me debug:
1. Check if it's configured correctly
2. Verify the GITHUB_TOKEN is set
3. Run claude --mcp-debug to see connection logs
4. Test a simple GitHub API call
```

## Troubleshooting

### "MCP server not found"
```bash
# Check configuration
cat .mcp.json | jq .
cat ~/.claude/settings.local.json | jq '.mcpServers'

# Verify npx/uvx is available
which npx
which uvx
```

### "Executable not found"
```bash
# Install the MCP server package globally
npm install -g @anthropic-ai/mcp-server-github

# Or use npx (downloads on demand)
npx -y @anthropic-ai/mcp-server-github --help
```

### MCP Server Won't Start
```bash
# Debug mode
claude --mcp-debug

# Common issues:
# 1. Missing environment variables
# 2. Wrong command path
# 3. Permission issues
# 4. Port conflicts
```

### Authentication Failures
```bash
# GitHub: Check token
echo $GITHUB_TOKEN

# Postgres: Test connection
psql $DATABASE_URL -c "SELECT 1"

# Slack: Verify bot token
curl -H "Authorization: Bearer $SLACK_BOT_TOKEN" https://slack.com/api/auth.test
```

### Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| "Server not responding" | Server crashed or timeout | Check logs with `--mcp-debug` |
| "Tool not found" | Server didn't register tools | Verify server implementation |
| "Permission denied" | Auth token invalid/expired | Refresh credentials |
| "Connection refused" | Server not running | Check server process |
| "Invalid configuration" | JSON syntax error | Validate .mcp.json |

### Environment Variables for MCP
```bash
# GitHub
export GITHUB_TOKEN=ghp_xxxxx

# PostgreSQL
export DATABASE_URL=postgresql://user:pass@host:5432/db

# Slack
export SLACK_BOT_TOKEN=xoxb-xxxxx

# Figma
export FIGMA_ACCESS_TOKEN=xxxxx

# Google (OAuth)
export GOOGLE_CLIENT_ID=xxxxx
export GOOGLE_CLIENT_SECRET=xxxxx
```

## MCP Scopes

| Scope | Flag | Location | Use Case |
|-------|------|----------|----------|
| **user** | `-s user` | `~/.claude/settings.local.json` | Personal tools for all projects |
| **local** | `-s local` | `.claude/settings.local.json` | Project-specific, not shared |
| **project** | (default in .mcp.json) | `.mcp.json` | Team-shared, checked into git |

## Security Considerations

- **Never commit secrets** to `.mcp.json` — use environment variables
- **Limit MCP scope** — use local/user scope for sensitive servers
- **Audit tool access** — MCP servers can read/write data
- **Use read-only credentials** where possible

## Related Domains

- **[Plugins](01-plugins.md)** — Plugins can include MCP configurations
- **[Hooks](04-hooks.md)** — Use hooks to audit MCP tool usage
- **[Troubleshooting](15-troubleshooting.md)** — MCP debugging techniques
- **[Headless](08-headless.md)** — MCP in CI/CD environments
