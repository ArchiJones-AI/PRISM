# Routing Instruction Template — Instructions as Conductor

Use this template for ALL decomposed systems. Instructions route to knowledge; they don't contain all the knowledge. This template creates a slim routing layer that indexes both operational and reference content.

Key terms: routing layer, conductor pattern, content index, query routing, operational retrieval, reference retrieval, slim instructions

---

## Core Principle

**Instructions are a conductor, not a container.**

The conductor:
- Knows what music exists (content index)
- Knows when each section plays (query routing)
- Keeps the orchestra coordinated (edge case handling)
- Doesn't contain every note (that's in the knowledge files)

Target size: 4,000-8,000 characters. If your Instructions exceed 10,000 characters, you haven't decomposed enough.

---

## Template

```markdown
# [System Name]

## Core Purpose

[Irreducible purpose statement — 2-3 sentences maximum. This is the ONE thing that must be immediately present, not retrieved. What does this system fundamentally DO?]

## System Architecture

This project contains [brief description of scope]. Knowledge is organised into:

**Operational Knowledge** — HOW the system works:
[List operational domains with one-line descriptions]

**Reference Knowledge** — WHAT the system applies:
[List reference domains with one-line descriptions]

## Query Routing

When processing user input, retrieve relevant knowledge using these patterns:

### Operational Retrieval

| User Need / Situation | Retrieve | Search Query |
|-----------------------|----------|--------------|
| Starting a new task | [operational domain] | "[domain] process workflow start" |
| Moving to next phase | [operational domain] | "[phase name] phase steps" |
| Making a decision | [operational domain] | "[decision type] framework criteria" |
| Checking constraints | [operational domain] | "[constraint type] rules requirements" |
| Formatting output | [operational domain] | "output format specification structure" |
[Continue for 8-12 operational retrieval patterns]

### Reference Retrieval

| User Need / Situation | Retrieve | Search Query |
|-----------------------|----------|--------------|
| Need methodology for X | [reference domain] | "[methodology] approach method" |
| Looking for example of Y | [reference domain] | "[example type] example template" |
| Avoiding failure mode Z | [reference domain] | "[failure] avoid prevent fix" |
| Defining term W | [reference domain] | "[term] definition meaning" |
| Troubleshooting issue V | [reference domain] | "[issue] troubleshooting error" |
[Continue for 8-12 reference retrieval patterns]

### Combined Retrieval

For complex tasks, retrieve from BOTH layers:

| Task Type | Operational Retrieval | Reference Retrieval |
|-----------|----------------------|---------------------|
| [Complex task 1] | [what operational knowledge] | [what reference knowledge] |
| [Complex task 2] | [what operational knowledge] | [what reference knowledge] |
| [Complex task 3] | [what operational knowledge] | [what reference knowledge] |

## Content Index

### Operational Domains

**[Operational Domain 1]**
- Contains: [Specific content in this domain]
- Retrieve when: [Situations that need this knowledge]
- Key terms: [BM25 search terms]

**[Operational Domain 2]**
- Contains: [Specific content in this domain]
- Retrieve when: [Situations that need this knowledge]
- Key terms: [BM25 search terms]

[Continue for all operational domains...]

### Reference Domains

**[Reference Domain 1]**
- Contains: [Specific content in this domain]
- Retrieve when: [Situations that need this knowledge]
- Key terms: [BM25 search terms]

**[Reference Domain 2]**
- Contains: [Specific content in this domain]
- Retrieve when: [Situations that need this knowledge]
- Key terms: [BM25 search terms]

[Continue for all reference domains...]

## Edge Case Handling

### Vague Input

If user input is too vague to route effectively:
1. DO NOT retrieve broadly hoping for relevance
2. ASK for clarification: [Specific questions for this system]
3. THEN route to appropriate knowledge

### Knowledge Gaps

If retrieved content doesn't address the need:
1. State what specific knowledge wasn't found
2. Indicate whether this is operational or reference gap
3. [Appropriate fallback for this system]
4. DO NOT synthesise from tangentially related content

### Out of Scope

If the request falls outside this system's purpose:
1. State clearly what IS in scope
2. [Appropriate redirect or alternative]
```

---

## Template Sections Explained

### Core Purpose (Required — ~200-400 chars)

The irreducible statement of what the system does. This CANNOT be retrieved; it must be immediately present to orient all subsequent retrieval.

**Good example:**
```markdown
## Core Purpose

Transform raw, unstructured prompt ideas into expertly architected Claude 4 
prompts by applying systematic methodology. Analyse input for explicit and 
implicit requirements, then generate structured XML output optimised for 
Claude's processing.
```

**Bad example (too long, contains retrievable detail):**
```markdown
## Core Purpose

[500 words explaining the phases, the methodology, the output format, 
examples of good prompts, list of principles...]
```

If it's longer than a short paragraph, it belongs in a knowledge file.

### System Architecture (Required — ~300-500 chars)

Brief orientation of what knowledge exists. Not a full index—just enough for Claude to understand the landscape before routing.

### Query Routing (Critical — ~2,000-3,000 chars)

This is the heart of the routing layer. Concrete mappings from situations to retrieval actions.

**Two types of routing:**

1. **Operational retrieval:** When Claude needs to know HOW to proceed
   - "What phase am I in?" → retrieve phase workflow
   - "What are the constraints?" → retrieve constraint rules
   - "How do I structure output?" → retrieve output specification

2. **Reference retrieval:** When Claude needs to know WHAT to apply
   - "What methodology applies here?" → retrieve methodology
   - "What's an example of this?" → retrieve examples
   - "What should I avoid?" → retrieve anti-patterns

### Content Index (Required — ~1,500-2,500 chars)

Detailed map of what each domain contains and when to retrieve it. This is what makes routing precise rather than vague.

**Each domain entry needs:**
- Contains: What's actually in the file
- Retrieve when: Specific situations (not "when relevant")
- Key terms: Actual search terms that match the content

### Edge Case Handling (Required — ~400-600 chars)

What to do when routing fails or input is problematic. Prevents bad retrievals and hallucination.

---

## Size Budget

| Section | Target Size | Purpose |
|---------|-------------|---------|
| Core Purpose | 200-400 chars | Irreducible orientation |
| System Architecture | 300-500 chars | Landscape overview |
| Query Routing | 2,000-3,000 chars | Retrieval mappings |
| Content Index | 1,500-2,500 chars | Domain details |
| Edge Case Handling | 400-600 chars | Failure modes |
| **Total** | **4,400-7,000 chars** | Slim routing layer |

If exceeding 8,000 characters, you're putting content in Instructions that should be in knowledge files.

---

## Common Mistakes

### Mistake: Instructions contain workflows

**Wrong:**
```markdown
## Transformation Process

Phase 1: Intake Analysis
1. Parse the input for explicit requirements
2. Identify implicit assumptions
3. Classify complexity level
4. Document gaps and ambiguities
[...continues for 2,000 characters...]
```

**Right:**
```markdown
## Query Routing — Operational

| Situation | Retrieve | Search Query |
|-----------|----------|--------------|
| Starting new transformation | intake-analysis.md | "intake analysis process parse" |
```

The workflow lives in `operational/intake-analysis.md`, not in Instructions.

### Mistake: Instructions contain methodology

**Wrong:**
```markdown
## XML Blueprint Structure

The three-part XML blueprint consists of:
1. purpose_and_stance — Contains the core purpose and lever configuration...
2. instructions_and_context — Contains the detailed instructions...
3. output_format — Specifies the expected output structure...
[...continues for 1,500 characters...]
```

**Right:**
```markdown
## Query Routing — Reference

| Situation | Retrieve | Search Query |
|-----------|----------|--------------|
| Need output structure | xml-blueprint.md | "XML blueprint structure three-part" |
```

The methodology lives in `reference/xml-blueprint.md`.

### Mistake: Vague routing

**Wrong:**
```markdown
## Query Routing

Search for relevant operational knowledge when needed.
Search for relevant reference knowledge when applicable.
```

**Right:**
```markdown
## Query Routing

| User Need | Retrieve | Search Query |
|-----------|----------|--------------|
| Deciding between Opus and Sonnet | model-selection.md | "Opus Sonnet model selection comparison" |
| Structuring thinking mode | lever-configuration.md | "THINKING_MODE Extended Standard configure" |
```

Routing must be CONCRETE—specific situations mapped to specific retrievals.

---

## Customisation Checklist

When creating Instructions from this template:

- [ ] Core purpose is 2-3 sentences maximum
- [ ] Core purpose contains NO retrievable detail
- [ ] System architecture lists all domains briefly
- [ ] Query routing has 8-12 operational patterns
- [ ] Query routing has 8-12 reference patterns
- [ ] Combined retrieval covers complex multi-domain tasks
- [ ] Content index covers ALL domains
- [ ] Each domain has Contains, Retrieve when, Key terms
- [ ] Edge cases cover vague input, gaps, and out-of-scope
- [ ] Total size is 4,000-8,000 characters
- [ ] No workflows or methodology in Instructions

---

## Related References

- `phase-0-classification.md` — Determining what goes where
- `phase-1-ingestion.md` — Extracting content by type
- `operational-domain-template.md` — Writing operational knowledge files
- `reference-domain-template.md` — Writing reference knowledge files
