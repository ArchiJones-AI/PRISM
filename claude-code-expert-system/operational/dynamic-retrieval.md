---
domain: dynamic-retrieval
type: operational
semantic_triggers: fetch, retrieve, lookup, current, latest, up to date, verify,
  check documentation, official docs, github, what's new, recent changes, updates
cross_layer_refs:
  - supporting/source-index.md
related_operational:
  - problem-diagnosis.md
  - feature-implementation.md
last_updated: 2025-01
---

# Dynamic Knowledge Retrieval — Claude Code Expert System

When should you fetch from web sources? How do you retrieve current documentation? This document provides workflows for agentically retrieving up-to-date information from authoritative Claude Code sources.

Key terms: fetch, retrieve, web_fetch, web_search, documentation, current, latest, verify, update

---

## When to Fetch from Web Sources

### Always Fetch When

| Situation | Why | Action |
|-----------|-----|--------|
| User asks "what's new" or "recent changes" | Changelog evolves constantly | Fetch GitHub changelog |
| User reports unexpected behaviour | Documentation may have updated | Fetch relevant docs page |
| User asks about version-specific features | Features change between versions | Fetch current docs |
| Your knowledge seems uncertain | Training data may be stale | Verify against official docs |
| User asks for "current" or "latest" | Implies they need fresh info | Fetch authoritative source |
| Error messages don't match your knowledge | Error handling may have changed | Fetch troubleshooting docs |
| User mentions specific version numbers | Version-specific behaviour | Fetch changelog/docs |

### Consider Fetching When

| Situation | Decision Factor |
|-----------|-----------------|
| Complex configuration questions | Fetch if syntax might have changed |
| Plugin/MCP server availability | Fetch marketplace/directory if uncertain |
| Best practices questions | Fetch if Anthropic guidance may have evolved |
| Deprecated feature warnings | Verify current deprecation status |

### Don't Fetch When

| Situation | Why |
|-----------|-----|
| Basic conceptual questions | Concepts don't change frequently |
| User asks for comparison/analysis | Requires synthesis, not retrieval |
| Knowledge files already contain answer | Avoid redundant fetches |
| User explicitly wants quick answer | Respect time constraints |

---

## Retrieval Decision Flowchart

```
User asks a question
         │
         ▼
Is the answer in project knowledge files?
         │
    ┌────┴────┐
    │         │
   YES        NO
    │         │
    ▼         ▼
Is currency    Identify the domain
critical?      (plugins, skills, etc.)
    │               │
┌───┴───┐           ▼
│       │     Look up URL in source-index.md
NO     YES          │
│       │           ▼
▼       ▼     Use web_fetch to retrieve
Answer  Fetch to    │
from    verify      ▼
files   freshness   Synthesise answer from
        │           fetched content + knowledge
        ▼
Combine knowledge file
+ fetched verification
```

---

## Fetch Workflow: Step by Step

### Step 1: Identify the Domain

Map the user's question to a domain:

| Question Keywords | Domain | Primary URL |
|-------------------|--------|-------------|
| plugin, /plugin, marketplace | Plugins | https://code.claude.com/docs/en/plugins |
| agent, subagent, Task tool | Subagents | https://code.claude.com/docs/en/sub-agents |
| skill, SKILL.md, auto-invoke | Skills | https://code.claude.com/docs/en/skills |
| hook, PreToolUse, PostToolUse | Hooks | https://code.claude.com/docs/en/hooks-guide |
| MCP, mcp server, integration | MCP | https://code.claude.com/docs/en/mcp |
| CLAUDE.md, memory, context | Memory | https://code.claude.com/docs/en/memory |
| plan mode, Shift+Tab, explore | Plan Mode | https://code.claude.com/docs/en/common-workflows |
| headless, CI/CD, -p flag | Headless | https://code.claude.com/docs/en/headless |
| error, not working, debug | Troubleshooting | https://code.claude.com/docs/en/troubleshooting |
| statusline, output style | Output | https://code.claude.com/docs/en/statusline |

### Step 2: Fetch the Documentation

Use `web_fetch` with the appropriate URL:

```
web_fetch: https://code.claude.com/docs/en/[domain]
```

### Step 3: Synthesise the Answer

1. Read the fetched content
2. Combine with knowledge from project files
3. Prioritise fetched content for current syntax/behaviour
4. Prioritise knowledge files for context and patterns
5. Cite the source when providing specific information

---

## URL Quick Reference by Domain

### Plugins
```
Primary:   https://code.claude.com/docs/en/plugins
Discover:  https://code.claude.com/docs/en/discover-plugins
GitHub:    https://github.com/anthropics/claude-code/blob/main/plugins/README.md
Examples:  https://github.com/anthropics/claude-code/tree/main/plugins
```

### Subagents
```
Primary:   https://code.claude.com/docs/en/sub-agents
```

### Skills
```
Primary:   https://code.claude.com/docs/en/skills
GitHub:    https://github.com/anthropics/skills
```

### Hooks
```
Primary:   https://code.claude.com/docs/en/hooks-guide
Community: https://github.com/disler/claude-code-hooks-mastery
```

### MCP
```
Primary:   https://code.claude.com/docs/en/mcp
Directory: https://mcpez.com/claude-code
```

### Memory
```
Primary:   https://code.claude.com/docs/en/memory
```

### Plan Mode
```
Primary:   https://code.claude.com/docs/en/common-workflows#how-to-use-plan-mode
```

### Headless
```
Primary:   https://code.claude.com/docs/en/headless
```

### Output & Statusline
```
Styles:    https://code.claude.com/docs/en/output-styles
Statusline: https://code.claude.com/docs/en/statusline
```

### Troubleshooting
```
Primary:   https://code.claude.com/docs/en/troubleshooting
Community: https://claudelog.com/
```

### Best Practices
```
Primary:   https://www.anthropic.com/engineering/claude-code-best-practices
```

### Changelog / Updates
```
Primary:   https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
```

---

## Multi-Source Retrieval Pattern

For complex questions requiring multiple sources:

### Example: "How do I create a plugin with custom hooks?"

```
1. Fetch plugin documentation:
   web_fetch: https://code.claude.com/docs/en/plugins

2. Fetch hooks documentation:
   web_fetch: https://code.claude.com/docs/en/hooks-guide

3. Optionally fetch GitHub examples:
   web_fetch: https://github.com/anthropics/claude-code/tree/main/plugins

4. Synthesise from all sources + knowledge files
```

### Example: "My MCP server isn't connecting"

```
1. Fetch troubleshooting docs:
   web_fetch: https://code.claude.com/docs/en/troubleshooting

2. Fetch MCP-specific docs:
   web_fetch: https://code.claude.com/docs/en/mcp

3. If specific server, check directory:
   web_fetch: https://mcpez.com/claude-code

4. Synthesise diagnostic approach
```

---

## Handling Fetch Failures

| Failure Type | Action |
|--------------|--------|
| URL returns 404 | Try backup URL from source-index.md |
| Content seems outdated | Note to user, suggest checking GitHub |
| Site unreachable | Use knowledge files, note limitation |
| Content doesn't answer question | Try related domain URL |

---

## Verification Pattern

When user reports behaviour that contradicts your knowledge:

```
1. Acknowledge the discrepancy
2. Fetch current documentation
3. Compare fetched content with your knowledge
4. If documentation confirms user's experience:
   - Update your response
   - Note the change
5. If documentation matches your knowledge:
   - Suggest user check version
   - Guide through troubleshooting
```

---

## Response Template with Source Citation

When providing answers from fetched sources:

```
According to the [current Claude Code documentation](URL):

[Answer based on fetched content]

Additional context from my knowledge:
[Supplementary information from knowledge files]
```

---

## Related Operational Knowledge

- **problem-diagnosis.md** — When fetched docs help diagnose issues
- **feature-implementation.md** — When building features, verify current syntax

## Related Reference Knowledge

- **source-index.md** — Complete URL catalogue with semantic triggers
- **15-troubleshooting.md** — Error resolution that may require fresh docs
