---
domain: hooks
semantic_triggers: hook, hooks, hooks.json, PreToolUse, PostToolUse, Stop, SubagentStop, UserPromptSubmit, SessionStart, Notification, PreCompact, SubagentStart, event handler, automation, lifecycle events, deterministic, command hook, prompt hook
priority_sources:
  - https://code.claude.com/docs/en/hooks-guide
  - https://github.com/disler/claude-code-hooks-mastery
  - https://www.anthropic.com/engineering/claude-code-best-practices
related_domains:
  - 02-subagents.md
  - 01-plugins.md
  - 08-headless.md
---

# Hooks Reference

## Quick Answer

Hooks are **deterministic automation triggered by Claude Code lifecycle events**. Unlike skills (AI-decided) or commands (user-triggered), hooks fire automatically at specific moments: before/after tool use, on session start, when stopping, etc. They run shell commands or prompt-based evaluations, and can **block actions** (exit code 2) or **modify behaviour** via JSON responses.

## Key Concepts

### All 8 Lifecycle Events

| Event | When It Fires | Can Block? | Use Cases |
|-------|---------------|------------|-----------|
| **PreToolUse** | Before any tool executes | ✅ Yes (exit 2) | Validate commands, block dangerous ops |
| **PostToolUse** | After tool completes | ❌ No | Log results, auto-format, notify |
| **Stop** | Before Claude stops responding | ✅ Yes (exit 2) | Require confirmation, final checks |
| **SubagentStart** | When subagent is spawned | ❌ No | Log agent activity, setup |
| **SubagentStop** | When subagent completes | ❌ No | Process agent results, cleanup |
| **UserPromptSubmit** | When user sends message | ❌ No | Validate input, preprocess |
| **SessionStart** | When session begins | ❌ No | Load context, setup environment |
| **Notification** | When Claude wants to notify | ❌ No | TTS, desktop alerts, logging |
| **PreCompact** | Before context compaction | ❌ No | Save state, backup transcript |

### Hook Types

| Type | Execution | Use When |
|------|-----------|----------|
| **command** | Runs shell command | Deterministic actions (format, log, block) |
| **prompt** | Uses Haiku for evaluation | Need AI judgment (is this safe?) |

### Matcher Patterns

| Pattern | Example | Matches |
|---------|---------|---------|
| **Exact** | `"Write"` | Only the Write tool |
| **Wildcard** | `"*"` | All tools |
| **Regex** | `"/^(Write\|Edit)$/"` | Write or Edit tools |
| **Tool + Subpath** | `"Bash:rm"` | Bash commands containing "rm" |

### Exit Codes (for PreToolUse, Stop)

| Exit Code | Behaviour |
|-----------|-----------|
| **0** | Allow action to proceed |
| **2** | **Block action** (with stderr as reason) |
| **Other** | Allow (hook error logged) |

### JSON Response Schema (Advanced Control)
```json
{
  "continue": true,          // Whether to proceed
  "decision": "allow",       // "allow", "block", "ask"
  "reason": "Explanation",   // Shown to user
  "systemMessage": "..."     // Inject into context
}
```

## Configuration Locations

| Location | Scope |
|----------|-------|
| `.claude/settings.json` | Project hooks |
| `~/.claude/settings.json` | User-wide hooks |
| `.claude-plugin/hooks/hooks.json` | Plugin hooks |

### Configuration Format
```json
{
  "hooks": [
    {
      "event": "PreToolUse",
      "matcher": "Bash",
      "type": "command",
      "command": "/path/to/validator.sh"
    },
    {
      "event": "PostToolUse",
      "matcher": "*",
      "type": "command", 
      "command": "echo \"Tool: $TOOL_NAME\" >> /tmp/claude.log"
    }
  ]
}
```

## Authoritative Sources

| Source | Type | URL |
|--------|------|-----|
| Hooks Guide | Official Docs | https://code.claude.com/docs/en/hooks-guide |
| Hooks Mastery | Community | https://github.com/disler/claude-code-hooks-mastery |
| Best Practices | Blog | https://www.anthropic.com/engineering/claude-code-best-practices |

## Common Patterns

### Auto-Format Code on Save
```json
{
  "hooks": [
    {
      "event": "PostToolUse",
      "matcher": "Write",
      "type": "command",
      "command": "prettier --write \"$TOOL_INPUT_PATH\" 2>/dev/null || true"
    }
  ]
}
```

### Block Dangerous Commands
```bash
#!/bin/bash
# Save to: ~/.claude/hooks/block-dangerous.sh

# Block rm -rf on system directories
if echo "$TOOL_INPUT" | grep -qE "rm\s+-rf\s+(/|/home|/etc|/var)"; then
  echo "BLOCKED: Dangerous rm -rf on system directory" >&2
  exit 2
fi

exit 0
```

```json
{
  "hooks": [
    {
      "event": "PreToolUse",
      "matcher": "Bash",
      "type": "command",
      "command": "~/.claude/hooks/block-dangerous.sh"
    }
  ]
}
```

### TTS Notification on Stop
```json
{
  "hooks": [
    {
      "event": "Stop",
      "type": "command",
      "command": "say 'Claude has finished' || espeak 'Claude has finished' || true"
    }
  ]
}
```

### Log All Tool Usage
```json
{
  "hooks": [
    {
      "event": "PostToolUse",
      "matcher": "*",
      "type": "command",
      "command": "echo \"$(date): $TOOL_NAME - $TOOL_INPUT\" >> ~/.claude/tool.log"
    }
  ]
}
```

### Require Tests Before Commit
```bash
#!/bin/bash
# Save to: .claude/hooks/require-tests.sh

if echo "$TOOL_INPUT" | grep -q "git commit"; then
  # Check if tests pass
  if ! npm test 2>/dev/null; then
    echo "BLOCKED: Tests must pass before committing" >&2
    exit 2
  fi
fi

exit 0
```

### Backup Transcript Before Compaction
```json
{
  "hooks": [
    {
      "event": "PreCompact",
      "type": "command",
      "command": "cp ~/.claude/transcript.json ~/.claude/backups/transcript-$(date +%s).json"
    }
  ]
}
```

### Prompt-Based Security Review
```json
{
  "hooks": [
    {
      "event": "PreToolUse",
      "matcher": "/^(Write|Edit|Bash)$/",
      "type": "prompt",
      "prompt": "Review this action for security risks. If it could expose secrets, delete important files, or compromise security, respond with 'BLOCK'. Otherwise respond 'ALLOW'. Action: $TOOL_INPUT"
    }
  ]
}
```

## Environment Variables Available

| Variable | Available In | Description |
|----------|--------------|-------------|
| `$TOOL_NAME` | Pre/PostToolUse | Name of the tool |
| `$TOOL_INPUT` | Pre/PostToolUse | Tool's input parameters |
| `$TOOL_INPUT_PATH` | Pre/PostToolUse | File path if applicable |
| `$TOOL_OUTPUT` | PostToolUse | Tool's output |
| `$AGENT_NAME` | SubagentStart/Stop | Name of the subagent |
| `$USER_PROMPT` | UserPromptSubmit | User's message |
| `$SESSION_ID` | SessionStart | Session identifier |

## Prompt Templates

### Create Safety Hook
```
Create a PreToolUse hook that:
1. Blocks any Bash command containing "sudo" or "chmod 777"
2. Blocks Write operations to /etc/ or /usr/
3. Warns but allows git push to main branch
4. Logs all blocked attempts to ~/.claude/blocked.log

Save the hook script and update settings.json.
```

### Create Notification System
```
Set up a notification system using hooks:
1. Play a sound when Claude finishes (Stop event)
2. Send a desktop notification when subagents complete
3. Log all tool usage to a daily file

Make it work on both macOS and Linux.
```

### Create Auto-Formatting Pipeline
```
Create PostToolUse hooks that automatically:
1. Run Prettier on any .js/.ts/.jsx/.tsx files
2. Run Black on any .py files
3. Run gofmt on any .go files

Only format files that were actually modified (use $TOOL_INPUT_PATH).
```

## Troubleshooting

### Hook Not Firing
```bash
# Check hook configuration
cat .claude/settings.json | jq '.hooks'

# Verify event name spelling (case-sensitive)
# Valid: PreToolUse, PostToolUse, Stop, SubagentStart, SubagentStop, 
#        UserPromptSubmit, SessionStart, Notification, PreCompact

# Test hook script manually
TOOL_NAME=Bash TOOL_INPUT="ls -la" ./my-hook.sh
echo $?  # Check exit code
```

### Hook Script Not Executable
```bash
chmod +x ~/.claude/hooks/my-hook.sh
```

### Hook Blocking Unexpectedly
```bash
# Debug by adding logging
#!/bin/bash
echo "Hook received: TOOL_NAME=$TOOL_NAME TOOL_INPUT=$TOOL_INPUT" >> /tmp/hook-debug.log

# Your logic here

echo "Hook decision: $?" >> /tmp/hook-debug.log
```

### Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| Hook never fires | Wrong event name | Check exact spelling (case-sensitive) |
| Hook fires for wrong tools | Matcher too broad | Use specific matcher or regex |
| Script permission denied | Not executable | `chmod +x script.sh` |
| Exit code ignored | Script crashes | Add error handling, check stderr |
| JSON response ignored | Invalid JSON | Validate JSON format |

### Debugging Hook Execution
```bash
# Run Claude Code with hook debugging
claude --debug

# Or add explicit logging in your hook
#!/bin/bash
exec 2>> ~/.claude/hook-errors.log
set -x  # Enable command tracing
```

## Hooks in Plugins

Plugins can include hooks:
```
my-plugin/
└── .claude-plugin/
    └── hooks/
        └── hooks.json
```

Plugin hooks activate when the plugin is enabled.

## Related Domains

- **[Subagents](02-subagents.md)** — SubagentStart/SubagentStop events
- **[Plugins](01-plugins.md)** — Distribute hooks via plugins
- **[Headless](08-headless.md)** — Hooks work in headless/CI mode
- **[Troubleshooting](15-troubleshooting.md)** — Debugging hook issues
