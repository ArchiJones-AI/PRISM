# Phase 0: Classification — Understanding What You're Decomposing

What type of content am I decomposing? How should I structure the final system? This document covers Phase 0: classifying source content and determining the decomposition architecture. Phase 0 must be completed BEFORE Phase 1 ingestion begins.

Key terms: classification, operational content, reference content, routing layer, content type, decomposition architecture, three-layer system, conductor pattern

---

## The Three-Layer Architecture

All complex knowledge systems follow this structure:

```
┌─────────────────────────────────────────────────────────────┐
│ LAYER 1: INSTRUCTIONS (Routing Layer)                       │
│ Target: 4,000-8,000 characters                              │
│                                                             │
│ • Irreducible core purpose                                  │
│ • Content index (what exists, where it lives)               │
│ • Query routing (when to retrieve what)                     │
│ • Edge case handling                                        │
└─────────────────────────────────────────────────────────────┘
                          │
          ┌───────────────┴───────────────┐
          ▼                               ▼
┌─────────────────────────┐   ┌─────────────────────────┐
│ LAYER 2: OPERATIONAL    │   │ LAYER 3: REFERENCE      │
│ KNOWLEDGE               │   │ KNOWLEDGE               │
│                         │   │                         │
│ HOW the system works:   │   │ WHAT the system knows:  │
│ • Phases/workflows      │   │ • Methodology           │
│ • Principles            │   │ • Examples              │
│ • Decision frameworks   │   │ • Patterns              │
│ • Constraints           │   │ • Definitions           │
│ • Output specifications │   │ • Troubleshooting       │
└─────────────────────────┘   └─────────────────────────┘
```

**Key insight:** Instructions are a CONDUCTOR, not a container. They route to content; they don't hold all the content.

---

## Why Both Layers Get Decomposed

### The Problem with Long Instructions

Claude Projects has practical limits:
- Instructions over ~10,000 characters dilute routing effectiveness
- Long operational logic competes with retrieval guidance
- Monolithic instructions can't be selectively retrieved

### The Solution: Decompose Everything

Even operational content (phases, workflows, principles) becomes retrievable knowledge files. Instructions become slim routing that indexes to both:
- Operational files: HOW to perform the task
- Reference files: WHAT knowledge to apply

**Example:** A 50,000-character prompt architect becomes:
- ~6,000-character Instructions (routing layer)
- ~15,000 characters of operational files (phases, decision frameworks)
- ~30,000 characters of reference files (methodology, examples, patterns)

---

## Phase 0 Process

### Step 1: Measure Source Size

Calculate total source material size:
- Main prompt: X characters
- Attached files: Y characters
- Total: X + Y characters

**Size thresholds:**
- Under 15,000 chars total: May not need full decomposition
- 15,000-50,000 chars: Standard decomposition
- Over 50,000 chars: Extensive decomposition with multiple operational domains

### Step 2: Identify Content Types

Read through ALL source material and tag each section:

**OPERATIONAL content** (HOW the system works):
- Task directives and purpose statements
- Process phases and workflows
- Decision frameworks and logic trees
- Principles and constraints
- Output format specifications
- Edge case handling rules

**REFERENCE content** (WHAT the system knows):
- Methodology and theory
- Examples and templates
- Patterns and anti-patterns
- Definitions and terminology
- Troubleshooting guides
- Background knowledge

### Step 3: Estimate Layer Sizes

After tagging, estimate how content distributes:

```markdown
## Content Distribution Estimate

**Source Total:** [X] characters

**Irreducible Core (→ Instructions):** ~[Y] characters
- Core purpose statement
- Essential constraints that must always apply

**Operational Content (→ Operational Files):** ~[Z] characters
- [List major operational sections]

**Reference Content (→ Reference Files):** ~[W] characters
- [List major reference sections]
```

### Step 4: Design File Architecture

Based on content distribution, plan the file structure:

```markdown
## Proposed Architecture

{system-name}/
├── INSTRUCTIONS.md (~[size] chars)
│   └── Routing to: [list what it indexes]
├── operational/
│   ├── [domain-1].md (~[size] chars) — [what it contains]
│   ├── [domain-2].md (~[size] chars) — [what it contains]
│   └── ...
├── reference/
│   ├── [domain-1].md (~[size] chars) — [what it contains]
│   ├── [domain-2].md (~[size] chars) — [what it contains]
│   └── ...
└── supporting/
    ├── quick-reference.md
    └── terminology-index.md
```

### Step 5: Document Classification

Record your classification before proceeding:

```markdown
## Classification: [System Name]

**Source Size:** [X] characters total
**Decomposition Level:** [Standard / Extensive]

**Content Distribution:**
- Instructions (routing): ~[X]% ([Y] chars)
- Operational knowledge: ~[X]% ([Y] chars)
- Reference knowledge: ~[X]% ([Y] chars)

**Irreducible Core (must stay in Instructions):**
- [List elements that cannot be decomposed]

**Operational Domains Identified:**
1. [Domain] — [what it covers]
2. [Domain] — [what it covers]

**Reference Domains Identified:**
1. [Domain] — [what it covers]
2. [Domain] — [what it covers]

**File Architecture:** [See proposed structure above]
```

---

## The Instructions as Conductor

Instructions don't contain operational logic—they ROUTE to it.

### What Instructions MUST Contain

**Irreducible core (~500-1,000 chars):**
- One-paragraph purpose statement
- The fundamental "what this system does"
- Cannot be retrieved; must be immediately present

**Content index (~1,000-2,000 chars):**
- What operational files exist and what they cover
- What reference files exist and what they cover
- Organised for quick orientation

**Query routing (~2,000-4,000 chars):**
- Concrete examples: "When user asks X, retrieve Y"
- Covers both operational and reference retrieval
- Handles the common query patterns

**Edge case handling (~500-1,000 chars):**
- Vague input handling
- Out-of-scope handling
- Knowledge gap handling

### What Instructions Should NOT Contain

- Detailed phase-by-phase workflows (→ operational files)
- Complete decision frameworks (→ operational files)
- Methodology explanations (→ reference files)
- Examples and templates (→ reference files)
- Extensive constraints lists (→ operational files)

---

## Classification Examples

### Example 1: Small Reference System

**Source:** 12,000-character product FAQ

**Classification:**
- Content type: Pure reference (no operational task)
- Size: Below decomposition threshold
- Decision: May work as single knowledge file + slim instructions

**Architecture:**
```
product-faq/
├── INSTRUCTIONS.md (3,000 chars)
└── references/
    └── faq-complete.md (12,000 chars)
```

---

### Example 2: Medium Hybrid System

**Source:** 25,000-character prompt architect with 15,000-character field manual

**Classification:**
- Content type: Hybrid (operational + reference)
- Size: Standard decomposition
- Operational: ~10,000 chars (phases, decision framework)
- Reference: ~30,000 chars (methodology, examples, patterns)

**Architecture:**
```
prompt-architect/
├── INSTRUCTIONS.md (6,000 chars)
├── operational/
│   ├── intake-analysis.md (3,500 chars)
│   ├── transformation-process.md (4,000 chars)
│   └── output-generation.md (2,500 chars)
└── reference/
    ├── methodology/
    │   ├── xml-blueprint.md
    │   ├── lever-configuration.md
    │   └── model-selection.md
    ├── patterns/
    │   ├── wins.md
    │   └── whiffs.md
    └── examples/
        └── prompt-examples.md
```

---

### Example 3: Large Complex System

**Source:** 80,000-character enterprise system with multiple workflows

**Classification:**
- Content type: Hybrid with extensive operational logic
- Size: Extensive decomposition required
- Operational: ~35,000 chars (multiple workflows, decision trees)
- Reference: ~45,000 chars (policies, procedures, examples)

**Architecture:**
```
enterprise-system/
├── INSTRUCTIONS.md (7,000 chars)
├── operational/
│   ├── workflows/
│   │   ├── workflow-intake.md
│   │   ├── workflow-processing.md
│   │   └── workflow-delivery.md
│   ├── decisions/
│   │   ├── routing-logic.md
│   │   └── escalation-framework.md
│   └── constraints/
│       ├── compliance-rules.md
│       └── quality-gates.md
└── reference/
    ├── policies/
    │   └── [policy files]
    ├── procedures/
    │   └── [procedure files]
    └── examples/
        └── [example files]
```

---

## Phase 0 Checklist

Before proceeding to Phase 1:

- [ ] Total source size calculated
- [ ] All content tagged as OPERATIONAL or REFERENCE
- [ ] Content distribution estimated
- [ ] Irreducible core identified
- [ ] Operational domains identified
- [ ] Reference domains identified
- [ ] File architecture proposed
- [ ] Classification documented

---

## Phase 0 Output

**Deliverable:** Classification document containing:
1. Source size and decomposition level
2. Content distribution (operational vs reference)
3. Irreducible core elements
4. Proposed file architecture
5. Domain list for both operational and reference layers

**Next step:** Proceed to Phase 1 with clear understanding of what goes where.

---

## Related References

- `phase-1-ingestion.md` — Detailed extraction by content type
- `templates/routing-instruction-template.md` — Instructions as conductor
- `mechanics/retrieval-mechanics.md` — How retrieval informs architecture
