---
domain: source-index
type: supporting
semantic_triggers: source, url, documentation, official docs, github, reference,
  authoritative, fetch, retrieve, lookup, current, latest, up to date
purpose: Master catalogue of authoritative sources with semantic triggers for dynamic retrieval
last_updated: 2025-01
---

# Source Index — Authoritative URL Catalogue with Semantic Triggers

## Overview

This index maps **semantic triggers** to **authoritative URLs** for dynamic web retrieval. When answering questions, use `web_fetch` to retrieve current documentation from these sources.

**Retrieval Priority:**
1. Tier 1 (GitHub) — Source of truth, most current
2. Tier 2 (Official Docs) — Canonical documentation
3. Tier 3 (Engineering Blogs) — Context and rationale
4. Tier 4 (Community) — Practical examples, workarounds

---

## Tier 1: GitHub Sources (Source of Truth)

### Plugins

| Semantic Triggers | URL | Fetch When |
|-------------------|-----|------------|
| plugin structure, plugin.json, plugin manifest, plugin development, create plugin, plugin directory, plugin layout | https://github.com/anthropics/claude-code/blob/main/plugins/README.md | User asks about plugin structure, development, or manifest format |
| official plugins, plugin examples, plugin source code, anthropic plugins, plugin implementations | https://github.com/anthropics/claude-code/tree/main/plugins | User needs plugin examples or wants to see official implementations |
| plugin marketplace, plugin registry, community plugins, discover plugins | https://github.com/anthropics/claude-plugins-official | User asks about marketplace or finding plugins |

### Skills

| Semantic Triggers | URL | Fetch When |
|-------------------|-----|------------|
| skills repository, official skills, skill examples, skill templates | https://github.com/anthropics/skills | User needs skill examples or templates |

### Agent SDK

| Semantic Triggers | URL | Fetch When |
|-------------------|-----|------------|
| agent sdk python, claude sdk python, programmatic agent, python sdk installation | https://github.com/anthropics/claude-agent-sdk-python | User asks about Python SDK usage or installation |
| agent sdk demos, sdk examples, agent examples, sdk sample code | https://github.com/anthropics/claude-agent-sdk-demos | User wants working SDK examples |

### Main Repository

| Semantic Triggers | URL | Fetch When |
|-------------------|-----|------------|
| changelog, release notes, version history, what's new, updates, breaking changes | https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md | User asks about recent changes or version updates |
| claude code source, main repo, claude code github | https://github.com/anthropics/claude-code | General reference or when specific doc not found |

---

## Tier 2: Official Documentation (code.claude.com)

### Plugins Documentation

| Semantic Triggers | URL | Fetch When |
|-------------------|-----|------------|
| plugin, plugins, plugin install, plugin system, /plugin, plugin commands, plugin configuration, enable plugin, disable plugin | https://code.claude.com/docs/en/plugins | Any plugin-related question |
| discover plugins, find plugins, browse plugins, plugin marketplace, available plugins, install plugin from marketplace | https://code.claude.com/docs/en/discover-plugins | User wants to find or browse available plugins |

### Subagents Documentation

| Semantic Triggers | URL | Fetch When |
|-------------------|-----|------------|
| subagent, sub-agent, agent, agents, .claude/agents/, Task tool, spawn agent, parallel agent, agent delegation, custom agent, agent context, agent isolation, /agents | https://code.claude.com/docs/en/sub-agents | Any subagent-related question |

### Skills Documentation

| Semantic Triggers | URL | Fetch When |
|-------------------|-----|------------|
| skill, skills, SKILL.md, .claude/skills/, skill creation, auto-invoke, skill frontmatter, built-in skills, docx skill, pptx skill, xlsx skill | https://code.claude.com/docs/en/skills | Any skill-related question |

### Hooks Documentation

| Semantic Triggers | URL | Fetch When |
|-------------------|-----|------------|
| hook, hooks, hooks.json, PreToolUse, PostToolUse, Stop, SubagentStop, lifecycle event, automation, event handler, deterministic, SessionStart, Notification, PreCompact | https://code.claude.com/docs/en/hooks-guide | Any hook-related question |

### MCP Documentation

| Semantic Triggers | URL | Fetch When |
|-------------------|-----|------------|
| MCP, Model Context Protocol, mcp server, mcp.json, claude mcp add, external tools, tool integration, mcp configuration, mcp debug, puppeteer mcp, github mcp, postgres mcp | https://code.claude.com/docs/en/mcp | Any MCP-related question |

### Memory Documentation

| Semantic Triggers | URL | Fetch When |
|-------------------|-----|------------|
| memory, CLAUDE.md, context, remember, project context, rules, .claude/rules/, /memory, /init, /clear, context window, compaction, session persistence | https://code.claude.com/docs/en/memory | Any memory or context-related question |

### Plan Mode Documentation

| Semantic Triggers | URL | Fetch When |
|-------------------|-----|------------|
| plan mode, planning, research mode, Shift+Tab, explore, read-only mode, codebase research, --permission-mode plan, exploration, investigate | https://code.claude.com/docs/en/common-workflows#how-to-use-plan-mode | Any plan mode question |

### Headless Documentation

| Semantic Triggers | URL | Fetch When |
|-------------------|-----|------------|
| headless, automation, -p flag, CI/CD, non-interactive, piping, scripting, --output-format, stream-json, batch processing, github actions, pre-commit, cron, unattended | https://code.claude.com/docs/en/headless | Any headless/automation question |

### Output & Statusline Documentation

| Semantic Triggers | URL | Fetch When |
|-------------------|-----|------------|
| output style, response style, verbose, concise, formatting | https://code.claude.com/docs/en/output-styles | Questions about output formatting |
| statusline, status line, /statusline, token usage, model indicator, branch display, UI customisation | https://code.claude.com/docs/en/statusline | Questions about statusline configuration |

### Troubleshooting Documentation

| Semantic Triggers | URL | Fetch When |
|-------------------|-----|------------|
| error, bug, issue, not working, problem, troubleshoot, debug, fix, broken, failure, CLINotFoundError, permission error, crash, hang, slow | https://code.claude.com/docs/en/troubleshooting | Any error or troubleshooting question |

---

## Tier 3: Official Blogs & Engineering Posts

| Semantic Triggers | URL | Fetch When |
|-------------------|-----|------------|
| best practices, recommended workflow, official guidance, anthropic recommendations, expert tips | https://www.anthropic.com/engineering/claude-code-best-practices | User asks for best practices or recommended approaches |
| skills architecture, how skills work, skills engineering, skills design | https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills | Deep questions about skill architecture |
| agent sdk patterns, building agents, sdk architecture | https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk | Deep questions about SDK patterns |

---

## Tier 4: Community Resources

| Semantic Triggers | URL | Fetch When |
|-------------------|-----|------------|
| mcp servers directory, available mcp servers, mcp server list, community mcp | https://mcpez.com/claude-code | User wants to find MCP servers |
| hooks examples, hooks mastery, advanced hooks | https://github.com/disler/claude-code-hooks-mastery | User needs advanced hook patterns |
| faq, common questions, tips and tricks, cheatsheet | https://claudelog.com/ | General tips or FAQ |
| production tips, real-world usage, neon cheatsheet | https://neon.com/blog/our-claude-code-cheatsheet | Production deployment questions |
| community plugins, third-party plugins | https://claude-plugins.dev/ | Finding community plugins |

---

## Quick Lookup Table: Topic → URL

| Topic | Primary URL | Backup URL |
|-------|-------------|------------|
| **Plugins** | https://code.claude.com/docs/en/plugins | https://github.com/anthropics/claude-code/blob/main/plugins/README.md |
| **Subagents** | https://code.claude.com/docs/en/sub-agents | — |
| **Skills** | https://code.claude.com/docs/en/skills | https://github.com/anthropics/skills |
| **Hooks** | https://code.claude.com/docs/en/hooks-guide | https://github.com/disler/claude-code-hooks-mastery |
| **MCP** | https://code.claude.com/docs/en/mcp | https://mcpez.com/claude-code |
| **Memory** | https://code.claude.com/docs/en/memory | — |
| **Plan Mode** | https://code.claude.com/docs/en/common-workflows#how-to-use-plan-mode | — |
| **Headless** | https://code.claude.com/docs/en/headless | — |
| **Statusline** | https://code.claude.com/docs/en/statusline | — |
| **Troubleshooting** | https://code.claude.com/docs/en/troubleshooting | https://claudelog.com/ |
| **Best Practices** | https://www.anthropic.com/engineering/claude-code-best-practices | — |

---

## Domain-to-URL Mapping (for Instructions)

```yaml
plugins:
  primary: https://code.claude.com/docs/en/plugins
  secondary: https://code.claude.com/docs/en/discover-plugins
  github: https://github.com/anthropics/claude-code/blob/main/plugins/README.md

subagents:
  primary: https://code.claude.com/docs/en/sub-agents

skills:
  primary: https://code.claude.com/docs/en/skills
  github: https://github.com/anthropics/skills

hooks:
  primary: https://code.claude.com/docs/en/hooks-guide

mcp:
  primary: https://code.claude.com/docs/en/mcp
  directory: https://mcpez.com/claude-code

memory:
  primary: https://code.claude.com/docs/en/memory

plan_mode:
  primary: https://code.claude.com/docs/en/common-workflows#how-to-use-plan-mode

headless:
  primary: https://code.claude.com/docs/en/headless

output_statusline:
  primary: https://code.claude.com/docs/en/output-styles
  secondary: https://code.claude.com/docs/en/statusline

troubleshooting:
  primary: https://code.claude.com/docs/en/troubleshooting

best_practices:
  primary: https://www.anthropic.com/engineering/claude-code-best-practices
```

---

## Notes on URL Stability

| Domain | URL Stability | Notes |
|--------|---------------|-------|
| code.claude.com | High | Official docs, stable paths |
| github.com/anthropics | High | Main repo, stable structure |
| anthropic.com/engineering | Medium | Blog posts may move |
| Community sites | Variable | Check if still active |

---

*Use `web_fetch` with these URLs to retrieve current documentation. Always prefer Tier 1-2 sources.*
