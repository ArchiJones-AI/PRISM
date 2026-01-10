---
domain: output-statusline
semantic_triggers: output style, statusline, status line, /statusline, token usage, model indicator, branch display, UI customisation, ccusage, verbose, concise, formatting, response style
priority_sources:
  - https://code.claude.com/docs/en/output-styles
  - https://code.claude.com/docs/en/statusline
  - https://neon.com/blog/our-claude-code-cheatsheet
related_domains:
  - 12-commands-shortcuts.md
  - 06-memory.md
---

# Output Styles & Statusline Reference

## Quick Answer

**Output styles** control how Claude formats responses (verbose, concise, code-focused). **Statusline** is the customisable info strip showing model, git branch, tokens, etc. Configure with `/statusline add` command. Use `ccusage` for API cost tracking. The `learning-output-style` plugin adapts to your preferences over time.

## Output Styles

### Available Styles

| Style | Description | Best For |
|-------|-------------|----------|
| **Default** | Balanced detail | General use |
| **Concise** | Minimal explanation | Experienced users |
| **Verbose** | Detailed explanations | Learning, debugging |
| **Code-focused** | Minimal prose, more code | Implementation |

### Setting Output Style

**In conversation:**
```
"Be more concise in your responses"
"Give me detailed explanations"
"Just show me the code, minimal explanation"
```

**Via CLAUDE.md:**
```markdown
## Response Preferences
- Be concise and direct
- Prefer code examples over explanations
- Use bullet points for lists
- Skip preambles like "Sure, I can help with that"
```

### Persistent Style with learning-output-style Plugin
```bash
# Install
/plugin install learning-output-style

# Plugin learns from feedback:
"Too verbose" → Becomes more concise
"Explain more" → Becomes more detailed
"More examples" → Includes more code samples
```

## Statusline Configuration

### Default Components

```
┌─────────────────────────────────────────────────────────────┐
│ > Your prompt here...                                        │
├─────────────────────────────────────────────────────────────┤
│ sonnet │ main │ 1,234 tokens │ ~/project                     │
└─────────────────────────────────────────────────────────────┘
         ↑        ↑       ↑              ↑
       Model   Branch   Usage      Directory
```

### Available Components

| Component | Shows | Command |
|-----------|-------|---------|
| `model` | Current model (sonnet/opus/haiku) | Built-in |
| `git_branch` | Current git branch | Built-in |
| `tokens` | Token usage | Built-in |
| `directory` | Working directory | Built-in |
| `custom` | Custom script output | `/statusline add` |

### Adding Custom Components

```bash
# Add script-based component
/statusline add "My Script" "/path/to/script.sh"

# Example: Show current time
/statusline add "Time" "date +%H:%M"

# Example: Show git status
/statusline add "Status" "git status --short | wc -l"

# Example: Show test status
/statusline add "Tests" "./scripts/test-status.sh"
```

### Custom Script Examples

**API Cost Tracker:**
```bash
#!/bin/bash
# ~/.claude/scripts/cost.sh
# Requires ccusage: npm install -g ccusage

ccusage --today --short 2>/dev/null || echo "N/A"
```

```bash
/statusline add "Cost" "~/.claude/scripts/cost.sh"
```

**Build Status:**
```bash
#!/bin/bash
# ~/project/scripts/build-status.sh

if [ -f "dist/index.js" ]; then
    echo "✓ Built"
else
    echo "⚠ Not built"
fi
```

**Test Coverage:**
```bash
#!/bin/bash
# ~/project/scripts/coverage.sh

if [ -f "coverage/coverage-summary.json" ]; then
    jq -r '.total.lines.pct' coverage/coverage-summary.json | xargs printf "%.0f%%"
else
    echo "N/A"
fi
```

## ccusage — API Cost Tracking

### Installation
```bash
npm install -g ccusage
```

### Usage
```bash
# Today's usage
ccusage --today

# This week
ccusage --week

# Detailed breakdown
ccusage --detailed

# Short format (for statusline)
ccusage --today --short
```

### Integrate with Statusline
```bash
/statusline add "API Cost" "ccusage --today --short 2>/dev/null"
```

## Authoritative Sources

| Source | Type | URL |
|--------|------|-----|
| Output Styles | Official Docs | https://code.claude.com/docs/en/output-styles |
| Statusline | Official Docs | https://code.claude.com/docs/en/statusline |
| Neon Cheatsheet | Community | https://neon.com/blog/our-claude-code-cheatsheet |

## Common Patterns

### Developer Statusline Setup
```bash
# Model + Branch + Tokens + Cost
/statusline add "Cost" "ccusage --today --short 2>/dev/null || echo '?'"

# Result:
# sonnet │ feature-branch │ 2,345 tokens │ $1.23
```

### Minimal Statusline
```bash
# Remove all but essentials
# (Configure via /config → statusline)
# Keep only: model, tokens
```

### Information-Rich Statusline
```bash
# Add multiple custom components
/statusline add "Tests" "./scripts/test-count.sh"
/statusline add "TODO" "grep -r 'TODO' src/ 2>/dev/null | wc -l"
/statusline add "Build" "./scripts/build-status.sh"
```

### Dynamic Context Indicator
```bash
#!/bin/bash
# ~/.claude/scripts/context-health.sh

TOKENS=$(cat ~/.claude/last_tokens 2>/dev/null || echo 0)
if [ "$TOKENS" -lt 50000 ]; then
    echo "🟢"
elif [ "$TOKENS" -lt 150000 ]; then
    echo "🟡"
else
    echo "🔴"
fi
```

## Prompt Templates

### Configure Statusline
```
Set up my statusline to show:
1. Current model
2. Git branch
3. Token usage
4. API cost (using ccusage)
5. Build status indicator

Create any necessary scripts and add them to statusline.
```

### Optimise Output Style
```
I find your responses too verbose. Please:
1. Be more concise
2. Lead with code, explain after
3. Skip confirmations like "Sure, I can help"
4. Use bullet points for lists

Add these preferences to my CLAUDE.md.
```

### Cost Monitoring Setup
```
Set up API cost monitoring:
1. Install ccusage
2. Add to statusline
3. Create a daily cost report script
4. Set up alert if cost exceeds $X/day

Help me track my Claude Code spending.
```

## Troubleshooting

### Statusline Not Updating
```bash
# Scripts must be executable
chmod +x ~/.claude/scripts/my-script.sh

# Check script works independently
~/.claude/scripts/my-script.sh

# Statusline updates on each prompt
# Not real-time
```

### Custom Script Errors
```bash
# Scripts should handle errors gracefully
#!/bin/bash
result=$(my-command 2>/dev/null) || result="N/A"
echo "$result"
```

### ccusage Not Working
```bash
# Verify installation
which ccusage

# Check it runs
ccusage --help

# Ensure API key is configured
echo $ANTHROPIC_API_KEY | head -c 10
```

### Output Style Not Persisting
- Add preferences to CLAUDE.md for persistence
- Install learning-output-style plugin for adaptive learning
- Preferences reset on `/clear`

### Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| Script not found | Wrong path | Use absolute path |
| Permission denied | Not executable | `chmod +x script.sh` |
| Statusline blank | Script error | Test script independently |
| ccusage timeout | API issue | Check network, API key |

## Best Practices

### Output Style
- Be consistent with preferences
- Put preferences in CLAUDE.md for persistence
- Use learning-output-style for gradual adaptation
- Be specific about what you want changed

### Statusline
- Don't overload with too many components
- Keep scripts fast (<100ms)
- Handle errors gracefully (don't break on failure)
- Use short output (fits in statusline)

### Cost Tracking
- Set up ccusage early
- Monitor daily to catch unexpected usage
- Consider alerts for high-cost days
- Review which tasks consume most tokens

## Related Domains

- **[Commands](12-commands-shortcuts.md)** — /statusline command
- **[Memory](06-memory.md)** — Storing preferences in CLAUDE.md
- **[Workflows](13-workflows.md)** — Monitoring during work
