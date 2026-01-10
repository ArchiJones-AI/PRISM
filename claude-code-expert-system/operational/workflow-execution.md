---
domain: workflow-execution
type: operational
semantic_triggers: workflow, how to, step by step, process, plan mode, commit,
  review, plan-code-commit, best practice, pattern
cross_layer_refs:
  - reference/domains/13-workflows.md
  - reference/domains/07-plan-mode.md
related_operational:
  - feature-implementation.md
  - problem-diagnosis.md
last_updated: 2025-01
---

# Workflow Execution — Claude Code Expert System

How do I execute a plan-code-commit workflow? What's the step-by-step process for common Claude Code patterns? This document provides actionable workflows for executing common Claude Code development patterns.

Key terms: workflow, step by step, process, plan, code, commit, review, execute, pattern, best practice

---

## Plan-Code-Commit Workflow

### Purpose

Execute a structured development workflow: plan first, implement second, commit last.

### When to Use

Use this workflow for:
- Non-trivial code changes
- Features requiring design decisions
- Changes affecting multiple files

### Process Steps

**Step 1: Enter Plan Mode**

Start by researching and planning without making changes:

```
/plan
```

Or use the keyboard shortcut for plan mode. Plan mode is read-only—Claude can explore but not modify files.

**Step 2: Research the Codebase**

In plan mode, investigate:
- Existing patterns and conventions
- Files that need modification
- Dependencies and implications

Ask Claude to explore: "Find where X is implemented" or "Show me how Y works"

**Step 3: Design the Approach**

Have Claude propose an implementation plan:
- Which files to create/modify
- What changes to make in each file
- Order of operations

Review and approve the plan before proceeding.

**Step 4: Exit Plan Mode and Implement**

Leave plan mode to start implementation:

```
/exitplan
```

Claude will execute the approved plan, making the actual changes.

**Step 5: Review Changes**

Before committing, review what changed:

```
git diff
```

Verify the implementation matches the plan.

**Step 6: Commit Changes**

Use the commit skill to create a commit:

```
/commit
```

Claude will generate an appropriate commit message based on the changes.

### Output

Committed code changes following a thoughtful, planned approach.

---

## Code Review Workflow

### Purpose

Review code changes systematically using Claude's analysis capabilities.

### Process Steps

**Step 1: Specify Scope**

Tell Claude what to review:
- "Review the changes in this PR"
- "Review file X for issues"
- "Check this function for bugs"

**Step 2: Request Specific Checks**

Direct the review focus:

| Focus Area | Prompt Pattern |
|------------|----------------|
| Bugs | "Look for logic errors and edge cases" |
| Security | "Check for security vulnerabilities" |
| Performance | "Identify performance concerns" |
| Style | "Check adherence to conventions" |

**Step 3: Address Findings**

For each finding:
1. Evaluate if it's valid
2. Make fixes if needed
3. Discuss trade-offs if applicable

---

## Multi-File Refactoring Workflow

### Purpose

Execute large-scale refactoring across multiple files safely.

### Process Steps

**Step 1: Define Scope**

Clearly specify what you're refactoring and why.

**Step 2: Create a Checklist**

Have Claude identify all files/locations that need changes. Use the todo list to track progress.

**Step 3: Execute Incrementally**

Make changes in logical groups:
1. Change one aspect across all files
2. Test that the change works
3. Proceed to next aspect

**Step 4: Verify Completeness**

Use grep/search to confirm no instances were missed.

**Step 5: Run Tests**

Execute the full test suite to catch regressions.

---

## Exploration Workflow

### Purpose

Understand an unfamiliar codebase systematically.

### Process Steps

**Step 1: Start High-Level**

Ask for architecture overview:
- "What's the structure of this codebase?"
- "What are the main components?"

**Step 2: Drill Down**

Focus on specific areas:
- "How does X feature work?"
- "Where is Y implemented?"

**Step 3: Document Findings**

Have Claude update CLAUDE.md with discovered patterns and conventions.

---

## Related Operational Knowledge

- **feature-implementation.md** — Building new features within workflows
- **problem-diagnosis.md** — When workflows encounter issues

## Related Reference Knowledge

- **reference/domains/13-workflows.md** — Workflow patterns and best practices
- **reference/domains/07-plan-mode.md** — Plan mode capabilities
- **reference/domains/06-memory.md** — Persisting context between sessions
