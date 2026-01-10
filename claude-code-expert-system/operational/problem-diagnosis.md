---
domain: problem-diagnosis
type: operational
semantic_triggers: debug, diagnose, fix, error, not working, broken, troubleshoot,
  issue, problem, help, stuck, failing
cross_layer_refs:
  - reference/domains/15-troubleshooting.md
related_operational:
  - feature-implementation.md
  - workflow-execution.md
last_updated: 2025-01
---

# Problem Diagnosis — Claude Code Expert System

How do I debug a Claude Code issue? What's the process for diagnosing problems? This document provides a systematic workflow for troubleshooting Claude Code issues including plugins, skills, hooks, and configuration problems.

Key terms: debug, diagnose, troubleshoot, fix, error, not working, problem, issue, failing, broken

---

## Systematic Diagnosis Workflow

### Purpose

Guide users through a structured process to identify and resolve Claude Code issues.

### When to Use

Use this diagnosis workflow when:
- Something isn't working as expected
- You see an error message
- A feature behaves unexpectedly
- You're stuck and need help debugging

### Process Steps

**Step 1: Identify the Component**

First, determine which Claude Code component is involved:

| Symptom | Likely Component |
|---------|------------------|
| Plugin not loading | Plugin configuration |
| Skill not activating | Skill triggers/registration |
| Hook not firing | Hook setup/permissions |
| MCP connection failing | MCP server configuration |
| Context not persisting | Memory/CLAUDE.md |
| Command not found | Commands/shortcuts |

**Step 2: Gather Error Information**

Collect all available error information:

1. **Exact error message** — Copy the verbatim text
2. **When it occurs** — What action triggers the error?
3. **Reproducibility** — Does it happen every time?
4. **Recent changes** — What changed before it broke?

Run with verbose logging if available: `claude --verbose`

**Step 3: Check Common Causes**

For the identified component, check these common causes:

| Component | Common Causes |
|-----------|---------------|
| Plugins | Invalid plugin.json, missing permissions, wrong path |
| Skills | Syntax errors, trigger conflicts, not registered |
| Hooks | Not executable, wrong event type, script errors |
| MCP | Server not running, auth issues, config errors |

**Step 4: Consult Reference Knowledge**

Look up the specific error in reference/domains/15-troubleshooting.md. Match your error message to documented solutions.

**Step 5: Isolate the Issue**

If common causes don't apply, isolate the problem:

1. **Disable other components** — Does it work alone?
2. **Use minimal reproduction** — Simplest case that fails
3. **Check versions** — Are you on latest Claude Code?

**Step 6: Apply Fix and Verify**

Once you identify the cause:

1. Apply the fix
2. Test that the issue is resolved
3. Verify no new issues were introduced

### Decision Tree

```
Issue Reported
     │
     ▼
Is there an error message?
     │
     ├─ Yes ─▶ Search error in 15-troubleshooting.md
     │              │
     │              ├─ Found ─▶ Apply documented fix
     │              │
     │              └─ Not found ─▶ Continue to Step 3
     │
     └─ No ─▶ Describe expected vs actual behavior
                    │
                    ▼
              Identify component (Step 1)
                    │
                    ▼
              Check common causes (Step 3)
```

---

## Quick Diagnosis by Error Type

### Permission Errors

When you see "permission denied" or "access denied":
1. Check file permissions on scripts
2. Verify hooks are executable: `chmod +x script.sh`
3. Confirm Claude Code has necessary access

### Configuration Errors

When you see "invalid config" or "parse error":
1. Validate JSON syntax in config files
2. Check for missing required fields
3. Verify file paths are correct

### Connection Errors

When you see "connection refused" or "timeout":
1. Confirm the service is running
2. Check network configuration
3. Verify authentication credentials

---

## When to Escalate

If the diagnosis workflow doesn't resolve the issue:

1. **Check GitHub Issues** — Search for similar reports
2. **Check Official Docs** — Verify expected behavior
3. **File a Bug Report** — With minimal reproduction case

---

## Related Operational Knowledge

- **feature-implementation.md** — When debugging implementation issues
- **workflow-execution.md** — When workflows aren't executing properly

## Related Reference Knowledge

- **reference/domains/15-troubleshooting.md** — Error messages and solutions
- **supporting/source-index.md** — Where to find authoritative help
