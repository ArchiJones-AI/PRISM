---
domain: plugins
semantic_triggers: plugins, plugin, /plugin, install plugin, plugin marketplace, plugin structure, .claude-plugin, plugin.json, marketplace add, plugin install, custom plugin, share plugin, plugin commands, plugin development, plugin creation, plugin scope, user plugin, local plugin
priority_sources:
  - https://code.claude.com/docs/en/plugins
  - https://code.claude.com/docs/en/discover-plugins
  - https://github.com/anthropics/claude-code/blob/main/plugins/README.md
  - https://github.com/anthropics/claude-code/tree/main/plugins
  - https://github.com/anthropics/claude-plugins-official
related_domains:
  - 02-subagents.md
  - 03-skills.md
  - 04-hooks.md
  - 05-mcp.md
---

# Plugins Reference

## Quick Answer

Plugins are **bundles that package multiple Claude Code extensions together**: slash commands, subagents, skills, hooks, and MCP servers. They're the distribution mechanism for sharing complete configurations across teams and projects. Install with `/plugin install {name}@{marketplace}` or `/plugin marketplace add {owner/repo}` to register a new source.

## Key Concepts

### What Plugins Contain
- **Slash Commands** — User-triggered workflows (`/command`)
- **Subagents** — Specialised AI agents for delegation
- **Skills** — Auto-invoked knowledge and capabilities
- **Hooks** — Lifecycle automation (PreToolUse, PostToolUse, etc.)
- **MCP Servers** — External tool integrations

### Plugin Structure
```
my-plugin/
├── .claude-plugin/
│   ├── plugin.json          # Plugin manifest (required)
│   ├── commands/            # Slash commands (*.md)
│   ├── agents/              # Subagent definitions (*.md)
│   ├── skills/              # Skill definitions (SKILL.md in subdirs)
│   └── hooks/               # Hook configurations
└── .mcp.json                # MCP server definitions (optional)
```

### plugin.json Schema
```json
{
  "name": "my-plugin",
  "version": "1.0.0",
  "description": "What this plugin does",
  "author": "Your Name",
  "repository": "https://github.com/you/my-plugin"
}
```

### Installation Scopes
| Scope | Location | Availability |
|-------|----------|--------------|
| **user** | `~/.claude/plugins/` | All projects |
| **local** | `./.claude/plugins/` | Current project only |

### Installation Methods
```bash
# From registered marketplace
/plugin install feature-dev@anthropics/claude-code

# Add new marketplace source
/plugin marketplace add owner/repo

# From git URL
/plugin install https://github.com/owner/plugin.git

# From local path
/plugin install /path/to/plugin
```

## Authoritative Sources

| Source | Type | URL |
|--------|------|-----|
| Official Plugins Docs | Documentation | https://code.claude.com/docs/en/plugins |
| Discover Plugins | Documentation | https://code.claude.com/docs/en/discover-plugins |
| Plugins README | GitHub | https://github.com/anthropics/claude-code/blob/main/plugins/README.md |
| Plugins Directory | GitHub | https://github.com/anthropics/claude-code/tree/main/plugins |
| Official Marketplace | GitHub | https://github.com/anthropics/claude-plugins-official |
| Community Registry | Community | https://claude-plugins.dev/ |

## Common Patterns

### Installing Official Anthropic Plugins
```bash
# First, add the official marketplace (one-time)
/plugin marketplace add anthropics/claude-code

# Then install specific plugins
/plugin install feature-dev
/plugin install frontend-design
/plugin install pr-review-toolkit
/plugin install commit-commands
/plugin install security-guidance
```

### Creating a Custom Plugin
```bash
# 1. Create plugin directory structure
mkdir -p my-plugin/.claude-plugin/{commands,agents,skills}

# 2. Create plugin.json
cat > my-plugin/.claude-plugin/plugin.json << 'EOF'
{
  "name": "my-plugin",
  "version": "1.0.0",
  "description": "My custom Claude Code plugin",
  "author": "Your Name"
}
EOF

# 3. Add a command
cat > my-plugin/.claude-plugin/commands/hello.md << 'EOF'
Say hello to the user in a friendly, energetic way.
Include a fun fact about programming.
EOF

# 4. Install locally for testing
/plugin install ./my-plugin -s local
```

### Publishing to a Marketplace
1. Push plugin to a GitHub repository
2. Others can install via: `/plugin install https://github.com/you/my-plugin.git`
3. Or register as marketplace: `/plugin marketplace add you/my-plugin`

## Prompt Templates

### Explore Available Plugins
```
What plugins are currently installed? Show me their commands, agents, and skills.
Also check for any plugin errors using /plugin and look at the Errors tab.
```

### Install and Configure Plugin
```
Install the feature-dev plugin from the anthropics/claude-code marketplace.
After installation, explain what agents and commands it provides.
```

### Create Custom Plugin
```
Create a plugin called "code-quality" with:
1. A /lint command that runs ESLint and fixes issues
2. A code-reviewer agent that checks for common mistakes
3. A PreToolUse hook that warns before committing without tests

Package everything properly with plugin.json.
```

## Troubleshooting

### Plugin Not Loading
```bash
# Check for errors
/plugin
# Navigate to "Errors" tab in the UI

# Verify plugin structure
ls -la .claude-plugin/

# Check plugin.json syntax
cat .claude-plugin/plugin.json | jq .
```

### Plugin Commands Not Available
- Ensure plugin is installed in correct scope (user vs local)
- Check that command files are in `.claude-plugin/commands/`
- Verify command files have `.md` extension
- Restart Claude Code session after installation

### MCP Servers from Plugin Not Starting
- Check `.mcp.json` in plugin root
- Run `claude --mcp-debug` to see connection errors
- Ensure required binaries are installed (e.g., `npx`, `uvx`)

### Common Errors
| Error | Cause | Solution |
|-------|-------|----------|
| "Plugin not found" | Wrong marketplace or name | Check exact name with `/plugin marketplace list` |
| "Invalid plugin.json" | JSON syntax error | Validate with `jq` or JSON linter |
| "Permission denied" | File permissions | `chmod -R 755 .claude-plugin/` |
| "Duplicate plugin" | Already installed | `/plugin uninstall {name}` first |

## Related Domains

- **[Subagents](02-subagents.md)** — Plugins can include custom agents
- **[Skills](03-skills.md)** — Plugins can bundle skills
- **[Hooks](04-hooks.md)** — Plugins can define lifecycle automation
- **[MCP](05-mcp.md)** — Plugins can include MCP server configurations
- **[Core Plugins](11-core-plugins.md)** — Detailed breakdown of official Anthropic plugins
