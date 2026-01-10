# Phase 6: Verification — Testing Retrieval Accuracy

How do I verify the knowledge system works correctly? This reference covers Phase 6: testing retrieval accuracy for BOTH operational and reference content using `project_knowledge_search` output, debugging retrieval failures, and iterating until accuracy is acceptable.

Key terms: verification, testing, project_knowledge_search, debugging, retrieval accuracy, test matrix, observability, operational retrieval, combined retrieval

## Phase 6 Overview

Verification is your feedback loop. You can OBSERVE what chunks are retrieved by expanding the `project_knowledge_search` tool output. Use this observability to debug and improve.

For three-layer systems, you must test:
- **Reference retrieval:** Can Claude find knowledge content?
- **Operational retrieval:** Can Claude find workflow and process content?
- **Combined retrieval:** Can Claude find BOTH for complex tasks?

**Deliverable:** TEST-MATRIX.md with results, and any iterations needed.

## The Debugging Workflow

### Step 1: Ask a Test Query

Enter a test query in the Claude Project conversation.

### Step 2: Expand Tool Output FIRST

BEFORE reading Claude's answer, expand the `project_knowledge_search` tool call output. This shows:
- The search query Claude generated
- The chunks that were retrieved
- The relevance scores (if shown)

### Step 3: Evaluate Retrieval

Ask yourself:
- Did Claude generate a good search query?
- Were the expected chunks retrieved?
- For combined retrieval: Were BOTH operational AND reference chunks retrieved?
- Were unexpected/irrelevant chunks retrieved?

### Step 4: Diagnose Problems

If retrieval was wrong:

**Bad query generation → Adjust Instructions**
- Query was too vague
- Query missed key terms
- Query didn't include operational OR reference keywords
- Combined retrieval only searched one layer

**Bad chunk matching → Adjust Domain Files**
- Expected chunk lacked keywords
- Content didn't match semantic phrasing
- Keywords were front-loaded, not distributed
- Operational file lacked process terms

### Step 5: Iterate

Make adjustments and re-test until retrieval is accurate.

## Test Query Types

Create test queries across 7 categories:

### Type 1: Direct Keyword Queries (Reference)

Use exact terminology from your reference domains.

**Purpose:** Tests BM25 matching on specific terms in reference content.

**Examples:**
- "How do I configure OAuth?"
- "401 Unauthorized error"
- "SSO setup"

**Expected:** Retrieve reference chunks containing those exact terms.

### Type 2: Semantic Variation Queries (Reference)

Use natural language WITHOUT exact keywords.

**Purpose:** Tests embedding similarity and semantic hedging in reference content.

**Examples:**
- "Let users sign in with their Google account" (tests OAuth without saying OAuth)
- "My login keeps timing out" (tests session without saying session)
- "Connect to external authentication" (tests IdP/SSO without exact terms)

**Expected:** Retrieve semantically related reference chunks despite different wording.

### Type 3: Troubleshooting Queries

Include error messages or symptoms.

**Purpose:** Tests BM25 on verbatim error text.

**Examples:**
- "Getting error: invalid_client"
- "401 Unauthorized - Invalid token"
- "redirect_uri_mismatch"

**Expected:** Retrieve troubleshooting chunks with exact error matches.

### Type 4: Vague Queries

Deliberately vague, underspecified questions.

**Purpose:** Tests vague query handling in Instructions.

**Examples:**
- "It's not working"
- "Help me with authentication"
- "There's an error"
- "What's wrong?"

**Expected:** Claude should ask for clarification, NOT search with vague terms. If Claude searches and retrieves irrelevant chunks, strengthen vague query handling instructions.

### Type 5: Out-of-Scope Queries

Questions NOT covered in the knowledge base.

**Purpose:** Tests knowledge gap handling.

**Examples:**
- Topics explicitly not in your domains
- Edge cases you identified as gaps
- Tangentially related but uncovered topics

**Expected:** Claude should acknowledge the gap, NOT synthesise from tangentially related chunks. If Claude answers confidently using irrelevant chunks, strengthen gap handling instructions.

### Type 6: Operational Retrieval Queries

Questions about HOW to perform processes and workflows.

**Purpose:** Tests retrieval of operational knowledge files.

**Examples:**
- "What's the process for intake analysis?"
- "How do I handle the transformation phase?"
- "What are the steps for output generation?"
- "What constraints apply to the output?"
- "How do I classify complexity?"

**Expected:** Retrieve operational workflow chunks, NOT reference chunks. The retrieved content should include imperative instructions (do X, then Y).

**Diagnosis if failing:**
- Check operational file has question echoes
- Check process terms are distributed throughout
- Check Instructions have operational routing entries
- Verify operational file exists and isn't orphaned

### Type 7: Combined Retrieval Queries

Complex task descriptions requiring BOTH operational AND reference knowledge.

**Purpose:** Tests that complex tasks retrieve from both layers.

**Examples:**
- "Transform this prompt using the methodology" → needs operational workflow + reference methodology
- "Analyse this input and generate structured output" → needs operational phases + reference format specs
- "Review the output for common failures" → needs operational review process + reference failure patterns

**Expected:** Retrieve chunks from BOTH operational AND reference files. If only one layer is retrieved, the task can't be completed fully.

**Diagnosis if failing:**
- Check Instructions have combined retrieval patterns
- Verify query routing covers both layers for this task type
- Check that search query includes terms from both layers
- Test operational and reference queries separately to isolate the issue

---

## Test Matrix Template

Create a test matrix with 30+ queries across all types:

```markdown
# Test Matrix: [System Name]

## Test Results Summary

| Type | Queries Tested | Passed | Failed |
|------|----------------|--------|--------|
| Direct Keyword | 6 | | |
| Semantic Variation | 6 | | |
| Troubleshooting | 5 | | |
| Vague | 4 | | |
| Out-of-Scope | 4 | | |
| Operational Retrieval | 5 | | |
| Combined Retrieval | 4 | | |
| **Total** | **34** | | |

## Detailed Test Results

### Direct Keyword Queries

| Query | Expected Domain | Expected Chunk | Search Query Generated | Chunks Retrieved | Pass/Fail | Notes |
|-------|-----------------|----------------|------------------------|------------------|-----------|-------|
| "Configure OAuth" | oauth-sso | OAuth setup section | | | | |
| "401 Unauthorized" | troubleshooting | 401 error section | | | | |
| [Continue...] | | | | | | |

### Semantic Variation Queries

| Query | Expected Domain | Expected Chunk | Search Query Generated | Chunks Retrieved | Pass/Fail | Notes |
|-------|-----------------|----------------|------------------------|------------------|-----------|-------|
| "Sign in with Google account" | oauth-sso | OAuth setup | | | | |
| [Continue...] | | | | | | |

### Troubleshooting Queries

| Query | Expected Domain | Expected Chunk | Search Query Generated | Chunks Retrieved | Pass/Fail | Notes |
|-------|-----------------|----------------|------------------------|------------------|-----------|-------|
| "error: invalid_client" | oauth-sso | OAuth troubleshooting | | | | |
| [Continue...] | | | | | | |

### Vague Queries

| Query | Expected Behaviour | Actual Behaviour | Pass/Fail | Notes |
|-------|-------------------|------------------|-----------|-------|
| "It's not working" | Ask for clarification | | | |
| [Continue...] | | | | |

### Out-of-Scope Queries

| Query | Expected Behaviour | Actual Behaviour | Pass/Fail | Notes |
|-------|-------------------|------------------|-----------|-------|
| "[Uncovered topic]" | Acknowledge gap | | | |
| [Continue...] | | | | |

### Operational Retrieval Queries

| Query | Expected Operational File | Expected Chunk | Search Query Generated | Chunks Retrieved | Pass/Fail | Notes |
|-------|--------------------------|----------------|------------------------|------------------|-----------|-------|
| "What's the intake process?" | intake-analysis.md | Process steps | | | | |
| "How do I classify complexity?" | intake-analysis.md | Complexity framework | | | | |
| [Continue...] | | | | | | |

### Combined Retrieval Queries

| Query | Expected Operational | Expected Reference | Operational Retrieved? | Reference Retrieved? | Pass/Fail | Notes |
|-------|---------------------|-------------------|----------------------|---------------------|-----------|-------|
| "Transform this prompt" | transformation-process.md | xml-blueprint.md | | | | |
| "Review for failures" | output-generation.md | common-failures.md | | | | |
| [Continue...] | | | | | | |
```

## Diagnosing Common Failures

### Failure: Wrong chunks retrieved

**Symptom:** Query retrieves chunks from wrong domain.

**Diagnosis:**
1. Check the search query Claude generated — was it good?
2. If query was bad → Adjust Instructions (add better examples)
3. If query was good but wrong chunks matched → Adjust domain files (add keywords to correct chunks, make wrong chunks less matching)

### Failure: No relevant chunks retrieved

**Symptom:** Query returns chunks that don't answer the question.

**Diagnosis:**
1. Check if expected content exists in any domain
2. Check if expected content has the right keywords
3. Check if semantic phrasing matches
4. Add keywords or semantic variations to expected content

### Failure: Vague query not handled

**Symptom:** Claude searches with vague terms instead of asking for clarification.

**Diagnosis:**
- Vague query handling instructions not strong enough
- Add more explicit examples of vague patterns
- Strengthen "DO NOT search" language

### Failure: Out-of-scope query not acknowledged

**Symptom:** Claude synthesises answer from tangentially related chunks.

**Diagnosis:**
- Knowledge gap handling instructions not strong enough
- Add more explicit "DO NOT synthesise" language
- Add examples of when to acknowledge gaps

### Failure: Operational chunks not retrieved

**Symptom:** Query about process/workflow retrieves reference content instead.

**Diagnosis:**
1. Check operational file exists and has question echoes
2. Check process terms distributed throughout operational file
3. Check Instructions have operational retrieval routing
4. Verify operational file isn't orphaned (has routing entry)
5. Add more process-specific keywords to operational file

### Failure: Combined retrieval incomplete

**Symptom:** Complex task retrieves from only one layer (operational OR reference, not both).

**Diagnosis:**
1. Check Instructions have combined retrieval patterns for this task type
2. Verify search query includes terms from both layers
3. Test operational and reference queries separately
4. If one layer works alone, the other layer's content may lack keywords
5. Add cross-layer terms to the weaker content

## Iteration Protocol

After each test batch:

1. **Record results** in test matrix
2. **Identify patterns** in failures
3. **Prioritise fixes:**
   - Fix Instructions for query generation issues
   - Fix operational files for operational retrieval issues
   - Fix reference files for reference retrieval issues
4. **Make targeted changes** (don't change everything at once)
5. **Re-test failed queries** to verify fix
6. **Re-test passing queries** to verify no regression

## Acceptable Accuracy Thresholds

| Query Type | Target Pass Rate |
|------------|-----------------|
| Direct Keyword | 95%+ |
| Semantic Variation | 85%+ |
| Troubleshooting | 95%+ |
| Vague Queries | 90%+ |
| Out-of-Scope | 90%+ |
| Operational Retrieval | 90%+ |
| Combined Retrieval | 85%+ |

If below thresholds, continue iterating.

## Phase 6 Checklist

- [ ] Test matrix created with 30+ queries
- [ ] All 7 query types represented
- [ ] Operational retrieval tested (minimum 5 queries)
- [ ] Combined retrieval tested (minimum 4 queries)
- [ ] Each query tested and results recorded
- [ ] Failures diagnosed (Instructions vs operational vs reference files)
- [ ] Targeted fixes made
- [ ] Re-tested until thresholds met
- [ ] Final test matrix documents results

## Next Phase

After verification passes, proceed to **Phase 7: Package** — Final delivery.

## Related References

- `templates/test-matrix-template.md` — Test matrix template
- `phases/phase-3-instructions.md` — Fixing query generation issues
- `phases/phase-4-domain-files.md` — Fixing reference chunk matching issues
- `templates/operational-domain-template.md` — Fixing operational chunk matching issues
- `anti-patterns.md` — Common issues to check for
