# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

PRISM (Progressive Retrieval-Optimised Information Structure Maker) v3.4 is a methodology for transforming monolithic system prompts into Claude Projects-optimised knowledge architectures. It is not a codebase—it is a structured methodology implemented as documentation for a Claude Project.

**Core Insight:** Instructions should be a CONDUCTOR, not a CONTAINER. They route to content; they don't hold it. If Instructions exceed 8,000 chars, you're putting sheet music in the conductor.

### v3.4 New Features
- **YAML Frontmatter:** Machine-readable metadata in all domain files (semantic triggers, source authority, related domains)
- **Tiered Source Authority:** 4-tier system for ranking authoritative sources
- **Cross-Cutting Domains:** Explicit guidance for decision matrices and bridging files
- **Prompt Templates:** Ready-to-use prompts in reference files for user experience

> **Full changelog:** See [CHANGELOG.md](CHANGELOG.md) for version history and migration guides.

## Architecture

The methodology uses a three-layer architecture:

1. **Layer 1: Instructions (4,000-8,000 chars)** - The conductor that routes to content
   - Core purpose (200-400 chars)
   - System architecture (300-500 chars)
   - Query routing with 15-20 concrete examples
   - Content index (1,500-2,500 chars)
   - Edge case handling (400-600 chars)

2. **Layer 2: Operational Knowledge** - HOW the system works
   - Phases and workflows (imperative voice)
   - Decision frameworks (specific criteria)
   - Constraints and output specs

3. **Layer 3: Reference Knowledge** - WHAT the system knows
   - Methodology explanations
   - Examples and templates
   - Patterns and definitions

## Key Directories

```
Claude Project/
├── INSTRUCTION.md              # Core routing for the methodology itself
├── Project-Files/
│   ├── README.md               # Overview and deployment guide
│   ├── phase-0-classification.md   # Content classification
│   ├── phase-1-ingestion.md        # Extract content by layer
│   ├── phase-2-architecture.md     # Design domain structure (both layers)
│   ├── phase-3-instructions.md     # Create routing layer (conductor pattern)
│   ├── phase-4-domain-files.md     # Write reference files
│   ├── phase-5-hybrid-optimisation.md  # Balance retrieval signals
│   ├── phase-6-verification.md     # Test retrieval accuracy (7 test types)
│   ├── anti-patterns.md            # 16 anti-patterns to avoid
│   ├── quality-gates.md            # 8 quality gates before delivery
│   └── Templates:
│       ├── routing-instruction-template.md   # v3.4 Instructions template
│       ├── operational-domain-template.md    # Operational file template (with frontmatter)
│       ├── domain-file-template.md           # Reference file template (with frontmatter)
│       ├── quick-reference-template.md       # Supporting file template (v3.4)
│       ├── terminology-index-template.md     # Supporting file template (v3.4)
│       └── test-matrix-template.md           # Test matrix template
docs/
├── SOLUTION-ARCHITECTURE.md    # Complete technical architecture
├── Handoff for v3.3.md         # Version evolution and design decisions
└── Learning-how-it-works/      # Practical tutorials
```

## Core Concepts

### The Conductor Pattern
Instructions don't contain operational logic—they ROUTE to it. Query routing has three categories:
- **Operational retrieval:** HOW to proceed (workflows, processes)
- **Reference retrieval:** WHAT knowledge to apply (methodology, examples)
- **Combined retrieval:** Complex tasks needing BOTH layers

### Chunk Resilience
Content must survive Claude Projects' arbitrary chunking (400-800 tokens). Every paragraph should:
- Open with context (no "this" without antecedent)
- Distribute keywords throughout (not just in headings)
- Use inverted pyramid structure (answer first, details after)
- Avoid cross-references between files

Applies to BOTH operational and reference files.

### Hybrid Retrieval Optimisation
Claude Projects uses Contextual Retrieval combining:
- **Embeddings** - Semantic similarity (optimise with synonym variations)
- **BM25** - Keyword matching (distribute exact terms every 500-800 chars)
- **Rank Fusion** - Chunks must score well on BOTH signals

## Eight-Phase Execution Sequence

1. **Phase 0: Classify** - Tag content as OPERATIONAL or REFERENCE
2. **Phase 1: Ingest** - Extract content by layer
3. **Phase 2: Architect** - Design domain structure for both layers (MANDATORY USER CHECKPOINT)
4. **Phase 3: Instructions** - Create routing layer using conductor pattern
5. **Phase 4-5: Domain Files** - Write operational + reference files
6. **Phase 6: Optimise** - Balance BM25 and embedding signals
7. **Phase 7: Verify** - Test retrieval with 30+ queries across 7 types
8. **Phase 8: Package** - Organise and deliver

## Output Structure

Generated systems follow this structure:
```
{system-name}/
├── INSTRUCTIONS.md             # Routing layer (4,000-8,000 chars)
├── operational/                # HOW files (2,000-5,000 chars each)
├── reference/                  # WHAT files (2,000-6,000 chars each)
├── supporting/
│   ├── quick-reference.md      # Fast lookups
│   ├── terminology-index.md    # Term-to-domain mapping
│   └── [cross-cutting].md      # Decision matrices, source indices (v3.4)
├── TEST-MATRIX.md
└── README.md
```

All domain files include YAML frontmatter with semantic triggers, source authority, and related domains.

## Quality Gates (8 Gates)

Before delivery, verify:
- **Gate 1:** Instructions 4,000-8,000 chars with 15-20 query routing examples
- **Gate 2:** Reference files with chunk-resilient paragraphs
- **Gate 2A:** Operational files with imperative voice and specific criteria
- **Gate 3:** Supporting files present
- **Gate 4:** Test matrix 30+ queries covering all 7 types
- **Gate 5:** Coverage complete (all source material represented)
- **Gate 6:** No anti-patterns (16 patterns to avoid)
- **Gate 7:** Correct directory structure with operational/ and reference/ folders

## Anti-Patterns (16 Patterns)

Key anti-patterns organised by category:
- **Conductor:** Sheet music in conductor, orphaned files
- **Operational:** Context-free steps, passive voice, vague criteria, broken cross-layer connections
- **Reference:** Context-free paragraphs, front-loaded keywords, keyword deserts, giant sections, cross-references, paraphrased errors

## Example Implementation: claude-code-expert-system

The `claude-code-expert-system/` directory contains a complete PRISM v3.4 implementation—a Claude Projects knowledge system for Claude Code documentation.

```
claude-code-expert-system/
├── INSTRUCTIONS.md              # Conductor layer (~4,500 chars)
├── operational/                 # 3 HOW workflow files
│   ├── feature-implementation.md
│   ├── problem-diagnosis.md
│   └── workflow-execution.md
├── reference/domains/           # 15 WHAT reference files
├── supporting/                  # 5 cross-cutting files
│   ├── quick-reference.md
│   ├── terminology-index.md
│   ├── component-comparison.md
│   ├── source-index.md
│   └── model-selection.md
├── TEST-MATRIX.md               # 35 verification queries
└── README.md                    # Deployment guide
```

This system demonstrates all v3.4 features:
- YAML frontmatter with semantic triggers and tiered sources
- Three-layer architecture (operational + reference + supporting)
- Cross-cutting decision matrix files
- Complete test matrix with 7 query types

## Example Implementation: conveyancing-expert-system

The `conveyancing-expert-system/` directory contains a PRISM v3.4 implementation—a Claude Projects knowledge system for UK residential conveyancing advisory.

```
conveyancing-expert-system/
├── INSTRUCTIONS.md              # Conductor layer (~5,900 chars)
├── operational/                 # 5 HOW workflow files
│   ├── query-handling.md
│   ├── document-retrieval.md
│   ├── negotiation-workflow.md
│   ├── communication-support.md
│   └── risk-assessment.md
├── reference/                   # 6 WHAT reference files
│   ├── conveyancing-fundamentals.md
│   ├── aml-regulations.md
│   ├── source-of-funds.md
│   ├── price-renegotiation.md
│   ├── critical-rules.md
│   └── output-formats.md
├── supporting/                  # 4 cross-cutting files
│   ├── quick-reference.md
│   ├── terminology-index.md
│   ├── transaction-context.md
│   └── document-index.md
├── TEST-MATRIX.md               # 35 verification queries
└── README.md                    # Deployment guide
```

This system demonstrates:
- Decomposition of a complex advisory prompt (~15,000 chars source)
- Transaction-specific context preservation
- Critical rules enforcement (fund transfers, exchange binding)
- Operational workflows for negotiation, communication, risk assessment
- 35-query test matrix across 7 types

## Recent Implementations

### PRISM Self-Remediation (January 2026)

Applied remediation to bring the methodology into compliance with its own standards:

- **FIX-01:** Added "Handling Vague Questions" section to INSTRUCTION.md
- **FIX-02:** Added "When Retrieved Content Doesn't Answer" section to INSTRUCTION.md
- **FIX-03:** Added comprehensive Content Index (20 files) to INSTRUCTION.md
- **FIX-04:** Updated SOLUTION-ARCHITECTURE.md anti-pattern count from 11 to 16 (4 categories)
- **FIX-05:** Deleted deprecated `instruction-template.md`; updated file counts (21→20)
- **FIX-06:** Added YAML frontmatter to 5 key reference files (chunk-resilience, anti-patterns, retrieval-mechanics, bm25-optimisation, semantic-hedging)

The methodology now complies with Anti-Patterns #2 (vague query handling) and #3 (knowledge gap handling).

### Conveyancing Expert System (January 2026)

Created `conveyancing-expert-system/` by decomposing a ~15,000 character UK conveyancing advisory prompt:

- **Source:** Complex XML prompt with expertise domains, operational principles, capabilities, interaction approaches, critical rules, AML framework, transaction context
- **Output:** 18 files across operational/reference/supporting layers
- **INSTRUCTIONS.md:** 5,917 chars (within 4,000-8,000 target)
- **Test coverage:** 35 queries across 7 types

Key decomposition decisions:
- Separated operational workflows (query-handling, negotiation, communication, risk) from reference knowledge (law, AML, SOF evidence)
- Created transaction-context.md and document-index.md as supporting files to preserve client-specific data
- Enforced critical rules in dedicated reference file with cross-references

### Claude Code Expert System (v3.4 Update)

The v3.4 update involved bidirectional learning between PRISM and the claude-code-expert-system:

**Stream A (PRISM v3.4 Updates):**
- Added YAML frontmatter pattern to domain templates
- Added tiered source authority section
- Added prompt templates section
- Created `quick-reference-template.md` and `terminology-index-template.md`
- Added cross-cutting domain guidance to phase-2-architecture.md
- Updated quality-gates.md with v3.4 checklist items

**Stream B (Expert System Completion):**
- Created INSTRUCTIONS.md conductor layer
- Created 3 operational workflow files
- Reorganized directory structure to PRISM-compliant format
- Created terminology-index.md and TEST-MATRIX.md
- Created deployment README.md

## Working with This Repository

This is a methodology, not executable code. When working here:
- Read `SOLUTION-ARCHITECTURE.md` for complete technical understanding
- Read phase files sequentially when learning the process
- The Handoff document explains v3.1→v3.2→v3.3→v3.4 evolution and design rationale
- Use `routing-instruction-template.md` for v3.4 Instructions
- Templates in Project-Files/ are blueprints for generated output
- v3.4 adds YAML frontmatter to all domain templates—use these patterns for new files

**Example implementations:**
- `claude-code-expert-system/` — Technical documentation system (15 reference files)
- `conveyancing-expert-system/` — Advisory system with transaction context (18 files)
