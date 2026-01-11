# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with this repository.

## Repository

- **GitHub:** https://github.com/ArchiJones-AI/PRISM
- **License:** MIT
- **Version:** v3.4

## Project Overview

PRISM (Progressive Retrieval-Optimised Information Structure Maker) v3.4 is a methodology for transforming monolithic system prompts into Claude Projects-optimised knowledge architectures. It is not a codebase—it is a structured methodology implemented as documentation for a Claude Project.

**Core Insight:** Instructions should be a CONDUCTOR, not a CONTAINER. They route to content; they don't hold it. If Instructions exceed 8,000 chars, you're putting sheet music in the conductor.

## Architecture

The methodology uses a three-layer architecture:

1. **Layer 1: Instructions (4,000-8,000 chars)** - The conductor that routes to content
2. **Layer 2: Operational Knowledge** - HOW the system works (workflows, decisions)
3. **Layer 3: Reference Knowledge** - WHAT the system knows (concepts, examples)

## Repository Structure

```
PRISM/
├── README.md                   # GitHub landing page
├── CLAUDE.md                   # This file (Claude Code guidance)
├── CHANGELOG.md                # Version history
├── LICENSE                     # MIT License
├── assets/
│   └── prism-overview.png      # Overview diagram
├── Claude Project/             # The PRISM methodology
│   ├── INSTRUCTION.md          # Core routing instructions
│   └── Project-Files/          # Phase guides, templates, reference docs
├── claude-code-expert-system/  # Example implementation
│   ├── INSTRUCTIONS.md         # Conductor layer
│   ├── operational/            # 4 workflow files
│   ├── reference/              # 15 domain files
│   └── supporting/             # 5 cross-cutting files
└── docs/                       # Architecture and tutorials
    ├── SOLUTION-ARCHITECTURE.md
    ├── Handoff for v3.3.md
    ├── Learning-how-it-works/
    └── release-notes/
```

## Key Directories

| Directory | Purpose |
|-----------|---------|
| `Claude Project/` | The PRISM methodology itself—upload to a Claude Project to use |
| `Claude Project/Project-Files/` | Phase guides (0-6), templates, reference documentation |
| `claude-code-expert-system/` | Complete example implementation demonstrating all v3.4 features |
| `docs/` | Technical architecture, version history, learning tutorials |

## Core Concepts

### The Conductor Pattern
Instructions route to knowledge; they don't contain it. Query routing has three categories:
- **Operational retrieval:** HOW to proceed (workflows, processes)
- **Reference retrieval:** WHAT knowledge to apply (methodology, examples)
- **Combined retrieval:** Complex tasks needing BOTH layers

### Chunk Resilience
Content must survive Claude Projects' arbitrary chunking (400-800 tokens). Every paragraph should:
- Open with context (no "this" without antecedent)
- Distribute keywords throughout (not just in headings)
- Use inverted pyramid structure (answer first, details after)

### Hybrid Retrieval Optimisation
Claude Projects uses Contextual Retrieval combining:
- **Embeddings** - Semantic similarity
- **BM25** - Keyword matching (distribute terms every 500-800 chars)
- **Rank Fusion** - Chunks must score well on BOTH signals

## Eight-Phase Execution Sequence

1. **Phase 0: Classify** - Tag content as OPERATIONAL or REFERENCE
2. **Phase 1: Ingest** - Extract content by layer
3. **Phase 2: Architect** - Design domain structure (MANDATORY USER CHECKPOINT)
4. **Phase 3: Instructions** - Create routing layer using conductor pattern
5. **Phase 4-5: Domain Files** - Write operational + reference files
6. **Phase 6: Optimise** - Balance BM25 and embedding signals
7. **Phase 7: Verify** - Test retrieval with 30+ queries across 7 types
8. **Phase 8: Package** - Organise and deliver

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

## Working with This Repository

This is a methodology, not executable code. When working here:
- Read `docs/SOLUTION-ARCHITECTURE.md` for complete technical understanding
- Read phase files sequentially when learning the process
- Use `routing-instruction-template.md` for creating v3.4 Instructions
- Templates in `Claude Project/Project-Files/` are blueprints for generated output
- The `claude-code-expert-system/` directory demonstrates all v3.4 features in practice

## v3.4 Features

- **YAML Frontmatter:** Machine-readable metadata in all domain files
- **Tiered Source Authority:** 4-tier system for ranking authoritative sources
- **Cross-Cutting Domains:** Guidance for decision matrices and bridging files
- **Prompt Templates:** Ready-to-use prompts in reference files
