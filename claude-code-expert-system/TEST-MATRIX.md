# Test Matrix — Claude Code Expert System

Use this matrix to verify retrieval accuracy. Run each query and confirm the expected domain is retrieved and/or web_fetch is triggered appropriately.

---

## Test Summary

| Query Type | Count | Purpose |
|------------|-------|---------|
| Direct Keyword | 6 | Test BM25 keyword matching |
| Semantic Variation | 6 | Test embedding similarity |
| Troubleshooting | 5 | Test error message matching |
| Vague Query | 4 | Test clarification behaviour |
| Out-of-Scope | 4 | Test gap acknowledgment |
| Operational Retrieval | 5 | Test HOW workflow retrieval |
| Combined Retrieval | 5 | Test WHAT + HOW together |
| **Dynamic Retrieval** | **8** | **Test web_fetch triggers** |
| **Total** | **43** | |

---

## Type 1: Direct Keyword Queries (6)

These queries contain exact domain keywords for BM25 matching.

| # | Query | Expected Domain | Pass Criteria | Result |
|---|-------|-----------------|---------------|--------|
| 1 | "plugin install" | 01-plugins.md | Plugin domain retrieved | |
| 2 | "MCP server configuration" | 05-mcp.md | MCP domain retrieved | |
| 3 | "plan mode" | 07-plan-mode.md | Plan mode domain retrieved | |
| 4 | "headless CI/CD" | 08-headless.md | Headless domain retrieved | |
| 5 | "extended thinking budget" | 09-thinking.md | Thinking domain retrieved | |
| 6 | "keyboard shortcuts" | 12-commands-shortcuts.md | Commands domain retrieved | |

---

## Type 2: Semantic Variation Queries (6)

These queries use synonyms or natural language instead of exact terms.

| # | Query | Expected Domain | Pass Criteria | Result |
|---|-------|-----------------|---------------|--------|
| 7 | "add an extension" | 01-plugins.md | Maps to plugin domain | |
| 8 | "run code in background" | 02-subagents.md | Maps to subagents | |
| 9 | "make Claude think harder" | 09-thinking.md | Maps to extended thinking | |
| 10 | "connect external tool" | 05-mcp.md | Maps to MCP | |
| 11 | "save context between sessions" | 06-memory.md | Maps to memory | |
| 12 | "automate on file change" | 04-hooks.md | Maps to hooks | |

---

## Type 3: Troubleshooting Queries (5)

These queries include error symptoms or messages.

| # | Query | Expected Domain | Pass Criteria | Result |
|---|-------|-----------------|---------------|--------|
| 13 | "plugin not loading" | 15-troubleshooting.md | Error section retrieved | |
| 14 | "skill not activating" | 15-troubleshooting.md | Skill troubleshooting | |
| 15 | "MCP connection refused" | 15-troubleshooting.md | MCP errors retrieved | |
| 16 | "context getting lost" | 15-troubleshooting.md | Memory issues | |
| 17 | "hook not firing" | 15-troubleshooting.md | Hook troubleshooting | |

---

## Type 4: Vague Queries (4)

These queries are intentionally vague. Claude should ask for clarification.

| # | Query | Expected Behaviour | Pass Criteria | Result |
|---|-------|-------------------|---------------|--------|
| 18 | "it's not working" | Ask for specifics | Does NOT search blindly | |
| 19 | "help me" | Ask what they need | Requests clarification | |
| 20 | "something's broken" | Ask for component/error | Asks follow-up questions | |
| 21 | "how do I fix this?" | Ask what's wrong | Doesn't guess the issue | |

---

## Type 5: Out-of-Scope Queries (4)

These topics are not covered. Claude should acknowledge the gap.

| # | Query | Expected Behaviour | Pass Criteria | Result |
|---|-------|-------------------|---------------|--------|
| 22 | "how do I use Cursor?" | Acknowledge not covered | Doesn't synthesise from tangential content | |
| 23 | "VS Code extension setup" | Acknowledge not covered | States topic outside scope | |
| 24 | "how do I learn Python?" | Acknowledge not covered | Redirects appropriately | |
| 25 | "compare to GitHub Copilot" | Acknowledge not covered | Admits knowledge gap | |

---

## Type 6: Operational Retrieval Queries (5)

These queries need HOW workflow files.

| # | Query | Expected Domain | Pass Criteria | Result |
|---|-------|-----------------|---------------|--------|
| 26 | "walk me through creating a plugin" | feature-implementation.md | Operational workflow retrieved | |
| 27 | "step by step skill creation" | feature-implementation.md | Step-by-step process | |
| 28 | "help me debug this issue" | problem-diagnosis.md | Diagnosis workflow | |
| 29 | "how do I execute plan-code-commit?" | workflow-execution.md | Workflow execution | |
| 30 | "systematic troubleshooting process" | problem-diagnosis.md | Diagnosis workflow | |

---

## Type 7: Combined Retrieval Queries (5)

These queries need BOTH reference and operational files.

| # | Query | Expected Domains | Pass Criteria | Result |
|---|-------|------------------|---------------|--------|
| 31 | "build a plugin with skills and hooks" | 01-plugins.md + feature-implementation.md | WHAT + HOW both retrieved | |
| 32 | "debug skill activation problem" | 15-troubleshooting.md + problem-diagnosis.md | Reference + operational | |
| 33 | "set up complete development workflow" | 13-workflows.md + workflow-execution.md | Patterns + steps | |
| 34 | "which component should I use and how" | component-comparison.md + feature-implementation.md | Decision + implementation | |
| 35 | "fix MCP and understand how it works" | 05-mcp.md + problem-diagnosis.md | Reference + diagnostic | |

---

## Type 8: Dynamic Retrieval Queries (8) — NEW

These queries should trigger web_fetch from authoritative sources.

| # | Query | Expected Behaviour | Expected URL(s) | Pass Criteria | Result |
|---|-------|-------------------|-----------------|---------------|--------|
| 36 | "What's new in Claude Code?" | web_fetch changelog | https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md | Fetches changelog, summarises changes | |
| 37 | "What's the current syntax for plugin.json?" | web_fetch + knowledge | https://code.claude.com/docs/en/plugins | Fetches docs, provides current syntax | |
| 38 | "Has the hooks API changed recently?" | web_fetch to verify | https://code.claude.com/docs/en/hooks-guide | Fetches current docs, compares | |
| 39 | "Is ultrathink still the only working keyword?" | web_fetch to verify | https://code.claude.com/docs/en/common-workflows | Verifies against current docs | |
| 40 | "What MCP servers are currently available?" | web_fetch directory | https://mcpez.com/claude-code | Fetches MCP directory | |
| 41 | "Show me the official plugin examples" | web_fetch GitHub | https://github.com/anthropics/claude-code/tree/main/plugins | Fetches plugin examples | |
| 42 | "What does the current best practices guide say?" | web_fetch blog | https://www.anthropic.com/engineering/claude-code-best-practices | Fetches engineering blog | |
| 43 | "The documentation says X but I'm seeing Y" | web_fetch to verify | (Relevant domain URL) | Fetches current docs to verify discrepancy | |

---

## Dynamic Retrieval Verification Checklist

For Type 8 queries, verify:

- [ ] Claude identifies currency is critical
- [ ] Claude uses web_fetch (not just web_search)
- [ ] Correct authoritative URL is fetched
- [ ] Content is synthesised with knowledge files
- [ ] Source is cited in response

---

## Domain Coverage Check

Verify all domains have at least 2 test queries:

| Domain | Test Query #s | Count |
|--------|---------------|-------|
| 01-plugins.md | 1, 7, 31, 37, 41 | 5 |
| 02-subagents.md | 8 | 1 |
| 03-skills.md | (via 27, 32) | 2 |
| 04-hooks.md | 12, 17, 38 | 3 |
| 05-mcp.md | 2, 10, 15, 35, 40 | 5 |
| 06-memory.md | 11, 16 | 2 |
| 07-plan-mode.md | 3, 39 | 2 |
| 08-headless.md | 4 | 1 |
| 09-thinking.md | 5, 9, 39 | 3 |
| 10-agent-sdk.md | (covered by subagents) | 1 |
| 11-core-plugins.md | (covered by plugins) | 1 |
| 12-commands-shortcuts.md | 6 | 1 |
| 13-workflows.md | 33, 42 | 2 |
| 14-output-statusline.md | (add if needed) | 0 |
| 15-troubleshooting.md | 13, 14, 15, 16, 17, 32 | 6 |
| feature-implementation.md | 26, 27, 31, 34 | 4 |
| problem-diagnosis.md | 28, 30, 32, 35 | 4 |
| workflow-execution.md | 29, 33 | 2 |
| dynamic-retrieval.md | 36, 37, 38, 39, 40, 41, 42, 43 | 8 |
| component-comparison.md | 34 | 1 |
| source-index.md | (via dynamic queries) | 8 |

---

## Test Execution Notes

- Run tests in a fresh Claude Project to ensure clean retrieval
- Expand `project_knowledge_search` output to verify correct chunks retrieved
- For Type 8 queries, verify `web_fetch` tool call with correct URL
- Mark each test as Pass/Fail in the Result column
- For failures, note which wrong domain was retrieved or if web_fetch wasn't triggered
- Re-run after fixes to confirm resolution
