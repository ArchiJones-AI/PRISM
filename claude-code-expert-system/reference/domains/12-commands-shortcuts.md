---
domain: commands-shortcuts
semantic_triggers: command, commands, slash command, shortcut, keyboard, /clear, /init, /model, /config, /agents, /hooks, /plugin, /commit, /pr, Tab, Escape, Ctrl, @, !, &, custom command, keyboard shortcut
priority_sources:
  - https://code.claude.com/docs/en/common-workflows
  - https://www.anthropic.com/engineering/claude-code-best-practices
  - https://claudelog.com/
related_domains:
  - 06-memory.md
  - 01-plugins.md
  - 13-workflows.md
---

# Commands & Shortcuts Reference

## Quick Answer

Claude Code uses **slash commands** (`/command`) for built-in operations and custom workflows, **keyboard shortcuts** for quick actions, and **special syntax** (`@`, `!`, `&`) for file inclusion and command composition. Create custom commands in `.claude/commands/*.md`. Use `/clear` between tasks, `Tab` to toggle thinking, `Escape` to interrupt.

## Built-in Slash Commands

### Session Management

| Command | Purpose | When to Use |
|---------|---------|-------------|
| `/clear` | Reset context window | Between major tasks |
| `/init` | Reload CLAUDE.md and rules | After editing memory files |
| `/memory` | View loaded memory | Check what context is active |

### Model & Settings

| Command | Purpose | Options |
|---------|---------|---------|
| `/model` | Switch models | `sonnet`, `opus`, `haiku`, or full name |
| `/config` | Open settings | Interactive configuration |
| `/statusline` | Configure status bar | Add/remove components |

### Component Management

| Command | Purpose | Subcommands |
|---------|---------|-------------|
| `/agents` | Manage subagents | List, enable, disable |
| `/hooks` | Manage hooks | View, test |
| `/plugin` | Manage plugins | install, uninstall, marketplace |

### Git & Workflow (with commit-commands plugin)

| Command | Purpose |
|---------|---------|
| `/commit` | Generate message, create commit |
| `/pr` | Create pull request |
| `/push` | Push with safety checks |

### Diagnostics

| Command | Purpose |
|---------|---------|
| `/bug` | Report issue to Anthropic |

## Keyboard Shortcuts

### Essential Shortcuts

| Shortcut | Action | Notes |
|----------|--------|-------|
| `Tab` | Toggle thinking mode | Shows/hides thinking output |
| `Shift+Tab+Tab` | Toggle Plan Mode | Read-only research mode |
| `Escape` | Interrupt Claude | Stops current generation |
| `Double Escape` | Edit previous prompt | Modify your last message |
| `Ctrl+B` | Send to background | Runs async, returns when done |
| `Ctrl+O` | Toggle verbose mode | See thinking in grey italic |
| `Ctrl+C` | Cancel/exit | Standard terminal cancel |

### Navigation

| Shortcut | Action |
|----------|--------|
| `↑` / `↓` | Navigate history |
| `Ctrl+R` | Search history |
| `Ctrl+L` | Clear screen (not context) |

## Special Syntax

### @ Syntax — File Inclusion
```bash
# Include specific file
@src/auth/login.ts

# Include directory contents
@src/components/

# Include multiple files
@package.json @tsconfig.json

# Include with glob
@src/**/*.test.ts
```

### ! Syntax — Command Composition
```bash
# Run shell command and include output
!git diff HEAD~3

# Combine with prompt
!git log --oneline -10
"Summarise these recent commits"

# Dynamic context
!npm test 2>&1
"Analyse these test failures"
```

### & Syntax — Claude Code on Web (v2.0.45+)
```bash
# Send to web interface
& What is the architecture of this project?

# Opens in Claude Code Web with context
```

## Custom Commands

### Creating Custom Commands

Location: `.claude/commands/*.md` (project) or `~/.claude/commands/*.md` (user)

**Simple command:**
```markdown
<!-- .claude/commands/fix-lint.md -->
Run the linter and fix all auto-fixable issues:
1. Run `npm run lint -- --fix`
2. If errors remain, fix them manually
3. Commit with message "fix: lint errors"
```

**Command with arguments ($ARGUMENTS):**
```markdown
<!-- .claude/commands/fix-github-issue.md -->
Analyse and fix the GitHub issue: $ARGUMENTS

Follow these steps:
1. Use `gh issue view $ARGUMENTS` to get details
2. Understand the problem
3. Search codebase for relevant files
4. Implement the fix
5. Write tests
6. Create descriptive commit
7. Push and create PR

Use `gh` CLI for all GitHub operations.
```

**Usage:**
```bash
/project:fix-github-issue 1234
# $ARGUMENTS becomes "1234"
```

### Command Templates

**Deploy command:**
```markdown
<!-- .claude/commands/deploy.md -->
Deploy to $ARGUMENTS environment.

Steps:
1. Run tests: `npm test`
2. Build: `npm run build`
3. Deploy: `./scripts/deploy.sh $ARGUMENTS`
4. Verify: `curl https://$ARGUMENTS.example.com/health`
5. Report status

If any step fails, stop and report the error.
```

**PR preparation:**
```markdown
<!-- .claude/commands/prepare-pr.md -->
Prepare this branch for a PR:

1. Run all tests and fix failures
2. Run linter and fix issues
3. Update CHANGELOG.md
4. Generate PR description based on commits
5. Check for sensitive data in diff

Output the PR description for review.
```

**Code review:**
```markdown
<!-- .claude/commands/review.md -->
Review the code in $ARGUMENTS:

1. Read all files in the path
2. Check for:
   - Bugs and logic errors
   - Security vulnerabilities
   - Performance issues
   - Style inconsistencies
3. Suggest improvements
4. Rate overall quality (1-10)

Be constructive and specific.
```

## Authoritative Sources

| Source | Type | URL |
|--------|------|-----|
| Common Workflows | Official Docs | https://code.claude.com/docs/en/common-workflows |
| Best Practices | Blog | https://www.anthropic.com/engineering/claude-code-best-practices |
| Claudelog | Community | https://claudelog.com/ |

## Common Patterns

### Daily Workflow Shortcuts
```bash
# Start of day
/init                    # Refresh memory
/model sonnet            # Ensure fast model

# During work
Tab                      # Toggle thinking for complex tasks
@relevant-files          # Quick context loading
!git status              # Check state

# Between tasks
/clear                   # Reset context

# End of day
/commit                  # Commit work
```

### Quick Context Loading
```bash
# Load related files
@src/auth/ "Review the authentication system"

# Load with shell output
!find . -name "*.test.ts" -mtime -1
"Run these recently modified tests"

# Load from git
!git diff main
"Review these changes"
```

### Custom Command Organisation
```
.claude/commands/
├── deploy-staging.md     # /project:deploy-staging
├── deploy-prod.md        # /project:deploy-prod
├── fix-issue.md          # /project:fix-issue {number}
├── review.md             # /project:review {path}
├── prepare-pr.md         # /project:prepare-pr
└── generate-docs.md      # /project:generate-docs
```

## Prompt Templates

### Create Custom Commands
```
Create these custom commands for our project:

1. /project:setup — Initialize development environment
2. /project:test — Run tests with coverage report
3. /project:deploy — Deploy to environment (takes $ARGUMENTS)
4. /project:hotfix — Emergency fix workflow

Each command should:
- Have clear step-by-step instructions
- Include error handling
- Use our project conventions
```

### Keyboard Workflow
```
Help me understand the best keyboard shortcuts for:
1. Rapidly iterating on code changes
2. Managing context during long sessions
3. Efficiently reviewing code
4. Quick model switching for different tasks
```

## Troubleshooting

### Command Not Found
```bash
# Check command location
ls .claude/commands/
ls ~/.claude/commands/

# Verify .md extension
file .claude/commands/my-command.md

# Commands are prefixed by scope
# Project: /project:command-name
# User: /user:command-name
```

### $ARGUMENTS Not Expanding
- Ensure using exact `$ARGUMENTS` (uppercase)
- Check command file encoding (UTF-8)
- Verify command file has `.md` extension

### Shortcuts Not Working
- Check terminal compatibility
- Some shortcuts require specific terminal emulators
- Try alternative (e.g., if Ctrl+B fails, use `claude --background`)

### Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| "Unknown command" | Typo or wrong scope | Check `/` prefix and scope |
| "$ARGUMENTS literal" | Wrong variable name | Use exactly `$ARGUMENTS` |
| Shortcut ignored | Terminal capture | Try different terminal |
| Command not listed | Wrong directory | Use `.claude/commands/` |

## Related Domains

- **[Memory](06-memory.md)** — /memory, /init, /clear commands
- **[Plugins](01-plugins.md)** — Plugin-provided commands
- **[Workflows](13-workflows.md)** — Using commands in workflows
- **[Troubleshooting](15-troubleshooting.md)** — Debugging commands
