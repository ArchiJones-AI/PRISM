---
domain: headless
semantic_triggers: headless, automation, -p flag, CI/CD, non-interactive, piping, scripting, --output-format, stream-json, batch processing, github actions, pre-commit, cron, unattended, pipeline, automated
priority_sources:
  - https://code.claude.com/docs/en/headless
  - https://www.anthropic.com/engineering/claude-code-best-practices
related_domains:
  - 04-hooks.md
  - 10-agent-sdk.md
  - 13-workflows.md
---

# Headless Mode Reference

## Quick Answer

Headless mode runs Claude Code **non-interactively** for automation, CI/CD, and scripting. Use `claude -p "prompt"` to send a single prompt and get output. Pipe data with `echo "data" | claude -p "analyse"`. For structured output, use `--output-format stream-json`. **Warning:** Use `--dangerously-skip-permissions` only in trusted, sandboxed environments.

## Key Concepts

### Basic Syntax
```bash
# Simple prompt
claude -p "explain this error: division by zero"

# With file context
claude -p "review this code" < myfile.py

# Piped input
cat error.log | claude -p "summarise these errors"

# Multiple files
claude -p "compare these files" file1.js file2.js
```

### Output Formats

| Format | Flag | Use Case |
|--------|------|----------|
| **Text** | (default) | Human-readable output |
| **Stream JSON** | `--output-format stream-json` | Programmatic parsing |
| **JSON** | `--output-format json` | Structured single response |

### Stream JSON Format
```json
{"type": "assistant", "content": "Here's my analysis..."}
{"type": "tool_use", "name": "Read", "input": {"path": "file.py"}}
{"type": "tool_result", "content": "file contents..."}
{"type": "assistant", "content": "Based on the file..."}
{"type": "result", "content": "Final answer here"}
```

### Permission Modes

| Mode | Flag | Behaviour |
|------|------|-----------|
| **Default** | (none) | Prompts for tool approvals |
| **Accept Edits** | `--permission-mode acceptEdits` | Auto-approve file edits |
| **Plan** | `--permission-mode plan` | Read-only mode |
| **Bypass** | `--dangerously-skip-permissions` | ⚠️ No prompts at all |

## Authoritative Sources

| Source | Type | URL |
|--------|------|-----|
| Headless Documentation | Official Docs | https://code.claude.com/docs/en/headless |
| Best Practices | Blog | https://www.anthropic.com/engineering/claude-code-best-practices |

## Common Patterns

### GitHub Actions: Auto-Label Issues
```yaml
# .github/workflows/label-issues.yml
name: Label New Issues
on:
  issues:
    types: [opened]

jobs:
  label:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code
      
      - name: Analyse and Label Issue
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          LABELS=$(claude -p "Analyse this GitHub issue and suggest labels (bug, feature, docs, question). Output only comma-separated labels, nothing else.

          Title: ${{ github.event.issue.title }}
          Body: ${{ github.event.issue.body }}")
          
          gh issue edit ${{ github.event.issue.number }} --add-label "$LABELS"
```

### GitHub Actions: Auto-Fix Lint Errors
```yaml
# .github/workflows/auto-fix.yml
name: Auto-Fix Lint
on:
  pull_request:
    types: [opened, synchronize]

jobs:
  fix:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          ref: ${{ github.head_ref }}
      
      - name: Install Dependencies
        run: npm ci
      
      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code
      
      - name: Fix Lint Errors
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          # Get lint errors
          npm run lint 2>&1 | claude -p "Fix these lint errors. Use Edit tool to modify files directly." \
            --dangerously-skip-permissions
      
      - name: Commit Fixes
        run: |
          git config user.name "Claude Bot"
          git config user.email "claude@example.com"
          git add -A
          git diff --cached --quiet || git commit -m "fix: auto-fix lint errors"
          git push
```

### Pre-Commit Hook
```bash
#!/bin/bash
# .git/hooks/pre-commit

# Get staged files
STAGED_FILES=$(git diff --cached --name-only --diff-filter=ACM | grep -E '\.(js|ts|py)$')

if [ -z "$STAGED_FILES" ]; then
  exit 0
fi

# Quick code review
echo "$STAGED_FILES" | xargs cat | claude -p "Quick review: any obvious bugs, security issues, or style violations? Be brief." 

# Or auto-fix
# claude -p "Fix any issues in these staged files" $STAGED_FILES --permission-mode acceptEdits
```

### Batch Processing
```bash
#!/bin/bash
# process-docs.sh

for file in docs/*.md; do
  echo "Processing: $file"
  claude -p "Improve this documentation: fix grammar, improve clarity, add examples where helpful." \
    < "$file" \
    --permission-mode acceptEdits \
    > "${file}.improved"
  mv "${file}.improved" "$file"
done
```

### Cron Job: Daily Code Review
```bash
# Add to crontab: 0 9 * * * /path/to/daily-review.sh

#!/bin/bash
# daily-review.sh

cd /path/to/project

# Get yesterday's commits
COMMITS=$(git log --since="1 day ago" --oneline)

if [ -z "$COMMITS" ]; then
  echo "No commits to review"
  exit 0
fi

# Generate review
REVIEW=$(claude -p "Review these commits for potential issues:

$COMMITS

Check for: bugs, security issues, performance problems, missing tests.")

# Send notification
echo "$REVIEW" | mail -s "Daily Code Review" team@example.com
```

### Piping Data Analysis
```bash
# Analyse logs
tail -1000 /var/log/app.log | claude -p "Summarise errors and warnings. Group by type."

# Analyse CSV
cat sales.csv | claude -p "Identify trends and anomalies in this sales data."

# Diff review
git diff main..feature | claude -p "Review this diff for issues."
```

### Structured Output for Scripts
```bash
#!/bin/bash

# Get JSON output for parsing
RESULT=$(claude -p "Analyse this code for bugs. Output JSON: {\"bugs\": [{\"file\": \"...\", \"line\": N, \"severity\": \"high|medium|low\", \"description\": \"...\"}]}" \
  --output-format json \
  < mycode.py)

# Parse with jq
echo "$RESULT" | jq -r '.bugs[] | select(.severity == "high") | "\(.file):\(.line) - \(.description)"'
```

## Prompt Templates

### CI/CD Integration
```
Set up a GitHub Action that:
1. Triggers on PR open/sync
2. Runs Claude Code to review the diff
3. Posts review comments on the PR
4. Suggests improvements

Include proper secrets handling and error handling.
```

### Batch Automation
```
Create a script that processes all Python files in src/:
1. Adds missing docstrings
2. Fixes type hints
3. Updates to modern Python syntax

Use headless mode with appropriate permissions.
```

### Monitoring Integration
```
Create a cron job that:
1. Runs every hour
2. Analyses application logs for errors
3. Categorises issues by severity
4. Sends Slack alert if critical issues found

Use stream-json output for parsing.
```

## Troubleshooting

### "Permission denied" in CI
```bash
# Use --dangerously-skip-permissions in sandboxed CI
claude -p "..." --dangerously-skip-permissions

# Or use acceptEdits for file modifications only
claude -p "..." --permission-mode acceptEdits
```

### Timeout in Long Operations
```bash
# Increase timeout (if available)
timeout 300 claude -p "complex task..."

# Or break into smaller tasks
claude -p "step 1..." && claude -p "step 2..."
```

### Output Parsing Issues
```bash
# Use stream-json for reliable parsing
claude -p "..." --output-format stream-json | while read -r line; do
  TYPE=$(echo "$line" | jq -r '.type')
  if [ "$TYPE" = "result" ]; then
    CONTENT=$(echo "$line" | jq -r '.content')
    echo "$CONTENT"
  fi
done
```

### API Key Not Found
```bash
# Ensure key is set
export ANTHROPIC_API_KEY=sk-ant-...

# In GitHub Actions
env:
  ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}

# Verify
echo $ANTHROPIC_API_KEY | head -c 10
```

### Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| "API key not found" | Missing env var | Set ANTHROPIC_API_KEY |
| "Permission required" | Missing skip flag | Add `--dangerously-skip-permissions` |
| "Timeout" | Task too long | Break into subtasks |
| "Parse error" | Wrong output format | Use `--output-format stream-json` |

## Security Considerations

### ⚠️ --dangerously-skip-permissions

This flag is dangerous because Claude can:
- Execute arbitrary bash commands
- Modify any files
- Delete files
- Access network resources

**Only use when:**
- Running in sandboxed environment (Docker, CI runner)
- No sensitive data accessible
- Output is validated before use
- Environment is disposable

**Never use:**
- On production servers
- With access to secrets
- On personal machines with sensitive data

### Sandboxing Recommendations
```bash
# Run in Docker
docker run --rm -v $(pwd):/workspace -w /workspace \
  -e ANTHROPIC_API_KEY=$ANTHROPIC_API_KEY \
  node:20 \
  npx @anthropic-ai/claude-code -p "..." --dangerously-skip-permissions

# Limit filesystem access
docker run --rm -v $(pwd)/src:/workspace/src:rw \
  -v $(pwd)/tests:/workspace/tests:ro \
  ...
```

## Related Domains

- **[Hooks](04-hooks.md)** — Hooks work in headless mode
- **[Agent SDK](10-agent-sdk.md)** — Programmatic alternative to headless
- **[Workflows](13-workflows.md)** — Automating development workflows
- **[Troubleshooting](15-troubleshooting.md)** — CI/CD debugging
