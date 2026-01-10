# Phase 3: Create Instructions — The Routing Layer

How do I write effective Project Instructions? This reference covers Phase 3: creating Instructions that act as a CONDUCTOR, routing to both operational and reference knowledge files through concrete query examples and edge case handling.

Key terms: Project Instructions, routing layer, conductor pattern, query routing, operational retrieval, reference retrieval, combined retrieval, query biasing

## Phase 3 Overview

Instructions are the routing layer of your knowledge system. They tell Claude WHAT knowledge exists and WHEN to retrieve it. Instructions do NOT contain the knowledge itself—that lives in operational and reference files.

**Deliverable:** INSTRUCTIONS.md ready to paste into Project Instructions field.

**Target size:** 4,000-8,000 characters. If longer, you're putting content in Instructions that should be in knowledge files.

**Template:** Use `templates/routing-instruction-template.md` for the full template.

---

## The Conductor Pattern

**Instructions are a conductor, not a container.**

The conductor of an orchestra:
- Knows what music exists (content index)
- Knows when each section plays (query routing)
- Keeps the orchestra coordinated (edge case handling)
- Does NOT contain every note (that's in the knowledge files)

### What Instructions MUST Contain

| Component | Target Size | Purpose |
|-----------|-------------|---------|
| Core Purpose | 200-400 chars | Irreducible statement of what system does |
| System Architecture | 300-500 chars | Overview of what knowledge exists |
| Query Routing | 2,000-3,000 chars | Concrete retrieval mappings |
| Content Index | 1,500-2,500 chars | Domain details for precise routing |
| Edge Case Handling | 400-600 chars | Vague input, gaps, out-of-scope |

### What Instructions Should NOT Contain

- Detailed phase-by-phase workflows → put in operational files
- Complete decision frameworks → put in operational files
- Methodology explanations → put in reference files
- Examples and templates → put in reference files
- Extensive constraints lists → put in operational files

**Rule of thumb:** If it's more than a few sentences, it's a knowledge file, not an instruction.

---

## Instruction Components

### Component 1: Core Purpose

The irreducible statement of what this system fundamentally DOES. This cannot be retrieved; it must be immediately present.

```markdown
## Core Purpose

Transform raw, unstructured prompt ideas into expertly architected Claude 4
prompts by applying systematic methodology. Analyse input for explicit and
implicit requirements, then generate structured XML output optimised for
Claude's processing.
```

**Too long?** If your purpose statement exceeds a short paragraph, move detail to operational files.

### Component 2: System Architecture

Brief orientation of what knowledge exists—not a full index, just enough for Claude to understand the landscape.

```markdown
## System Architecture

This project contains expertise for prompt transformation. Knowledge is organised into:

**Operational Knowledge** — HOW the system works:
- Intake analysis process
- Transformation workflow
- Output generation

**Reference Knowledge** — WHAT the system applies:
- XML blueprint methodology
- Lever configuration
- Model selection guidance
- Common failures to avoid
```

### Component 3: Query Routing

CRITICAL component. Concrete mappings from situations to retrieval actions. Include 15-20 specific examples across three categories.

#### Operational Retrieval

When Claude needs to know HOW to proceed:

```markdown
### Operational Retrieval

| User Need / Situation | Retrieve | Search Query |
|-----------------------|----------|--------------|
| Starting new transformation | intake-analysis.md | "intake analysis process parse" |
| Moving to transformation phase | transformation-process.md | "transformation phase workflow" |
| Generating final output | output-generation.md | "output generation format structure" |
| Making complexity decision | intake-analysis.md | "complexity classification framework" |
| Checking constraints | constraints.md | "constraints rules requirements" |
```

#### Reference Retrieval

When Claude needs to know WHAT knowledge to apply:

```markdown
### Reference Retrieval

| User Need / Situation | Retrieve | Search Query |
|-----------------------|----------|--------------|
| Need output structure | xml-blueprint.md | "XML blueprint structure three-part" |
| Configuring settings | lever-configuration.md | "THINKING_MODE INTERACTION_STANCE" |
| Choosing model | model-selection.md | "Opus Sonnet model selection" |
| Avoiding failures | common-failures.md | "whiff failure avoid common" |
| Finding example | prompt-examples.md | "example prompt template" |
```

#### Combined Retrieval

For complex tasks requiring BOTH layers:

```markdown
### Combined Retrieval

| Complex Task | Operational Retrieval | Reference Retrieval |
|--------------|----------------------|---------------------|
| Full prompt transformation | intake-analysis.md + transformation-process.md | xml-blueprint.md + lever-configuration.md |
| Handling vague input | intake-analysis.md | common-failures.md |
| Quality review | output-generation.md | xml-blueprint.md + common-failures.md |
```

### Component 4: Content Index

Detailed map of what each domain contains and when to retrieve it.

```markdown
## Content Index

### Operational Domains

**intake-analysis.md**
- Contains: Input parsing, complexity classification, gap identification
- Retrieve when: Starting new transformation, classifying input
- Key terms: intake, analysis, parse, complexity, classify, gaps

**transformation-process.md**
- Contains: Applying methodology to parsed input, decision logic
- Retrieve when: Moving from intake to generation
- Key terms: transformation, process, apply, methodology

### Reference Domains

**xml-blueprint.md**
- Contains: Three-part XML structure specification
- Retrieve when: Structuring output, checking format requirements
- Key terms: XML, blueprint, structure, purpose_and_stance, instructions_and_context

**lever-configuration.md**
- Contains: THINKING_MODE and INTERACTION_STANCE settings
- Retrieve when: Configuring prompt settings based on complexity
- Key terms: THINKING_MODE, Extended, Standard, INTERACTION_STANCE
```

### Component 5: Edge Case Handling

What to do when routing fails or input is problematic.

```markdown
## Edge Case Handling

### Vague Input
If user input is too vague to route effectively:
1. DO NOT retrieve broadly hoping for relevance
2. ASK for clarification: What is the core task? What should the output do?
3. THEN route to appropriate knowledge

### Knowledge Gaps
If retrieved content doesn't address the need:
1. State what specific knowledge wasn't found
2. Offer general guidance with caveats
3. DO NOT synthesise from tangentially related content

### Out of Scope
If the request falls outside prompt transformation:
1. State clearly what IS in scope (prompt architecture for Claude)
2. Suggest alternative resources if appropriate
```

---

## Size Guidelines

| Section | Target Size | If Too Long... |
|---------|-------------|----------------|
| Core Purpose | 200-400 chars | Move detail to operational files |
| System Architecture | 300-500 chars | Just list domains, don't describe |
| Query Routing | 2,000-3,000 chars | Reduce examples, keep most common |
| Content Index | 1,500-2,500 chars | Shorten descriptions |
| Edge Cases | 400-600 chars | Be more concise |
| **Total** | **4,000-8,000 chars** | Decompose more content to files |

**Too short (< 4,000 chars):** Insufficient routing guidance, missing edge cases.

**Too long (> 8,000 chars):** You're putting content in Instructions that should be in knowledge files. Apply the conductor pattern more aggressively.

---

## Phase 3 Checklist

Before delivering INSTRUCTIONS.md:

**Structure:**
- [ ] Core purpose present (2-3 sentences maximum)
- [ ] System architecture lists operational AND reference domains
- [ ] Query routing has 15-20 concrete examples
- [ ] Operational retrieval section present
- [ ] Reference retrieval section present
- [ ] Combined retrieval section present
- [ ] Content index covers ALL domains
- [ ] Edge case handling present

**Quality:**
- [ ] Total size 4,000-8,000 characters
- [ ] NO workflows or processes in Instructions (those go in operational files)
- [ ] NO methodology or examples in Instructions (those go in reference files)
- [ ] Query examples are CONCRETE, not abstract patterns
- [ ] Each domain in content index has: Contains, Retrieve when, Key terms

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
[...continues for 2,000 characters...]
```

**Right:**
```markdown
## Query Routing — Operational

| Situation | Retrieve | Search Query |
|-----------|----------|--------------|
| Starting transformation | intake-analysis.md | "intake analysis process" |
```

The workflow lives in `operational/intake-analysis.md`, not in Instructions.

### Mistake: Abstract routing patterns

**Wrong:**
```markdown
Search for [component] + [issue] when troubleshooting.
Search for [action] + [feature] for how-to questions.
```

**Right:**
```markdown
| User Question | Retrieve | Search Query |
|---------------|----------|--------------|
| "Why is my output wrong?" | common-failures.md | "output failure whiff troubleshoot" |
| "How do I configure thinking mode?" | lever-configuration.md | "THINKING_MODE configure Extended" |
```

Query generation is stochastic. Concrete examples are pattern-matched more reliably than abstract rules.

### Mistake: Missing combined retrieval

**Wrong:** Only operational OR reference retrieval documented.

**Right:** Include combined retrieval for complex tasks that need both layers. Most real tasks need knowledge from both layers.

---

## Next Phase

Proceed to **Phase 4: Create Domain Files** — Write chunk-resilient operational and reference content.

- For operational files: Use `templates/operational-domain-template.md`
- For reference files: Use `templates/domain-file-template.md`

---

## Related References

- `templates/routing-instruction-template.md` — Full Instructions template
- `phases/phase-2-architecture.md` — Source for domain structure
- `phases/phase-6-verification.md` — Testing instruction effectiveness
- `anti-patterns.md` — Instruction-related anti-patterns
