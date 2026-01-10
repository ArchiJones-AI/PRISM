# Quality Gates — Final Checklist Before Delivery

Is the knowledge system ready for deployment? This reference provides the complete quality checklist. Verify all gates pass before presenting the final deliverable to the user.

Key terms: quality gates, checklist, verification, deployment ready, final review, operational files, reference files, routing coverage

## How to Use This Checklist

Before delivering the final knowledge system:

1. Go through EVERY gate below
2. Mark each as Pass or Fail
3. Fix any failures before delivery
4. Re-verify fixed items
5. Only deliver when ALL gates pass

## Gate 1: Project Instructions Quality

### 1.1 Structure Gates

- [ ] Core purpose present (2-3 sentences maximum)
- [ ] System architecture lists BOTH operational and reference domains
- [ ] Query routing table present with 15-20 CONCRETE mappings
- [ ] Operational retrieval section present
- [ ] Reference retrieval section present
- [ ] Combined retrieval section present
- [ ] Vague query handling section present with explicit "DO NOT search" guidance
- [ ] Knowledge gap handling section present with explicit "DO NOT synthesise" guidance
- [ ] Content index covers ALL domains (operational and reference)

### 1.2 Size Gate

- [ ] Instructions are 4,000-8,000 characters
  - Too short (< 4,000): Insufficient routing guidance
  - Too long (> 8,000): Content belongs in knowledge files, not Instructions

### 1.3 Routing Coverage Gates

- [ ] ALL operational files have routing entries in Instructions
- [ ] ALL reference files have routing entries in Instructions
- [ ] Combined retrieval patterns documented for complex tasks
- [ ] No orphaned files (files exist but not routed to)

### 1.4 Content Quality Gates

- [ ] Query examples are CONCRETE (specific situations → specific files → specific queries)
- [ ] Query examples cover all operational domains
- [ ] Query examples cover all reference domains
- [ ] Content index includes Contains, Retrieve when, Key terms for each domain
- [ ] Instructions read clearly (conductor pattern, not keyword dumps)

## Gate 2: Reference File Quality

### 2.1 Per-File Structure Gates

For EACH reference file, verify:

- [ ] YAML frontmatter present (v3.4):
  - [ ] `domain:` matches filename
  - [ ] `type: reference` set
  - [ ] `semantic_triggers:` includes keywords and variations
  - [ ] `priority_sources:` lists URLs with tier badges
  - [ ] `related_domains:` links to related files
  - [ ] `last_updated:` set to YYYY-MM
- [ ] Opening paragraph contains:
  - [ ] Question echoes
  - [ ] Primary term + variations
  - [ ] Semantic description of topics
  - [ ] Key terms list
- [ ] Authoritative sources section present (v3.4):
  - [ ] Sources ranked by tier (1-4)
  - [ ] Tier 1 = official/source of truth
  - [ ] Precedence rule stated
- [ ] Prompt templates section present (v3.4):
  - [ ] 2-4 ready-to-use prompts
  - [ ] Prompts echo typical user queries
- [ ] Troubleshooting section present (if applicable)
- [ ] Related topics section present
- [ ] File size 2,000-6,000 characters

### 2.2 Chunk Resilience Gates

For EACH reference file, verify:

- [ ] Every PARAGRAPH opens with context-setting sentence
- [ ] No paragraph starts with "To do this..." or similar context-free openings
- [ ] No cross-references ("as mentioned above", "see below")
- [ ] Keywords distributed every 500-800 characters (not front-loaded)
- [ ] Inverted pyramid in every paragraph (answer first)

### 2.3 Hybrid Optimisation Gates

For EACH reference file, verify:

- [ ] Content reads naturally (human would find it clear)
- [ ] Primary domain term appears 2-3 times per section
- [ ] Variations and synonyms included
- [ ] No keyword stuffing (same term 5+ times per section)
- [ ] Question echoes embedded naturally
- [ ] Technical terms present throughout (not generic language only)

### 2.4 Troubleshooting Gates

For troubleshooting sections:

- [ ] Error messages included VERBATIM (exact text users see)
- [ ] Error codes included (401, 403, ERR_*)
- [ ] Each error has cause AND solution
- [ ] Domain terms appear in solutions (not just error descriptions)

## Gate 2A: Operational File Quality

### 2A.1 Per-File Structure Gates

For EACH operational file, verify:

- [ ] YAML frontmatter present (v3.4):
  - [ ] `domain:` matches filename
  - [ ] `type: operational` set
  - [ ] `semantic_triggers:` includes action verbs and process names
  - [ ] `cross_layer_refs:` links to reference domains needed
  - [ ] `related_operational:` links to related operational files
  - [ ] `last_updated:` set to YYYY-MM
- [ ] Opening paragraph contains:
  - [ ] Question echoes ("How do I...?", "What's the process for...?")
  - [ ] Process/workflow name + variations
  - [ ] Description of what this operational file covers
  - [ ] Key terms list (action verbs, phase names)
- [ ] Process steps are clearly numbered or sequenced
- [ ] Decision frameworks have specific criteria tables
- [ ] Related operational/reference domains noted
- [ ] File size 2,000-5,000 characters

### 2A.2 Operational Content Gates

For EACH operational file, verify:

- [ ] Workflows use IMPERATIVE voice ("Do X", not "X is done")
- [ ] Steps are self-contained (context in each step)
- [ ] Decision points have specific criteria (not vague guidance)
- [ ] Sequencing is explicit (Phase 1 before Phase 2)
- [ ] Output/deliverable of each process is stated

### 2A.3 Chunk Resilience Gates (Operational)

For EACH operational file, verify:

- [ ] Every PARAGRAPH opens with context (process name, phase)
- [ ] No step starts with "Next, do..." without context
- [ ] Process names distributed throughout (not just in headings)
- [ ] Keywords appear every 400-600 characters
- [ ] Each step would make sense if retrieved alone

### 2A.4 Cross-Layer Connection Gates

For EACH operational file, verify:

- [ ] References to required reference knowledge are noted
- [ ] All referenced reference files actually exist
- [ ] "Related Reference Knowledge" section present

## Gate 3: Supporting Files Quality

### 3.1 Quick Reference

- [ ] quick-reference.md present
- [ ] YAML frontmatter present (v3.4)
- [ ] Contains most common lookups
- [ ] Organised for fast scanning (tables, not paragraphs)
- [ ] Key terms included for BM25 matching
- [ ] Links to detailed domain files for more info

### 3.2 Terminology Index

- [ ] terminology-index.md present
- [ ] YAML frontmatter present (v3.4)
- [ ] Maps ALL key terms to their domains (operational and reference)
- [ ] Includes variations and synonyms
- [ ] Acronyms have expanded forms
- [ ] Alphabetically organised

### 3.3 Cross-Cutting Files (v3.4)

- [ ] Decision matrix files present (if comparison queries expected)
- [ ] Cross-cutting files placed in supporting/ directory
- [ ] Each cross-cutting file has YAML frontmatter
- [ ] Source authority documented (if source-index pattern used)
- [ ] Total cross-cutting files: 1-3 beyond quick-reference and terminology-index

## Gate 4: Test Matrix Quality

### 4.1 Coverage Gates

- [ ] TEST-MATRIX.md present
- [ ] 30+ test queries included
- [ ] All 7 query types represented:
  - [ ] Direct keyword queries (6+)
  - [ ] Semantic variation queries (6+)
  - [ ] Troubleshooting queries (5+)
  - [ ] Vague queries (4+)
  - [ ] Out-of-scope queries (4+)
  - [ ] Operational retrieval queries (5+)
  - [ ] Combined retrieval queries (4+)
- [ ] All operational domains have at least 2 test queries
- [ ] All reference domains have at least 2 test queries

### 4.2 Quality Gates

- [ ] Expected domain/chunk documented for each query
- [ ] Expected BEHAVIOUR documented for vague/out-of-scope queries
- [ ] Expected operational AND reference retrieval documented for combined queries
- [ ] Results column present for recording test outcomes

## Gate 5: Coverage and Completeness

### 5.1 Source Coverage

- [ ] 100% of source material topics are represented in knowledge files
- [ ] Operational content in operational files (not scattered)
- [ ] Reference content in reference files (not scattered)
- [ ] No significant concepts missing
- [ ] Cross-cutting concepts have clear primary homes

### 5.2 Knowledge Gap Documentation

- [ ] Identified gaps documented
- [ ] Gap handling in Instructions addresses documented gaps
- [ ] Out-of-scope test queries cover documented gaps

## Gate 6: Anti-Pattern Verification

Verify NO anti-patterns present:

### Conductor Anti-Patterns

- [ ] Instructions do NOT contain operational workflows (conductor contains sheet music)
- [ ] Instructions do NOT exceed 8,000 characters
- [ ] No orphaned operational files (operational file exists but not routed to)
- [ ] No orphaned reference files (reference file exists but not routed to)

### Operational File Anti-Patterns

- [ ] No context-free steps ("Next, do..." without stating phase/process)
- [ ] No passive voice workflows ("X is done" instead of "Do X")
- [ ] No vague decision criteria (all decisions have specific criteria)
- [ ] No broken cross-layer connections (referencing non-existent reference files)
- [ ] No operational content in reference files (misclassification)

### Reference File Anti-Patterns

- [ ] No context-free paragraph openings
- [ ] No front-loaded keywords (distributed throughout)
- [ ] No keyword deserts (terms in every section)
- [ ] No sections over 4,000 characters
- [ ] No cross-references ("as mentioned above")
- [ ] No paraphrased errors (all verbatim)
- [ ] No single-term reliance (variations included)
- [ ] No over-optimisation (balanced prose + keywords)

## Gate 7: Deployment Package

### 7.1 File Structure

- [ ] Correct directory structure:
  ```
  {system-name}/
  ├── INSTRUCTIONS.md
  ├── operational/
  │   ├── [workflow-1].md
  │   ├── [workflow-2].md
  │   └── ...
  ├── reference/
  │   ├── [domain-1].md
  │   ├── [domain-2].md
  │   └── ...
  ├── supporting/
  │   ├── quick-reference.md
  │   ├── terminology-index.md
  │   └── [cross-cutting files if applicable]
  ├── TEST-MATRIX.md
  └── README.md
  ```
- [ ] All files have YAML frontmatter (v3.4)

### 7.2 README Quality

- [ ] README.md present
- [ ] Deployment steps documented
- [ ] Debugging workflow included
- [ ] Clear instructions for:
  - [ ] Creating Claude Project
  - [ ] Pasting Instructions
  - [ ] Uploading operational files
  - [ ] Uploading reference files
  - [ ] Running test matrix
  - [ ] Interpreting `project_knowledge_search` output

## Quality Gate Summary

Before delivery, ALL sections must pass:

| Section | Status |
|---------|--------|
| Gate 1: Instructions | |
| Gate 2: Reference Files | |
| Gate 2A: Operational Files | |
| Gate 3: Supporting Files | |
| Gate 4: Test Matrix | |
| Gate 5: Coverage | |
| Gate 6: Anti-Patterns | |
| Gate 7: Package | |

**Only deliver when ALL gates pass**

## Related References

- `anti-patterns.md` — Detailed anti-pattern descriptions
- `phases/phase-6-verification.md` — Testing process
- `templates/operational-domain-template.md` — Operational file template
- `templates/domain-file-template.md` — Reference file template
- `templates/routing-instruction-template.md` — Instructions template
