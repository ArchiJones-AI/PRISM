# Claude Code Expert System

A Claude Projects knowledge system for answering questions about Claude Code, Anthropic's official CLI tool. **Enhanced with dynamic retrieval** from authoritative web sources.

## Overview

This system provides expert assistance on:
- **Plugins** — Extension bundling and marketplace
- **Subagents** — Autonomous task execution
- **Skills** — User-invocable capabilities
- **Hooks** — Lifecycle automation
- **MCP** — External tool integrations
- **Workflows** — Development patterns and best practices

**Key Enhancement:** The system uses both static knowledge files AND dynamic web retrieval from authoritative sources to ensure answers are current and accurate.

## Directory Structure

```
claude-code-expert-system/
├── INSTRUCTIONS.md              # Routing layer with dynamic retrieval (deploy to Project Instructions)
├── operational/                  # HOW workflows
│   ├── feature-implementation.md
│   ├── problem-diagnosis.md
│   ├── workflow-execution.md
│   └── dynamic-retrieval.md     # NEW: Web retrieval workflows
├── reference/
│   └── domains/                 # WHAT knowledge (15 domain files)
│       ├── 01-plugins.md
│       ├── 02-subagents.md
│       └── ...
├── supporting/                   # Cross-cutting knowledge
│   ├── quick-reference.md
│   ├── terminology-index.md
│   ├── component-comparison.md
│   ├── source-index.md          # ENHANCED: Semantic triggers for URLs
│   └── model-selection.md
├── TEST-MATRIX.md               # 35+ verification queries
└── README.md                    # This file
```

## Deployment to Claude Projects

### Step 1: Create Project

1. Go to Claude Projects
2. Create a new project
3. Name it "Claude Code Expert" or similar

### Step 2: Set Instructions

1. Open INSTRUCTIONS.md
2. Copy the entire contents
3. Paste into the Project Instructions field
4. Save

### Step 3: Upload Knowledge Files

Upload all files from these directories:
- `operational/` — All 4 workflow files (including dynamic-retrieval.md)
- `reference/domains/` — All 15 domain files
- `supporting/` — All 5 cross-cutting files

**Total files to upload:** 24 knowledge files

### Step 4: Verify Retrieval

Run test queries from TEST-MATRIX.md to verify correct retrieval:

1. Ask a test query
2. Expand `project_knowledge_search` output
3. Verify expected domain was retrieved
4. For dynamic retrieval queries, verify web_fetch is triggered
5. Mark test as Pass/Fail

## Dynamic Retrieval Architecture

### How It Works

```
┌─────────────────────────────────────────────────────────────┐
│                    User Question                             │
└─────────────────────────────────┬───────────────────────────┘
                                  │
                                  ▼
┌─────────────────────────────────────────────────────────────┐
│         Project Knowledge Search (Static Files)              │
│   • 15 domain reference files                                │
│   • 4 operational workflow files                             │
│   • 5 supporting files                                       │
└─────────────────────────────────┬───────────────────────────┘
                                  │
                                  ▼
┌─────────────────────────────────────────────────────────────┐
│              Is Currency Critical?                           │
│   • User asks about "current", "latest", "what's new"       │
│   • User reports unexpected behaviour                        │
│   • Configuration syntax might have changed                  │
│   • Error messages don't match knowledge                     │
└─────────────────────────────────┬───────────────────────────┘
                                  │
                    ┌─────────────┴─────────────┐
                    │                           │
                   NO                          YES
                    │                           │
                    ▼                           ▼
┌───────────────────────────┐   ┌───────────────────────────────┐
│  Answer from Knowledge    │   │  web_fetch Authoritative URL  │
│  Files Only               │   │  (See source-index.md)        │
└───────────────────────────┘   └───────────────────┬───────────┘
                                                    │
                                                    ▼
                                ┌───────────────────────────────┐
                                │  Synthesise: Knowledge Files  │
                                │  + Fetched Current Content    │
                                └───────────────────────────────┘
```

### Authoritative Sources

| Tier | Source Type | Examples |
|------|-------------|----------|
| **1** | GitHub Repositories | claude-code repo, plugins, skills |
| **2** | Official Docs | code.claude.com/docs |
| **3** | Engineering Blogs | anthropic.com/engineering |
| **4** | Community | mcpez.com, claudelog.com |

### Key URLs (for web_fetch)

| Domain | Primary URL |
|--------|-------------|
| Plugins | https://code.claude.com/docs/en/plugins |
| Subagents | https://code.claude.com/docs/en/sub-agents |
| Skills | https://code.claude.com/docs/en/skills |
| Hooks | https://code.claude.com/docs/en/hooks-guide |
| MCP | https://code.claude.com/docs/en/mcp |
| Memory | https://code.claude.com/docs/en/memory |
| Plan Mode | https://code.claude.com/docs/en/common-workflows |
| Headless | https://code.claude.com/docs/en/headless |
| Troubleshooting | https://code.claude.com/docs/en/troubleshooting |
| Changelog | https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md |

## Content Architecture

### Three Knowledge Layers + Dynamic

| Layer | Purpose | Voice | Files |
|-------|---------|-------|-------|
| **Operational** | HOW to do things | Imperative | 4 files |
| **Reference** | WHAT things are | Descriptive | 15 files |
| **Supporting** | Cross-cutting concerns | Mixed | 5 files |
| **Dynamic** | Current information | Retrieved | web_fetch |

### Query Routing

The Instructions route queries to appropriate domains:
- Reference queries → reference/domains/
- Workflow queries → operational/
- Comparison queries → supporting/
- Currency-critical queries → web_fetch from source-index.md URLs

## Maintenance

### Adding New Domains

1. Create file in appropriate directory
2. Add YAML frontmatter with semantic_triggers
3. Add routing entry in INSTRUCTIONS.md
4. Add URL mapping in source-index.md
5. Add test queries to TEST-MATRIX.md
6. Re-upload affected files

### Updating Content

1. Edit the domain file
2. Update `last_updated` in frontmatter
3. Re-upload to Claude Project

### Updating Authoritative URLs

1. Edit source-index.md
2. Update URL mappings in INSTRUCTIONS.md
3. Re-upload both files

### Version Information

- **Architecture:** PRISM v3.4 methodology + Dynamic Retrieval
- **Last Updated:** January 2025
- **Domains:** 15 reference + 4 operational + 5 supporting
- **Authoritative URLs:** 20+ indexed for web_fetch
