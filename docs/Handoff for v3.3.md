> **Note:** This document has been archived to the release notes system. For the formatted version, see [docs/release-notes/v3.3.md](release-notes/v3.3.md). The centralized changelog is at [CHANGELOG.md](../CHANGELOG.md).

# Progressive Retrieval-Optimised
Information Structure Maker — Handoff Document

**Version:** 3.3
**Date:** 2 January 2026
**Purpose:** Enable continuation of methodology evolution by another LLM or context session

---

## Executive Summary

This document describes the Progressive Retrieval-Optimised
Information Structure Maker, a methodology for transforming monolithic prompts into Claude Projects-optimised knowledge architectures. It captures the critical evolution from v3.1 to v3.3, including the problem discovery, the failed intermediate attempt (v3.2), and the architectural insight that resolved the issue.

**The core deliverable:** A methodology that decomposes ANY prompt—whether pure reference material, pure operational logic, or a hybrid of both—into a three-layer architecture where slim Instructions route to decomposed Operational and Reference knowledge files.

---

## Part 1: The Problem Discovery

### The Original v3.1 System

Version 3.1 was designed to decompose large reference documents into retrieval-optimised knowledge files for Claude Projects. It worked well for its intended purpose: taking documentation (product manuals, policy guides, technical references) and structuring them so Claude could answer questions about the content.

**v3.1 assumed:** The input is reference material. The output is a Q&A system.

**v3.1 process:**
1. Ingest source material
2. Identify knowledge domains
3. Create Instructions focused on query routing
4. Create domain files optimised for retrieval
5. Test and verify

### The Test Case That Exposed the Gap

A user submitted a "Claude 4 Prompt Architect" system—a prompt designed to transform raw, unstructured prompt ideas into expertly architected Claude 4 prompts. The source material consisted of:

1. **An operational prompt** (~17,000 characters) — The Prompt Architect itself, containing:
   - Core purpose: "Transform diverse user inputs into expertly architected Claude 4 prompts"
   - Operational configuration: THINKING_MODE, INTERACTION_STANCE settings
   - Transformation process: How to analyse inputs and generate structured outputs
   - Decision frameworks: When to use which model, which settings
   - Output specification: The XML blueprint structure

2. **Reference material** (~15,000 characters) — "A Field Manual for Prompting Anthropic's Claude 4 Models", containing:
   - Methodology: The principles behind effective Claude 4 prompting
   - Examples: Sample prompts for different task types
   - Patterns: "Wins" (what works well)
   - Anti-patterns: "Whiffs" (common failures)

### What v3.1 Produced

Following the v3.1 methodology, the system was decomposed into:
- Instructions focused on query routing ("When user asks about X, search for Y")
- 9 domain files covering model selection, levers, dispositions, XML blueprint, etc.
- Test matrix for Q&A verification

### Why It Failed

When asked to evaluate whether the decomposed system could handle a real user query—a raw, vague prompt that needed transformation—the answer was **no**.

**The user's test query:**
```
Improve the prompt below:
I want you to draw up a plan that goes through all of the project files...
[~200 words of unstructured, stream-of-consciousness input]
```

**What the user expected:** The system would TRANSFORM this raw input into a structured XML prompt.

**What v3.1 produced:** A system that could ANSWER QUESTIONS about how to write prompts, but couldn't actually WRITE them.

**The fundamental error:** v3.1 treated the Prompt Architect's operational directives as content to decompose, rather than as task logic to preserve. The transformation capability was destroyed.

---

## Part 2: The Failed Fix (v3.2)

### Initial Diagnosis

The immediate diagnosis was: "I decomposed operational content that should have been preserved."

### The v3.2 Approach

v3.2 introduced:
- **Phase 0: Classification** — Determine if the prompt is REFERENCE, OPERATIONAL, or HYBRID
- **Three instruction templates:**
  - Reference template (for Q&A systems)
  - Operational template (for task-performing systems)
  - Hybrid template (for systems that transform using methodology)

**The v3.2 rule:** "Preserve operational directives in Instructions. Only decompose reference material."

### Why v3.2 Was Still Wrong

The user identified the flaw:

> "What if the instructions in the original prompt are so long? Say the monolithic prompt is 50,000-60,000 characters. Surely that prompt should be decomposed as well... I think there also needs to be—the original monolithic prompt needs to be decomposed into files as well, and the instructions need to index those instruction files."

**The insight:** Preservation vs decomposition is not the right framing. EVERYTHING might need decomposition—the question is what KIND of knowledge it becomes (operational vs reference) and how Instructions route to it.

v3.2 would fail for a 60,000-character operational prompt because it would try to stuff all that logic into Instructions, exceeding practical limits and diluting routing effectiveness.

---

## Part 3: The Resolution (v3.3)

### The Architectural Insight

**Instructions are a CONDUCTOR, not a CONTAINER.**

The conductor:
- Knows what music exists (content index)
- Knows when each section plays (query routing)
- Keeps the orchestra coordinated (edge case handling)
- Does NOT contain every note (that's in the knowledge files)

### The Three-Layer Architecture

```
┌─────────────────────────────────────────────────────────────┐
│ LAYER 1: INSTRUCTIONS (4,000-8,000 chars)                   │
│                                                             │
│ The Conductor:                                              │
│ • Irreducible core purpose (~300 chars)                     │
│ • Content index (~1,500 chars)                              │
│ • Query routing for operational AND reference (~3,000 chars)│
│ • Edge case handling (~500 chars)                           │
└─────────────────────────────────────────────────────────────┘
                          │
          ┌───────────────┴───────────────┐
          ▼                               ▼
┌─────────────────────────┐   ┌─────────────────────────┐
│ LAYER 2: OPERATIONAL    │   │ LAYER 3: REFERENCE      │
│ KNOWLEDGE               │   │ KNOWLEDGE               │
│                         │   │                         │
│ HOW the system works:   │   │ WHAT the system knows:  │
│ • Phases & workflows    │   │ • Methodology           │
│ • Decision frameworks   │   │ • Examples & templates  │
│ • Principles            │   │ • Patterns              │
│ • Constraints           │   │ • Definitions           │
│ • Output specifications │   │ • Troubleshooting       │
└─────────────────────────┘   └─────────────────────────┘
```

### What v3.3 Changes

| Aspect | v3.1 | v3.2 | v3.3 |
|--------|------|------|------|
| Classification | None (assumes reference) | Determines preserve vs decompose | Determines content TYPE |
| Operational content | Decomposed (wrong) | Preserved in Instructions | Decomposed into operational files |
| Reference content | Decomposed | Decomposed | Decomposed into reference files |
| Instructions size | Query routing only | Could be huge (preserving ops) | Always 4,000-8,000 chars |
| Scalability | Works for any reference size | Breaks for large operational prompts | Works for ANY input size |

### How v3.3 Handles the Prompt Architect

**Input:** 17,000-char operational prompt + 15,000-char Field Manual

**Output:**
```
prompt-architect/
├── INSTRUCTIONS.md (~6,000 chars)
│   └── Routes to both operational and reference files
├── operational/
│   ├── intake-analysis.md — How to parse and classify input
│   ├── transformation-process.md — How to apply methodology
│   └── output-generation.md — How to structure final output
├── reference/
│   ├── model-selection.md — When to use Opus vs Sonnet
│   ├── lever-configuration.md — THINKING_MODE and INTERACTION_STANCE
│   ├── xml-blueprint.md — The three-part structure
│   ├── wins.md — Effective patterns
│   └── whiffs.md — Common failures
└── supporting/
    ├── quick-reference.md
    └── terminology-index.md
```

**When the user submits a raw prompt:**
1. Instructions route to `intake-analysis.md` for the process
2. During intake, Claude retrieves `lever-configuration.md` to decide settings
3. During generation, Claude retrieves `xml-blueprint.md` for structure
4. Throughout, Claude can retrieve `whiffs.md` to avoid failures

The system TRANSFORMS inputs while RETRIEVING methodology—both capabilities intact.

---

## Part 4: Current State of v3.3

### Completed Files

The following files have been created and are included in this handoff:

**Core Files:**
- `INSTRUCTION.md` — Core routing for the methodology itself
- `README.md` — Overview and deployment guide

**Phase Documentation:**
- `phases/phase-0-classification.md` — Content classification and architecture design
- `phases/phase-1-ingestion.md` — Extraction by content type

**Templates:**
- `templates/routing-instruction-template.md` — Instructions as conductor
- `templates/operational-domain-template.md` — Writing operational knowledge files

### Files Still Needed

The following files from v3.1 remain valid but may need review for v3.3 consistency:

**From v3.1 (retain as-is or update):**
- `phases/phase-2-architecture.md` — Domain structure design
- `phases/phase-3-instructions.md` — Needs replacement with routing-focused guidance
- `phases/phase-4-domain-files.md` — Reference file creation (still valid)
- `phases/phase-5-hybrid-optimisation.md` — Balance checking (still valid)
- `phases/phase-6-verification.md` — Testing (needs update for operational retrieval)
- `templates/domain-file-template.md` — Reference domains (still valid)
- `templates/test-matrix-template.md` — Needs operational test types
- `mechanics/retrieval-mechanics.md` — Still valid
- `mechanics/chunk-resilience.md` — Still valid (applies to operational files too)
- `mechanics/semantic-hedging.md` — Still valid
- `mechanics/bm25-optimisation.md` — Still valid
- `mechanics/rank-fusion.md` — Still valid
- `anti-patterns.md` — Needs operational anti-patterns added
- `quality-gates.md` — Needs operational gates added

### Known Gaps

1. **Phase 3 needs rewriting** — Current phase-3-instructions.md assumes the old template; needs to reference routing-instruction-template.md

2. **Phase 6 verification needs expansion** — Test matrix should include:
   - Operational retrieval tests (can workflows be retrieved?)
   - Combined retrieval tests (can complex tasks get both layers?)
   - Transformation tests (does the system still DO its job?)

3. **Quality gates need operational gates** — Current gates focus on reference content; need gates for:
   - Operational file chunk resilience
   - Routing coverage (do Instructions route to all domains?)
   - Transformation verification

4. **Anti-patterns need operational examples** — Current anti-patterns focus on reference content; need:
   - "Conductor contains sheet music" (Instructions too long)
   - "Orphaned operational file" (not routed to)
   - "Broken cross-layer connection" (operational needs reference that doesn't exist)

---

## Part 5: Design Principles

These principles should guide continued evolution:

### Principle 1: Instructions Are Conductors

Instructions should NEVER exceed 8,000 characters. If they do, content that should be in knowledge files is in Instructions. The role of Instructions is to know what exists and route to it, not to contain operational logic.

### Principle 2: Content Type Determines File Type, Not Decomposition

The question is never "should I decompose this?" — everything gets decomposed. The question is "what kind of knowledge file does this become?"
- Process/workflow/decision → Operational file
- Methodology/example/pattern → Reference file

### Principle 3: Classification Is About Understanding, Not Deciding

Phase 0 classification doesn't decide whether to decompose. It helps you understand WHAT you're decomposing so you put content in the right layer.

### Principle 4: Chunk Resilience Applies to Everything

Operational files need the same chunk resilience as reference files:
- Paragraph autonomy
- Distributed keywords
- Self-contained steps
- No cross-references

### Principle 5: Test What the System Does

For reference-only systems: Test question answering
For hybrid systems: Test transformation WITH retrieval
For operational-only systems: Test transformation

The test matrix must match the system's purpose.

---

## Part 6: Continuation Guidance

### Immediate Priorities

1. **Update phase-3-instructions.md** to reference the routing-instruction-template and explain the conductor pattern

2. **Update phase-6-verification.md** to include operational retrieval testing

3. **Update quality-gates.md** with operational gates

4. **Create a complete worked example** — Take the Prompt Architect + Field Manual through the full v3.3 process to validate the methodology

### Open Questions

1. **Operational file granularity:** How fine-grained should operational decomposition be? One file per phase? Per decision framework? Current guidance suggests 2,000-5,000 chars per file, but real-world testing is needed.

2. **Cross-layer retrieval patterns:** When operational processes need reference knowledge, what's the best way to document these connections? The current approach uses a connection mapping table, but this may need refinement.

3. **Irreducible core definition:** What MUST stay in Instructions vs what CAN be retrieved? Current guidance says "core purpose only" but edge cases may exist.

4. **Test matrix for transformation:** How do you test that a decomposed operational system still performs its transformation correctly? Q&A testing is straightforward; transformation testing is harder.

### Potential Future Directions

1. **Automated classification:** Could Phase 0 be partially automated with heuristics?

2. **Size-based recommendations:** Could the methodology automatically suggest domain splits based on character counts?

3. **Template generation:** Could the methodology generate skeleton files based on Phase 0 classification?

4. **Retrieval simulation:** Could test matrices be validated before deployment by simulating retrieval?

---

## Part 7: File Reference

### Project Files (Included in Handoff)

```
methodology-v3.3/
├── INSTRUCTION.md              # Paste into Project Instructions
├── README.md                   # Overview and deployment
├── phases/
│   ├── phase-0-classification.md   # Content classification
│   └── phase-1-ingestion.md        # Layer-based extraction
└── templates/
    ├── routing-instruction-template.md   # Instructions as conductor
    └── operational-domain-template.md    # Operational file structure
```

### Existing v3.1 Files (In Current Project, May Need Updates)

```
Current project files (view with tool):
├── phase-2-architecture.md     # Domain design (review for v3.3)
├── phase-3-instructions.md     # NEEDS REPLACEMENT
├── phase-4-domain-files.md     # Reference files (still valid)
├── phase-5-hybrid-optimisation.md  # Balance (still valid)
├── phase-6-verification.md     # NEEDS UPDATE
├── domain-file-template.md     # Reference template (still valid)
├── test-matrix-template.md     # NEEDS UPDATE
├── instruction-template.md     # SUPERSEDED by routing template
├── retrieval-mechanics.md      # Still valid
├── chunk-resilience.md         # Still valid
├── semantic-hedging.md         # Still valid
├── bm25-optimisation.md        # Still valid
├── rank-fusion.md              # Still valid
├── anti-patterns.md            # NEEDS ADDITIONS
└── quality-gates.md            # NEEDS ADDITIONS
```

---

## Summary

**The journey:**
- v3.1 worked for reference material but broke operational prompts
- v3.2 tried to preserve operational content but couldn't scale
- v3.3 recognises that Instructions are conductors, and BOTH layers get decomposed

**The insight:** Don't ask "preserve or decompose?" Ask "what kind of file does this become?"

**The architecture:** Slim routing Instructions + Operational knowledge files + Reference knowledge files

**The outcome:** A methodology that handles ANY input size while preserving operational purpose.

---

*End of handoff document*