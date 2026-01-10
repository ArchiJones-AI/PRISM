# PRISM v3.4

**Progressive Retrieval-Optimised Information Structure Maker**

A methodology for transforming monolithic system prompts into Claude Projects-optimised knowledge architectures.

![PRISM Overview](assets/prism-overview.png)

## The Problem

Large system prompts hit Claude's context limits and degrade retrieval accuracy. Claude Projects solves this with knowledge files, but poorly structured files lead to retrieval failures—Claude fetches the wrong content or misses relevant information entirely.

## The Solution

PRISM transforms sprawling prompts into retrieval-optimised knowledge systems using a three-layer architecture:

| Layer | Purpose | Size Target |
|-------|---------|-------------|
| **Instructions** | Routes queries to content (conductor pattern) | 4,000-8,000 chars |
| **Operational** | HOW the system works (workflows, decisions) | 2,000-5,000 chars/file |
| **Reference** | WHAT the system knows (concepts, examples) | 2,000-6,000 chars/file |

> **Core Insight:** Instructions should be a CONDUCTOR, not a CONTAINER. They route to content; they don't hold it. If Instructions exceed 8,000 chars, you're putting sheet music in the conductor.

## Key Features

- **Conductor Pattern** — Instructions route to knowledge; they don't contain it
- **Chunk Resilience** — Content survives Claude's 400-800 token chunking
- **Hybrid Retrieval Optimisation** — Balanced for both BM25 keywords and semantic embeddings
- **8 Quality Gates** — Systematic verification before delivery
- **16 Anti-Patterns** — Common mistakes to avoid

## Quick Start

### 1. Deploy PRISM as a Claude Project

Upload the contents of `Claude Project/` to a new Claude Project:
- `INSTRUCTION.md` → Project Instructions
- `Project-Files/*` → Project Knowledge

### 2. Transform Your Prompt

Give Claude your monolithic prompt and ask it to apply PRISM methodology. The eight-phase process will:

1. **Classify** — Tag content as OPERATIONAL or REFERENCE
2. **Ingest** — Extract content by layer
3. **Architect** — Design domain structure
4. **Instructions** — Create routing layer
5. **Domain Files** — Write operational + reference files
6. **Optimise** — Balance retrieval signals
7. **Verify** — Test with 30+ queries
8. **Package** — Organise and deliver

### 3. Deploy Your New System

Upload the generated files to a new Claude Project and run the test matrix to verify retrieval accuracy.

## Repository Structure

```
├── Claude Project/           # The PRISM methodology itself
│   ├── INSTRUCTION.md        # Core routing instructions
│   └── Project-Files/        # Phase guides, templates, reference docs
│
├── claude-code-expert-system/  # Complete example implementation
│   ├── INSTRUCTIONS.md         # Conductor layer
│   ├── operational/            # 4 workflow files
│   ├── reference/              # 15 domain files
│   └── supporting/             # 5 cross-cutting files
│
└── docs/                     # Architecture docs and tutorials
    ├── SOLUTION-ARCHITECTURE.md
    └── Learning-how-it-works/
```

## Example Implementation

The `claude-code-expert-system/` directory demonstrates PRISM v3.4 applied to Claude Code documentation:

- **Source:** Claude Code official documentation
- **Output:** 27 files across three layers
- **Test Coverage:** 35 verification queries across 7 types

Features demonstrated:
- YAML frontmatter with semantic triggers
- Tiered source authority
- Cross-cutting decision matrices
- Complete test matrix

## Documentation

| Document | Description |
|----------|-------------|
| [SOLUTION-ARCHITECTURE.md](docs/SOLUTION-ARCHITECTURE.md) | Complete technical architecture |
| [CHANGELOG.md](CHANGELOG.md) | Version history and migration guides |
| [Learning-how-it-works/](docs/Learning-how-it-works/) | Practical tutorials |

## v3.4 Highlights

- **YAML Frontmatter** — Machine-readable metadata in all domain files
- **Tiered Source Authority** — 4-tier system for ranking authoritative sources
- **Cross-Cutting Domains** — Guidance for decision matrices and bridging files
- **Prompt Templates** — Ready-to-use prompts in reference files

## License

[MIT](LICENSE)

---

Built for [Claude Projects](https://claude.ai) by humans and AI working together.
