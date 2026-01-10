# Phase 1: Ingestion — Extracting Content by Layer

How do I extract knowledge from source material? This reference covers Phase 1: reading source material and extracting content into OPERATIONAL and REFERENCE categories based on Phase 0 classification.

Key terms: ingestion, extraction, operational content, reference content, layer separation, terminology, retrieval patterns, cross-layer connections

---

## Phase 1 Overview

Phase 1 extracts and categorises ALL content from source material into two layers:
- **Operational:** HOW the system works (phases, workflows, decisions)
- **Reference:** WHAT the system applies (methodology, examples, patterns)

**Prerequisite:** Complete Phase 0 classification with proposed architecture.

**Deliverable:** Knowledge inventory with content separated by layer and tagged for specific domains.

---

## Step 1: Read All Source Material

Read the complete source material before extracting. This includes:
- Primary system prompt
- Attached methodology documents
- Examples and templates
- Any supplementary files

**Goal:** Understand the complete scope before separation.

---

## Step 2: Extract Operational Content

Operational content tells Claude HOW to perform the task.

### What Counts as Operational

| Content Type | Examples | Why It's Operational |
|--------------|----------|---------------------|
| Process phases | "Phase 1: Intake, Phase 2: Analysis" | Defines execution sequence |
| Workflow steps | "First parse, then classify, then generate" | Direct action instructions |
| Decision frameworks | "If complex, use Extended; if simple, use Standard" | Execution logic |
| Principles | "Always validate before output" | Behavioural rules |
| Constraints | "Never exceed 8,000 characters" | Boundaries |
| Output specs | "Use XML with three sections" | Format requirements |

### Extraction Process

For each operational element:

1. **Identify the process/framework**
   - What is this telling Claude to DO?
   - Is this a sequence, a decision, or a rule?

2. **Tag with proposed domain**
   - Which operational domain (from Phase 0) does this belong to?
   - Does it span multiple domains?

3. **Note dependencies**
   - Does this step require output from another step?
   - Does this decision need reference knowledge to execute?

4. **Document verbatim or summarised**
   - If process is well-written, extract verbatim
   - If scattered, summarise and consolidate

### Operational Extraction Template

```markdown
## Operational Content: [Source Name]

### Domain: [Proposed Operational Domain]

**Content Type:** [Phase/Workflow/Decision/Principle/Constraint/Output Spec]

**Source Location:** [Where in source material]

**Extracted Content:**
[Verbatim or summarised content]

**Dependencies:**
- Requires: [Prior operational steps]
- Needs reference: [Reference domains needed to execute]

**Notes:**
[Any observations about chunking, overlap, gaps]
```

---

## Step 3: Extract Reference Content

Reference content tells Claude WHAT knowledge to apply.

### What Counts as Reference

| Content Type | Examples | Why It's Reference |
|--------------|----------|-------------------|
| Methodology | "The XML blueprint has three parts..." | Explains approach |
| Examples | "Here's a well-structured prompt..." | Shows what good looks like |
| Patterns | "Use semantic hedging to improve retrieval" | Named techniques |
| Anti-patterns | "Avoid front-loaded keywords" | What NOT to do |
| Definitions | "THINKING_MODE controls reasoning depth" | Terminology |
| Troubleshooting | "If retrieval fails, check keyword distribution" | Problem-solving |

### Extraction Process

For each reference element:

1. **Identify the knowledge type**
   - Is this explaining, exemplifying, or defining?
   - Is this positive guidance or negative (anti-pattern)?

2. **Tag with proposed domain**
   - Which reference domain (from Phase 0) does this belong to?
   - Is this a cross-cutting concept?

3. **Note operational connections**
   - Which operational processes need this knowledge?
   - When would this knowledge be retrieved?

4. **Extract with semantic hedging prep**
   - Note primary terms and variations
   - Note question forms users might ask

### Reference Extraction Template

```markdown
## Reference Content: [Source Name]

### Domain: [Proposed Reference Domain]

**Content Type:** [Methodology/Example/Pattern/Anti-pattern/Definition/Troubleshooting]

**Source Location:** [Where in source material]

**Extracted Content:**
[Verbatim or summarised content]

**Terminology:**
- Primary term: [Main term]
- Variations: [Synonyms, abbreviations, alternate phrasings]

**Question Forms:**
- "[How would users ask about this?]"
- "[Alternative question phrasing?]"

**Operational Connections:**
- Used by: [Which operational processes need this]
- Retrieved when: [Specific triggers]

**Notes:**
[Observations about chunking, examples needed, gaps]
```

---

## Step 4: Build Terminology Index

Extract ALL domain-specific vocabulary for BM25 optimisation.

### From Operational Content

| Term Type | Examples |
|-----------|----------|
| Phase names | Intake, Analysis, Generation |
| Action verbs | Parse, classify, transform, validate |
| Framework names | Complexity Framework, Decision Matrix |
| Constraint terms | Required, must, never, always |

### From Reference Content

| Term Type | Examples |
|-----------|----------|
| Methodology terms | XML blueprint, semantic hedging, rank fusion |
| Technical terms | BM25, embedding, chunk boundary |
| Pattern names | Inverted pyramid, distributed keywords |
| Anti-pattern names | Front-loading, keyword desert |

### Terminology Map Format

```markdown
## Terminology Index

### Operational Terms

| Term | Variations | Domain | Used In |
|------|------------|--------|---------|
| [term] | [variations] | [operational domain] | [which processes] |

### Reference Terms

| Term | Variations | Domain | Retrieved When |
|------|------------|--------|----------------|
| [term] | [variations] | [reference domain] | [operational triggers] |
```

---

## Step 5: Map Cross-Layer Connections

Identify where operational processes need reference knowledge.

This mapping becomes the query routing in Instructions.

### Connection Mapping Template

```markdown
## Cross-Layer Connections

| Operational Process | Reference Need | Reference Domain | Retrieval Trigger |
|--------------------|----------------|------------------|-------------------|
| [Process step] | [What knowledge needed] | [Domain] | "[Search query]" |
| Intake: Classify complexity | Complexity criteria | lever-configuration | "complexity THINKING_MODE" |
| Generation: Structure output | Blueprint format | xml-blueprint | "XML structure three-part" |
```

---

## Step 6: Identify Gaps and Issues

### Knowledge Gaps

Topics that should exist but don't:
- Expected reference content not in source
- Operational steps that lack methodology
- Edge cases not documented

### Chunking Issues

Content that may need restructuring:
- Very long sections (>5,000 chars)
- Content with internal cross-references
- Scattered information that should be consolidated

### Overlap Issues

Content that appears in multiple places:
- Determine canonical location
- Note what other locations should reference

---

## Phase 1 Deliverable: Knowledge Inventory

Compile extraction into structured inventory:

```markdown
# Knowledge Inventory: [System Name]

## Source Summary

- Total source size: [X] characters
- Operational content: ~[Y] characters ([Z]%)
- Reference content: ~[W] characters ([V]%)

## Operational Content Extracted

### Domain: [Operational Domain 1]
[List extracted content with templates above]

### Domain: [Operational Domain 2]
[Continue for each operational domain]

## Reference Content Extracted

### Domain: [Reference Domain 1]
[List extracted content with templates above]

### Domain: [Reference Domain 2]
[Continue for each reference domain]

## Terminology Index
[Complete terminology map]

## Cross-Layer Connections
[Connection mapping table]

## Gaps and Issues
- [Gap 1]
- [Issue 1]
- [Overlap 1]

## Ready for Phase 2
- [ ] All operational content extracted and tagged
- [ ] All reference content extracted and tagged
- [ ] Terminology index complete
- [ ] Cross-layer connections mapped
- [ ] Gaps documented
```

---

## Phase 1 Checklist

Before proceeding to Phase 2:

- [ ] All source material read completely
- [ ] Operational content identified and extracted
- [ ] Reference content identified and extracted
- [ ] Each extraction tagged with proposed domain
- [ ] Dependencies noted (operational-to-operational)
- [ ] Connections mapped (operational-to-reference)
- [ ] Terminology index built with variations
- [ ] Gaps and issues documented
- [ ] Knowledge inventory document created

---

## Next Phase

Proceed to **Phase 2: Architecture** — Finalise domain structure and get user approval.

---

## Related References

- `phase-0-classification.md` — Prerequisite: proposed architecture
- `phase-2-architecture.md` — Using extraction for domain design
- `templates/operational-domain-template.md` — How extracted operational content becomes files
- `templates/domain-file-template.md` — How extracted reference content becomes files
