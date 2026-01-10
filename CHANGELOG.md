# PRISM Changelog

All notable changes to the PRISM methodology will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

> **For GTM/Marketing:** Look for **Highlights** sections for slide-ready content.
> **For Implementers:** Full technical details in each version's **Technical Changes** section.

---

## [Unreleased]

### Coming Soon

- [Placeholder for next version features]

---

## [3.4] - 2025-01-02

### Highlights (GTM-Ready)

**Theme:** Enhanced Metadata and Source Authority

- **YAML Frontmatter** - Machine-readable metadata in every domain file enables tooling integration and automated validation
- **Tiered Source Authority** - 4-tier ranking system ensures users know which sources to trust
- **Cross-Cutting Domains** - New guidance for decision matrices and bridging files that span multiple domains
- **Prompt Templates** - Ready-to-use prompts embedded in reference files improve user experience

### Technical Changes

#### Added

- YAML frontmatter pattern for all domain files with fields: `domain`, `type`, `semantic_triggers`, `priority_sources`, `related_domains`, `last_updated`
- Tiered source authority system (Tier 1: Official/Source of Truth through Tier 4: Community/Third-party)
- Cross-cutting domain guidance in phase-2-architecture.md (Step 3B)
- Prompt templates section requirement for reference files
- `quick-reference-template.md` for supporting file creation
- `terminology-index-template.md` for supporting file creation
- Quality gate checklist items for v3.4 features (Gates 2, 2A, 3)

#### Changed

- `domain-file-template.md` now includes frontmatter, authoritative sources, and prompt templates sections
- `operational-domain-template.md` now includes frontmatter requirements
- Reference file size guidance expanded to 2,000-6,000 characters

### Migration from v3.3

1. Add YAML frontmatter to all existing domain files
2. Add "Authoritative Sources" section to reference files
3. Add "Prompt Templates" section to reference files
4. Review and add cross-cutting files to supporting/ directory if needed
5. Re-run quality gates using updated checklist

**Detailed release notes:** [docs/release-notes/v3.4.md](docs/release-notes/v3.4.md)

---

## [3.3] - 2025-01-02

### Highlights (GTM-Ready)

**Theme:** The Conductor Pattern - Universal Scalability

- **Three-Layer Architecture** - Instructions + Operational + Reference creates clear separation of concerns
- **Conductor Pattern** - Instructions route to content instead of containing it, enabling unlimited source prompt sizes
- **Universal Decomposition** - Both operational AND reference content become retrievable files
- **Size-Aware Design** - Target 4,000-8,000 char Instructions regardless of source size

### Technical Changes

#### Added

- Three-layer architecture (Instructions, Operational, Reference)
- Conductor pattern for Instructions design
- Operational domain template (`operational-domain-template.md`)
- Phase 0: Classification as new first phase
- Seven test query types (added operational and combined retrieval)
- `routing-instruction-template.md` as primary instruction template

#### Changed

- Instructions target reduced to 4,000-8,000 characters (was variable)
- All operational content now decomposed into files (was preserved in Instructions)
- Test matrix expanded to 30+ queries (was 25+)
- Quality gates reorganized with Gate 2A for operational files

#### Deprecated

- `instruction-template.md` superseded by `routing-instruction-template.md`

#### Fixed

- Resolved scalability issue where large operational prompts exceeded instruction limits

### Migration from v3.2

1. Classify existing content as operational vs reference using Phase 0
2. Move operational content from Instructions to operational/ files
3. Update Instructions to use conductor pattern (route, don't contain)
4. Add operational retrieval tests to test matrix
5. Add combined retrieval tests for queries needing both layers

**Detailed release notes:** [docs/release-notes/v3.3.md](docs/release-notes/v3.3.md)

---

## [3.2] - 2024-12

### Highlights (GTM-Ready)

**Theme:** Hybrid Prompt Support

- **Prompt Classification** - Distinguish between reference-only, operational-only, and hybrid prompts
- **Hybrid Templates** - Specialized templates for prompts that both transform and retrieve

### Technical Changes

#### Added

- Phase 0 classification concept (preliminary version)
- Hybrid instruction template for mixed operational/reference prompts
- Prompt type detection guidance

**Note:** This version attempted to solve operational prompt handling by preserving operational content in Instructions. This approach was superseded by v3.3's universal decomposition.

**Detailed release notes:** [docs/release-notes/v3.2.md](docs/release-notes/v3.2.md)

---

## [3.1] - 2024-11

### Highlights (GTM-Ready)

**Theme:** Retrieval Reliability

- **Chunk Resilience** - Content survives Claude Projects' arbitrary 400-800 token chunking
- **Concrete Query Examples** - 10-15 specific examples replace abstract routing patterns
- **Hybrid Retrieval Optimization** - Balance BM25 keywords and embedding similarity

### Technical Changes

#### Added

- Chunk resilience guidelines (paragraph autonomy, inverted pyramid structure)
- Concrete query example requirements in Instructions (10-15 examples)
- BM25 keyword distribution guidance (every 500-800 chars)
- Semantic hedging for embedding optimization
- Anti-pattern documentation

**Detailed release notes:** [docs/release-notes/v3.1.md](docs/release-notes/v3.1.md)

---

## [3.0] - 2024-10

### Highlights (GTM-Ready)

**Theme:** Foundation for Retrieval Optimization

- **Initial Architecture** - First chunk-aware decomposition methodology
- **Claude Projects Integration** - Designed specifically for Contextual Retrieval
- **Quality Gates** - Verification framework for decomposed systems

### Technical Changes

#### Added

- Initial methodology for decomposing prompts into knowledge systems
- Domain file structure with 2,000-5,000 character targets
- Test matrix concept for verification
- Basic quality gates framework
- Instructions template for query routing

**Detailed release notes:** [docs/release-notes/v3.0.md](docs/release-notes/v3.0.md)

---

## Version Comparison Matrix

| Feature | v3.0 | v3.1 | v3.2 | v3.3 | v3.4 |
|---------|:----:|:----:|:----:|:----:|:----:|
| Chunk resilience | - | Yes | Yes | Yes | Yes |
| Concrete query examples | - | Yes | Yes | Yes | Yes |
| Prompt classification | - | - | Yes | Yes | Yes |
| Three-layer architecture | - | - | - | Yes | Yes |
| Conductor pattern | - | - | - | Yes | Yes |
| Universal decomposition | - | - | - | Yes | Yes |
| YAML frontmatter | - | - | - | - | Yes |
| Tiered source authority | - | - | - | - | Yes |
| Cross-cutting domains | - | - | - | - | Yes |
| Prompt templates | - | - | - | - | Yes |

---

## GTM Extraction Guide

| Section | Use For |
|---------|---------|
| Version theme | Slide title |
| Highlights (4 bullets) | Key features slide |
| Version Comparison Matrix | Feature evolution visual |
| Executive Summary (from detailed notes) | Marketing copy |

---

*Changelog maintained for PRISM methodology. See [docs/release-notes/](docs/release-notes/) for detailed release notes.*
