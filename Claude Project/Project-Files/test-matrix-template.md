# Test Matrix Template — Verification Queries

Copy and customise this template for TEST-MATRIX.md to verify retrieval accuracy for both operational and reference content.

Key terms: test matrix, verification, operational retrieval, reference retrieval, combined retrieval, testing, query types

---

## Template

```markdown
# Test Matrix: [System Name]

## Instructions for Testing

1. Deploy the knowledge system to a Claude Project
2. For each test query below:
   a. Enter the query in the conversation
   b. BEFORE reading Claude's answer, expand the `project_knowledge_search` tool output
   c. Check: What search query did Claude generate?
   d. Check: What chunks were retrieved?
   e. For combined retrieval: Check if BOTH operational AND reference chunks retrieved
   f. Record results in the table
3. For failed tests, diagnose whether the issue is:
   - Instructions (bad query generation) → Fix INSTRUCTIONS.md
   - Operational files (bad operational chunk matching) → Fix operational files
   - Reference files (bad reference chunk matching) → Fix reference files
4. Re-test after fixes until all tests pass

## Results Summary

| Query Type | Total | Passed | Failed | Pass Rate |
|------------|-------|--------|--------|-----------|
| Direct Keyword | 6 | | | |
| Semantic Variation | 6 | | | |
| Troubleshooting | 5 | | | |
| Vague | 4 | | | |
| Out-of-Scope | 4 | | | |
| Operational Retrieval | 5 | | | |
| Combined Retrieval | 4 | | | |
| **TOTAL** | **34** | | | |

**Target Pass Rates:**
- Direct Keyword: 95%+
- Semantic Variation: 85%+
- Troubleshooting: 95%+
- Vague Queries: 90%+
- Out-of-Scope: 90%+
- Operational Retrieval: 90%+
- Combined Retrieval: 85%+

---

## Type 1: Direct Keyword Queries

Test BM25 matching on exact terminology in reference content.

| # | Query | Expected Domain | Expected Chunk | Query Generated | Chunks Retrieved | Pass/Fail | Notes |
|---|-------|-----------------|----------------|-----------------|------------------|-----------|-------|
| 1 | "[Query with exact term from domain 1]" | [domain-1] | [specific section] | | | | |
| 2 | "[Query with exact term from domain 2]" | [domain-2] | [specific section] | | | | |
| 3 | "[Query with exact term from domain 3]" | [domain-3] | [specific section] | | | | |
| 4 | "[Query with specific feature name]" | [domain] | [section] | | | | |
| 5 | "[Query with configuration term]" | [domain] | [section] | | | | |
| 6 | "[Query with exact command/setting]" | [domain] | [section] | | | | |

---

## Type 2: Semantic Variation Queries

Test embedding similarity without exact keywords.

| # | Query | Expected Domain | Expected Chunk | Query Generated | Chunks Retrieved | Pass/Fail | Notes |
|---|-------|-----------------|----------------|-----------------|------------------|-----------|-------|
| 1 | "[Natural language without exact terms]" | [domain] | [section] | | | | |
| 2 | "[Synonym-based question]" | [domain] | [section] | | | | |
| 3 | "[User-friendly phrasing]" | [domain] | [section] | | | | |
| 4 | "[Different wording for same concept]" | [domain] | [section] | | | | |
| 5 | "[Casual description of feature]" | [domain] | [section] | | | | |
| 6 | "[Indirect reference to topic]" | [domain] | [section] | | | | |

---

## Type 3: Troubleshooting Queries

Test BM25 matching on verbatim error text.

| # | Query | Expected Domain | Expected Chunk | Query Generated | Chunks Retrieved | Pass/Fail | Notes |
|---|-------|-----------------|----------------|-----------------|------------------|-----------|-------|
| 1 | "[Exact error message from domain]" | [domain] | troubleshooting section | | | | |
| 2 | "[Error code] error" | [domain] | troubleshooting section | | | | |
| 3 | "Getting [exact error text]" | [domain] | troubleshooting section | | | | |
| 4 | "[Symptom description with error]" | [domain] | troubleshooting section | | | | |
| 5 | "Why am I seeing [error]?" | [domain] | troubleshooting section | | | | |

---

## Type 4: Vague Queries

Test that Claude asks for clarification instead of searching.

| # | Query | Expected Behaviour | Actual Behaviour | Pass/Fail | Notes |
|---|-------|-------------------|------------------|-----------|-------|
| 1 | "It's not working" | Ask for clarification | | | |
| 2 | "Help me" | Ask for clarification | | | |
| 3 | "There's an error" | Ask for clarification | | | |
| 4 | "What's wrong?" | Ask for clarification | | | |

**Pass criteria:** Claude asks what component/feature and what symptoms, does NOT search with vague terms.

**Fail criteria:** Claude searches with terms like "not working" or "error" and retrieves irrelevant chunks.

---

## Type 5: Out-of-Scope Queries

Test that Claude acknowledges gaps instead of synthesising.

| # | Query | Expected Behaviour | Actual Behaviour | Pass/Fail | Notes |
|---|-------|-------------------|------------------|-----------|-------|
| 1 | "[Topic not in knowledge base]" | Acknowledge not covered | | | |
| 2 | "[Known gap from analysis]" | Acknowledge not covered | | | |
| 3 | "[Tangentially related topic]" | Acknowledge not covered | | | |
| 4 | "[Edge case not documented]" | Acknowledge not covered | | | |

**Pass criteria:** Claude states the topic isn't covered, offers general knowledge with caveats.

**Fail criteria:** Claude synthesises a confident answer from tangentially related chunks.

---

## Type 6: Operational Retrieval Queries

Test retrieval of operational workflow content.

| # | Query | Expected Operational File | Expected Chunk | Query Generated | Chunks Retrieved | Pass/Fail | Notes |
|---|-------|--------------------------|----------------|-----------------|------------------|-----------|-------|
| 1 | "What's the process for [phase 1]?" | [operational-file-1].md | process steps section | | | | |
| 2 | "How do I [action from phase 2]?" | [operational-file-2].md | workflow section | | | | |
| 3 | "What are the steps for [phase 3]?" | [operational-file-3].md | steps section | | | | |
| 4 | "What constraints apply to [output]?" | [constraints].md | constraints section | | | | |
| 5 | "How do I make decisions about [X]?" | [decision-file].md | decision framework | | | | |

**Pass criteria:** Retrieve chunks from operational files containing imperative instructions (do X, then Y).

**Fail criteria:** Retrieve reference content instead of operational workflow, or retrieve no relevant chunks.

**Diagnosis if failing:**
- Check operational file has question echoes for these queries
- Check process/workflow terms distributed throughout
- Check Instructions have operational retrieval routing entries
- Verify operational file exists and has routing entry (not orphaned)

---

## Type 7: Combined Retrieval Queries

Test complex tasks requiring BOTH operational AND reference knowledge.

| # | Query | Expected Operational | Expected Reference | Operational Retrieved? | Reference Retrieved? | Pass/Fail | Notes |
|---|-------|---------------------|-------------------|----------------------|---------------------|-----------|-------|
| 1 | "[Full task description 1]" | [operational-file].md | [reference-file].md | | | | |
| 2 | "[Full task description 2]" | [operational-file].md | [reference-file].md | | | | |
| 3 | "[Task needing workflow + methodology]" | [workflow].md | [methodology].md | | | | |
| 4 | "[Task needing process + examples]" | [process].md | [examples].md | | | | |

**Pass criteria:** Retrieve chunks from BOTH operational AND reference files.

**Fail criteria:** Retrieve from only one layer, or retrieve irrelevant chunks from either layer.

**Diagnosis if failing:**
- Check Instructions have combined retrieval patterns
- Test operational and reference queries separately to isolate issue
- Verify both files have terms that would match a combined query
- Check if search query includes terms from both layers

---

## Failure Diagnosis Guide

### If Direct Keyword queries fail:

**Check:** Does the expected chunk contain the query terms?
- If NO → Add keywords to reference file
- If YES → Check if another chunk is ranking higher (adjust keyword density)

### If Semantic Variation queries fail:

**Check:** Does the expected chunk contain synonyms/variations?
- If NO → Add semantic hedging (synonyms, question echoes)
- If YES → Check embedding phrasing; add more question-form echoes

### If Troubleshooting queries fail:

**Check:** Is the error message VERBATIM in the reference file?
- If NO → Add exact error text
- If YES → Check keyword distribution in troubleshooting section

### If Vague queries fail:

**Check:** Did Claude search instead of asking for clarification?
- If YES → Strengthen vague query handling in Instructions
- Add more explicit examples of vague patterns

### If Out-of-Scope queries fail:

**Check:** Did Claude synthesise from irrelevant chunks?
- If YES → Strengthen knowledge gap handling in Instructions
- Add more explicit "DO NOT synthesise" language

### If Operational Retrieval queries fail:

**Check:** Did Claude retrieve reference content instead of operational?
- Check operational file has question echoes
- Check process terms distributed throughout
- Check Instructions have operational routing entries
- Verify file isn't orphaned

### If Combined Retrieval queries fail:

**Check:** Did Claude retrieve from only one layer?
- Test each layer separately to isolate issue
- Check Instructions have combined retrieval patterns
- Verify search query includes terms from both layers
- Add cross-layer terms if one layer's content is weak

---

## Re-Test Log

Record iterations here:

| Date | Changes Made | Queries Re-tested | Results |
|------|--------------|-------------------|---------|
| | | | |
| | | | |
| | | | |
```

---

## Customisation Checklist

When creating the test matrix:

**Reference Content Tests:**
- [ ] Add 6+ direct keyword queries covering all reference domains
- [ ] Add 6+ semantic variation queries testing synonyms/natural language
- [ ] Add 5+ troubleshooting queries with exact error text

**Behaviour Tests:**
- [ ] Add 4+ vague queries testing clarification handling
- [ ] Add 4+ out-of-scope queries testing gap handling

**Operational Content Tests:**
- [ ] Add 5+ operational retrieval queries covering all operational domains
- [ ] Queries ask about processes, workflows, decision frameworks
- [ ] Expected operational file noted for each query

**Combined Retrieval Tests:**
- [ ] Add 4+ combined retrieval queries for complex tasks
- [ ] Expected operational AND reference files noted for each query
- [ ] Queries represent realistic task descriptions

**General:**
- [ ] Replace [System Name] throughout
- [ ] Total at least 34 queries (minimum 30)
- [ ] Expected domain/chunk filled for types 1-3, 6
- [ ] Expected behaviour filled for types 4-5
- [ ] Expected both layers filled for type 7

---

## Related References

- `phases/phase-6-verification.md` — Full verification process
- `quality-gates.md` — Quality requirements including test coverage
- `anti-patterns.md` — Common issues to check for
