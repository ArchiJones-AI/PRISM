---
domain: core-plugins
semantic_triggers: feature-dev, frontend-design, pr-review-toolkit, commit-commands, security-guidance, code-review, plugin-dev, anthropic plugins, official plugins, learning-output-style, code-explorer, code-architect, code-reviewer
priority_sources:
  - https://github.com/anthropics/claude-code/tree/main/plugins
  - https://github.com/anthropics/claude-plugins-official
  - https://code.claude.com/docs/en/discover-plugins
related_domains:
  - 01-plugins.md
  - 02-subagents.md
  - 03-skills.md
  - 04-hooks.md
---

# Core Anthropic Plugins Reference

## Quick Answer

Anthropic provides official plugins that extend Claude Code with specialised agents, skills, and workflows. **feature-dev** is the "gold standard" for building features with multi-agent research/plan/review. **frontend-design** generates high-quality UI. **commit-commands** adds git automation. Install from `anthropics/claude-code` marketplace.

## Installation

```bash
# Add the official marketplace (one-time)
/plugin marketplace add anthropics/claude-code

# Install specific plugins
/plugin install feature-dev
/plugin install frontend-design
/plugin install pr-review-toolkit
/plugin install commit-commands
/plugin install security-guidance
/plugin install code-review
/plugin install plugin-dev
```

## Plugin Breakdown

### feature-dev — The Gold Standard

**Purpose:** Complete feature development workflow with specialised agents.

**Contains:**
| Component | Type | Purpose |
|-----------|------|---------|
| `code-explorer` | Agent | Deep codebase research |
| `code-architect` | Agent | Implementation planning |
| `code-reviewer` | Agent | Code review and quality |

**Workflow:**
```
1. EXPLORE (code-explorer agent)
   ├─ Research relevant code
   ├─ Map dependencies
   └─ Understand patterns

2. ARCHITECT (code-architect agent)
   ├─ Design implementation
   ├─ Plan phases
   └─ Identify risks

3. IMPLEMENT (main Claude)
   ├─ Write code
   ├─ Follow architecture plan
   └─ Test incrementally

4. REVIEW (code-reviewer agent)
   ├─ Check for bugs
   ├─ Verify patterns
   └─ Suggest improvements
```

**Usage:**
```
Using the feature-dev plugin, help me implement user authentication.

Start by having code-explorer research how auth is done in similar projects,
then code-architect design our approach,
then implement it,
then have code-reviewer verify the implementation.
```

---

### frontend-design — High-Quality UI Generation

**Purpose:** Generate production-grade, non-generic UI components.

**Type:** Skill-based (auto-invokes on UI tasks)

**Capabilities:**
- React + Tailwind components
- Framer Motion animations
- Responsive layouts
- Accessibility-compliant
- Professional design aesthetics (not generic AI look)

**Usage:**
```
Create a dashboard for our analytics app with:
- Header with navigation
- Sidebar with collapsible menu
- Main area with metric cards
- Charts section (use Recharts)
- Recent activity feed

Make it look professional, not like generic AI output.
```

**Key Features:**
- Avoids "AI aesthetic" (generic gradients, over-rounded corners)
- Proper spacing and typography
- Real-world design patterns
- Mobile-responsive by default

---

### pr-review-toolkit — Post-Coding Quality Assurance

**Purpose:** Comprehensive PR review with specialised agents.

**Contains:**
| Agent | Focus Area |
|-------|------------|
| Security Reviewer | Vulnerabilities, auth issues |
| Performance Reviewer | N+1 queries, bottlenecks |
| Style Reviewer | Conventions, consistency |
| Test Reviewer | Coverage, edge cases |

**Usage:**
```
Use pr-review-toolkit to review my PR before merging.
Focus on security since this touches the payment system.
```

---

### commit-commands — Git Workflow Automation

**Purpose:** Streamlined git operations with AI assistance.

**Commands:**
| Command | Purpose |
|---------|---------|
| `/commit` | Generate commit message, create commit |
| `/pr` | Create PR with description |
| `/push` | Push with safety checks |

**Usage:**
```
/commit
# Claude analyses changes, generates conventional commit message

/pr
# Claude creates PR with:
# - Summary of changes
# - Testing notes
# - Breaking changes (if any)
```

---

### security-guidance — Vulnerability Warnings

**Purpose:** Real-time security warnings during editing.

**Type:** Hook-based (PreToolUse on file edits)

**Triggers on:**
- Editing auth-related files
- Modifying security configurations
- Working with credentials/secrets
- Touching payment/PII code

**Behaviour:**
```
When you edit src/auth/login.ts:

⚠️ Security Guidance Activated
This file handles authentication. Ensure:
- [ ] Passwords are hashed (bcrypt, cost ≥12)
- [ ] No plaintext secrets in code
- [ ] Rate limiting on auth endpoints
- [ ] Proper session management
```

---

### code-review — Automated PR Review

**Purpose:** High-quality automated code review.

**Architecture:**
- 5 parallel Sonnet agents
- Each focuses on different aspect
- Confidence scoring system
- Synthesised final review

**How it works:**
```
┌─────────────────────────────────────────────┐
│              PR Submitted                    │
└─────────────────┬───────────────────────────┘
                  │
    ┌─────────────┼─────────────┐
    ▼             ▼             ▼
┌───────┐   ┌───────┐   ┌───────┐
│Agent 1│   │Agent 2│   │Agent 3│   ... (5 total)
│Bugs   │   │Perf   │   │Style  │
└───┬───┘   └───┬───┘   └───┬───┘
    │           │           │
    └─────────┬─┴───────────┘
              │
    ┌─────────▼─────────┐
    │  Synthesis Agent  │
    │  Confidence Score │
    └─────────┬─────────┘
              │
    ┌─────────▼─────────┐
    │   Final Review    │
    │  with priorities  │
    └───────────────────┘
```

---

### plugin-dev — Plugin Creation Tools

**Purpose:** 7 expert skills for creating Claude Code plugins.

**Skills:**
| Skill | Purpose |
|-------|---------|
| Plugin Structure | Scaffolding and file layout |
| Command Design | Creating slash commands |
| Agent Design | Building custom agents |
| Skill Design | Creating auto-invoke skills |
| Hook Design | Lifecycle automation |
| MCP Integration | External tool connections |
| Publishing | Marketplace distribution |

**Usage:**
```
Using plugin-dev, help me create a plugin for our team that:
1. Adds /deploy command for our CI/CD
2. Includes a security-checker agent
3. Has hooks for code formatting
4. Connects to our internal API via MCP
```

---

### learning-output-style — Adaptive Formatting

**Purpose:** Learn and adapt to user's preferred output style.

**How it works:**
1. Observes your feedback patterns
2. Adjusts verbosity, formatting, tone
3. Remembers preferences across sessions

**Usage:**
```
# Give feedback naturally
"This is too verbose, be more concise"
"I prefer bullet points for lists"
"Always include code examples"

# Plugin learns and adapts
```

## Authoritative Sources

| Source | Type | URL |
|--------|------|-----|
| Plugins Directory | GitHub | https://github.com/anthropics/claude-code/tree/main/plugins |
| Official Marketplace | GitHub | https://github.com/anthropics/claude-plugins-official |
| Discover Plugins | Docs | https://code.claude.com/docs/en/discover-plugins |
| Community Registry | Community | https://claude-plugins.dev/ |

## Prompt Templates

### Full Feature Development
```
I need to implement [feature]. Use the feature-dev plugin workflow:

1. First, have code-explorer research:
   - How similar features are implemented in our codebase
   - What patterns we use
   - Dependencies involved

2. Then, have code-architect plan:
   - Implementation approach
   - File structure
   - Testing strategy

3. Implement following the plan

4. Finally, have code-reviewer verify:
   - Code quality
   - Security concerns
   - Test coverage

Begin with the exploration phase.
```

### UI Component Generation
```
Using frontend-design, create a [component type] with these requirements:

Design:
- [Visual requirements]
- [Layout needs]
- [Responsive behaviour]

Functionality:
- [Interactive features]
- [State management]
- [Data handling]

Make it production-ready with proper TypeScript types and accessibility.
```

### Comprehensive PR Review
```
Review this PR using code-review and pr-review-toolkit.

Focus areas:
1. Security (this touches user data)
2. Performance (high-traffic endpoint)
3. Test coverage

Provide a confidence score and prioritised list of concerns.
```

## Troubleshooting

### Plugin Not Installing
```bash
# Verify marketplace added
/plugin marketplace list

# Re-add if missing
/plugin marketplace add anthropics/claude-code

# Try install again
/plugin install feature-dev
```

### Agent Not Found
```bash
# Check plugin is enabled
/plugin list

# Verify agents are available
/agents
```

### Skill Not Auto-Invoking
- Ensure plugin is installed and enabled
- Check task matches skill description
- Restart session after installation

### Common Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| "Plugin not found" | Marketplace not added | `/plugin marketplace add anthropics/claude-code` |
| Agents unavailable | Plugin not enabled | `/plugin enable {name}` |
| Hook not firing | Wrong file pattern | Check hook configuration |
| Skill ignored | Description mismatch | Use explicit trigger words |

## Related Domains

- **[Plugins](01-plugins.md)** — Plugin system fundamentals
- **[Subagents](02-subagents.md)** — Custom agent creation
- **[Skills](03-skills.md)** — Skill architecture
- **[Hooks](04-hooks.md)** — Hook system details
