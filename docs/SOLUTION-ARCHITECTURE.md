# Solution Architecture: Progressive Disclosure Knowledge System Generator v3.4

## Document Information

| Attribute | Value |
|-----------|-------|
| System Name | Progressive Disclosure Knowledge System Generator |
| Version | 3.4 |
| Architecture Type | Three-Layer Decomposition System |
| Platform | Claude Projects (Anthropic) |
| Primary Function | Transform monolithic prompts into retrieval-optimised knowledge systems |

---

## 1. Executive Summary

The Progressive Disclosure Knowledge System Generator v3.4 is a **prompt decomposition methodology** implemented as a Claude Project. It transforms large, monolithic system prompts into structured knowledge systems optimised for Claude Projects' Contextual Retrieval mechanism.

### Core Innovation

**The Conductor Pattern:** Instructions don't contain operational logic—they *route* to it. This enables any prompt size to be decomposed whilst maintaining a slim, effective routing layer.

### Key Metrics

| Target | Value |
|--------|-------|
| Instructions Size | 4,000–8,000 characters |
| Individual Knowledge Files | 2,000–5,000 characters |
| Total Domains | 8–15 per system |
| Test Coverage | 25+ verification queries |

---

## 2. Problem Statement

### The Challenge

Claude Projects has practical limitations:

1. **Long instructions dilute effectiveness** — Instructions over ~10,000 characters reduce routing precision
2. **Monolithic prompts can't be selectively retrieved** — All-or-nothing retrieval wastes context
3. **Operational logic competes with routing guidance** — Workflows embedded in instructions crowd out retrieval direction
4. **No chunk boundary control** — Content is split arbitrarily into ~400–800 token chunks

### The Solution

Decompose EVERYTHING—both operational logic AND reference knowledge—into retrievable files, leaving Instructions as a pure routing layer.

```
┌─────────────────────────────────────────────────────────────┐
│                    BEFORE (Monolithic)                      │
│                                                             │
│  ┌───────────────────────────────────────────────────────┐  │
│  │ 50,000+ character prompt containing:                  │  │
│  │ • Purpose statement                                   │  │
│  │ • Phase 1 workflow (2,000 chars)                     │  │
│  │ • Phase 2 workflow (3,000 chars)                     │  │
│  │ • Decision framework (1,500 chars)                   │  │
│  │ • Methodology explanation (5,000 chars)              │  │
│  │ • Examples (10,000 chars)                            │  │
│  │ • Constraints and principles (3,000 chars)           │  │
│  │ • ...everything else...                              │  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                     AFTER (Decomposed)                      │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ INSTRUCTIONS (~6,000 chars)                         │    │
│  │ • Purpose (300 chars)                               │    │
│  │ • Content Index (1,500 chars)                       │    │
│  │ • Query Routing (3,000 chars)                       │    │
│  │ • Edge Cases (500 chars)                            │    │
│  └─────────────────────────────────────────────────────┘    │
│           │                               │                  │
│           ▼                               ▼                  │
│  ┌─────────────────────┐     ┌─────────────────────┐        │
│  │ OPERATIONAL FILES   │     │ REFERENCE FILES     │        │
│  │ (HOW it works)      │     │ (WHAT it knows)     │        │
│  │ • phase-1.md        │     │ • methodology.md    │        │
│  │ • phase-2.md        │     │ • examples.md       │        │
│  │ • decisions.md      │     │ • patterns.md       │        │
│  │ • constraints.md    │     │ • troubleshooting.md│        │
│  └─────────────────────┘     └─────────────────────┘        │
└─────────────────────────────────────────────────────────────┘
```

---

## 3. Three-Layer Architecture

The system implements a **three-layer architecture** where each layer serves a distinct purpose:

```
┌─────────────────────────────────────────────────────────────┐
│ LAYER 1: INSTRUCTIONS (Routing Layer)                       │
│ Target: 4,000–8,000 characters                              │
│                                                             │
│ The Conductor — knows what exists, routes to it:            │
│ • Irreducible core purpose (~300 chars)                     │
│ • Content index (~1,500 chars)                              │
│ • Query routing (~3,000 chars)                              │
│ • Edge case handling (~500 chars)                           │
│                                                             │
│ ❌ Does NOT contain: workflows, methodology, examples       │
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
│                         │   │                         │
│ Voice: Imperative       │   │ Voice: Descriptive      │
│ ("Do X, then Y")        │   │ ("X works by...")       │
└─────────────────────────┘   └─────────────────────────┘
```

### Layer Characteristics

| Attribute | Layer 1: Instructions | Layer 2: Operational | Layer 3: Reference |
|-----------|----------------------|---------------------|-------------------|
| **Purpose** | Route to content | Define processes | Provide knowledge |
| **Question Answered** | "Where do I look?" | "How do I do this?" | "What do I apply?" |
| **Voice** | Directive | Imperative | Descriptive |
| **Retrieval** | Always present | On-demand | On-demand |
| **Size Target** | 4,000–8,000 chars total | 2,000–5,000 chars per file | 2,000–5,000 chars per file |

---

## 4. Component Taxonomy

### 4.1 File Classification

The project contains **20 files** organised into four functional categories:

```
┌─────────────────────────────────────────────────────────────┐
│                    COMPONENT TAXONOMY                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ ORCHESTRATION (2 files)                             │   │
│  │ Define system purpose and execution sequence        │   │
│  │                                                     │   │
│  │ • README.md (overview, version history)             │   │
│  │ • INSTRUCTIONS (routing layer - in system prompt)   │   │
│  └─────────────────────────────────────────────────────┘   │
│                            │                                │
│          ┌─────────────────┼─────────────────┐             │
│          ▼                 ▼                 ▼             │
│  ┌─────────────┐   ┌─────────────┐   ┌─────────────┐      │
│  │ PHASES (7)  │   │ MECHANICS   │   │ TEMPLATES   │      │
│  │             │   │ (5)         │   │ (4)         │      │
│  │ Sequential  │   │ Technical   │   │ Blueprints  │      │
│  │ workflow    │   │ knowledge   │   │ for output  │      │
│  │             │   │             │   │             │      │
│  │ • phase-0   │   │ • retrieval │   │ • routing   │      │
│  │ • phase-1   │   │ • chunks    │   │ • operation │      │
│  │ • phase-2   │   │ • semantic  │   │ • reference │      │
│  │ • phase-3   │   │ • bm25      │   │ • test      │      │
│  │ • phase-4   │   │ • fusion    │   │             │      │
│  │ • phase-5   │   │             │   │             │      │
│  │ • phase-6   │   │             │   │             │      │
│  └─────────────┘   └─────────────┘   └─────────────┘      │
│                            │                                │
│                            ▼                                │
│              ┌─────────────────────────┐                   │
│              │ QUALITY ASSURANCE (2)   │                   │
│              │                         │                   │
│              │ • anti-patterns.md      │                   │
│              │ • quality-gates.md      │                   │
│              └─────────────────────────┘                   │
└─────────────────────────────────────────────────────────────┘
```

### 4.2 Detailed Component Map

| Category | File | Purpose | Size | Type |
|----------|------|---------|------|------|
| **Orchestration** | README.md | System overview, version history | 7.5K | Reference |
| | Instructions (prompt) | Routing layer, execution sequence | ~5K | Operational |
| **Phases** | phase-0-classification.md | Classify source content | 11K | Operational |
| | phase-1-ingestion.md | Extract content by layer | 9.5K | Operational |
| | phase-2-architecture.md | Design domain structure | 8K | Operational |
| | phase-3-instructions.md | Create routing instructions | 8K | Operational |
| | phase-4-domain-files.md | Write reference files | 9.5K | Operational |
| | phase-5-hybrid-optimisation.md | Balance retrieval signals | 6.5K | Operational |
| | phase-6-verification.md | Test retrieval accuracy | 8.5K | Operational |
| **Mechanics** | retrieval-mechanics.md | How Contextual Retrieval works | 5K | Reference |
| | chunk-resilience.md | Writing for arbitrary splits | 7K | Reference |
| | semantic-hedging.md | Embedding optimisation | 6K | Reference |
| | bm25-optimisation.md | Keyword matching optimisation | 6K | Reference |
| | rank-fusion.md | Balancing retrieval signals | 5.5K | Reference |
| **Templates** | routing-instruction-template.md | Blueprint for Instructions | 11K | Reference |
| | operational-domain-template.md | Blueprint for operational files | 11K | Reference |
| | domain-file-template.md | Blueprint for reference files | 6.5K | Reference |
| | test-matrix-template.md | Blueprint for verification | 7K | Reference |
| **Quality** | anti-patterns.md | 16 common failures to avoid | 9K | Reference |
| | quality-gates.md | Final checklist before delivery | 6.5K | Reference |

---

## 5. Process Model

### 5.1 Phase Sequence

The system executes through **8 sequential phases** with one mandatory checkpoint:

```
┌──────────────────────────────────────────────────────────────────────────┐
│                         EXECUTION SEQUENCE                                │
├──────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────────────────┐   │
│  │ PHASE 0 │───▶│ PHASE 1 │───▶│ PHASE 2 │───▶│ ⚠️ USER CHECKPOINT │   │
│  │ Classify│    │ Ingest  │    │ Architect│   │ Approval Required   │   │
│  └─────────┘    └─────────┘    └─────────┘    └──────────┬──────────┘   │
│                                                          │               │
│                                                          ▼               │
│  ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────────────────┐   │
│  │ PHASE 6 │◀───│ PHASE 5 │◀───│ PHASE 4 │◀───│ PHASE 3             │   │
│  │ Verify  │    │ Optimise│    │ Domain  │    │ Instructions        │   │
│  └────┬────┘    └─────────┘    │ Files   │    └─────────────────────┘   │
│       │                        └─────────┘                               │
│       ▼                                                                  │
│  ┌─────────┐    ┌─────────┐                                             │
│  │ PHASE 7 │───▶│ PHASE 8 │───▶ DELIVERY                                │
│  │ Test    │    │ Package │                                             │
│  │ Matrix  │    │         │                                             │
│  └─────────┘    └─────────┘                                             │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
```

### 5.2 Phase Details

| Phase | Name | Input | Output | Key Activities |
|-------|------|-------|--------|----------------|
| **0** | Classify | Source prompt + attachments | Classification document | Measure size, tag content types, design architecture |
| **1** | Ingest | Classification + source | Knowledge inventory | Extract operational content, extract reference content, build terminology index |
| **2** | Architect | Knowledge inventory | Domain architecture table | Group domains, define triggers, map connections |
| **⚠️** | **Checkpoint** | Architecture table | **User approval** | **MANDATORY PAUSE — Do not proceed without confirmation** |
| **3** | Instructions | Approved architecture | INSTRUCTIONS.md | Create routing layer with query examples |
| **4** | Domain Files | Architecture + inventory | Operational + Reference files | Write chunk-resilient content |
| **5** | Optimise | All files | Optimised files | Balance BM25 and embedding signals |
| **6** | Verify | Complete system | Test results | Execute test matrix, diagnose failures |
| **7** | Test Matrix | Verified system | TEST-MATRIX.md | Document 25+ verification queries |
| **8** | Package | All deliverables | Final system | Organise structure, create README |

### 5.3 Data Flow

```
┌──────────────────────────────────────────────────────────────────────────┐
│                            DATA FLOW                                      │
├──────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  SOURCE MATERIAL                                                         │
│  ┌──────────────────────────────────────────────────────────────────┐   │
│  │ • Primary system prompt                                          │   │
│  │ • Attached methodology documents                                 │   │
│  │ • Examples and templates                                         │   │
│  │ • Supporting files                                               │   │
│  └──────────────────────────────────────────────────────────────────┘   │
│                                    │                                     │
│                                    ▼                                     │
│  PHASE 0: CLASSIFICATION ─────────────────────────────────────────────  │
│                                    │                                     │
│                                    ▼                                     │
│  ┌──────────────────────────────────────────────────────────────────┐   │
│  │ CLASSIFICATION DOCUMENT                                          │   │
│  │ • Source size: X characters                                      │   │
│  │ • Operational content: ~Y%                                       │   │
│  │ • Reference content: ~Z%                                         │   │
│  │ • Proposed file architecture                                     │   │
│  └──────────────────────────────────────────────────────────────────┘   │
│                                    │                                     │
│                                    ▼                                     │
│  PHASE 1: INGESTION ──────────────────────────────────────────────────  │
│                                    │                                     │
│                    ┌───────────────┴───────────────┐                    │
│                    ▼                               ▼                    │
│  ┌─────────────────────────────┐   ┌─────────────────────────────┐     │
│  │ OPERATIONAL EXTRACTION      │   │ REFERENCE EXTRACTION        │     │
│  │ • Phases & workflows        │   │ • Methodology               │     │
│  │ • Decision frameworks       │   │ • Examples                  │     │
│  │ • Principles                │   │ • Patterns                  │     │
│  │ • Constraints               │   │ • Definitions               │     │
│  └─────────────────────────────┘   └─────────────────────────────┘     │
│                    │                               │                    │
│                    └───────────────┬───────────────┘                    │
│                                    ▼                                     │
│  ┌──────────────────────────────────────────────────────────────────┐   │
│  │ KNOWLEDGE INVENTORY                                              │   │
│  │ • Tagged operational content                                     │   │
│  │ • Tagged reference content                                       │   │
│  │ • Terminology index                                              │   │
│  │ • Cross-layer connections                                        │   │
│  └──────────────────────────────────────────────────────────────────┘   │
│                                    │                                     │
│                                    ▼                                     │
│  PHASE 2: ARCHITECTURE ───────────────────────────────────────────────  │
│                                    │                                     │
│                                    ▼                                     │
│  ┌──────────────────────────────────────────────────────────────────┐   │
│  │ DOMAIN ARCHITECTURE TABLE                                        │   │
│  │ • 8–15 domains defined                                           │   │
│  │ • Semantic triggers per domain                                   │   │
│  │ • Query examples per domain                                      │   │
│  │ • Cross-domain relationships                                     │   │
│  └──────────────────────────────────────────────────────────────────┘   │
│                                    │                                     │
│                             ⚠️ CHECKPOINT                               │
│                                    │                                     │
│                                    ▼                                     │
│  PHASES 3–6: CREATION & OPTIMISATION ─────────────────────────────────  │
│                                    │                                     │
│                                    ▼                                     │
│  ┌──────────────────────────────────────────────────────────────────┐   │
│  │ FINAL DELIVERABLE                                                │   │
│  │                                                                  │   │
│  │ {system-name}/                                                   │   │
│  │ ├── INSTRUCTIONS.md          (4,000–8,000 chars)                │   │
│  │ ├── operational/                                                 │   │
│  │ │   └── [workflow files]     (2,000–5,000 chars each)           │   │
│  │ ├── reference/                                                   │   │
│  │ │   └── [knowledge files]    (2,000–5,000 chars each)           │   │
│  │ ├── supporting/                                                  │   │
│  │ │   ├── quick-reference.md                                       │   │
│  │ │   └── terminology-index.md                                     │   │
│  │ ├── TEST-MATRIX.md                                               │   │
│  │ └── README.md                                                    │   │
│  └──────────────────────────────────────────────────────────────────┘   │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
```

---

## 6. Retrieval Optimisation Model

### 6.1 The Hybrid Retrieval Challenge

Claude Projects uses **Contextual Retrieval**, which combines two search mechanisms:

```
┌──────────────────────────────────────────────────────────────────────────┐
│                    HYBRID RETRIEVAL SYSTEM                                │
├──────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  USER QUERY: "How do I configure OAuth?"                                 │
│                          │                                               │
│                          ▼                                               │
│  ┌──────────────────────────────────────────────────────────────────┐   │
│  │ CLAUDE GENERATES SEARCH QUERY                                    │   │
│  │ (Influenced by Instructions)                                     │   │
│  │                                                                  │   │
│  │ Generated: "OAuth configuration setup identity provider"         │   │
│  └──────────────────────────────────────────────────────────────────┘   │
│                          │                                               │
│          ┌───────────────┴───────────────┐                              │
│          ▼                               ▼                              │
│  ┌─────────────────────┐     ┌─────────────────────┐                   │
│  │ EMBEDDING SEARCH    │     │ BM25 KEYWORD SEARCH │                   │
│  │                     │     │                     │                   │
│  │ Converts query to   │     │ Matches exact terms │                   │
│  │ vector, finds       │     │ like "OAuth" and    │                   │
│  │ semantically        │     │ "configuration"     │                   │
│  │ similar chunks      │     │                     │                   │
│  │                     │     │                     │                   │
│  │ Ranking:            │     │ Ranking:            │                   │
│  │ 1. Chunk A (#2)     │     │ 1. Chunk B (#3)     │                   │
│  │ 2. Chunk B (#5)     │     │ 2. Chunk A (#7)     │                   │
│  │ 3. Chunk C (#8)     │     │ 3. Chunk D (#1)     │                   │
│  └─────────────────────┘     └─────────────────────┘                   │
│          │                               │                              │
│          └───────────────┬───────────────┘                              │
│                          ▼                                               │
│  ┌──────────────────────────────────────────────────────────────────┐   │
│  │ RANK FUSION (RRF)                                                │   │
│  │                                                                  │   │
│  │ Combines rankings — chunks scoring well on BOTH signals win:     │   │
│  │                                                                  │   │
│  │ Final Ranking:                                                   │   │
│  │ 1. Chunk B (Embed #5 + BM25 #3 = balanced)    ✓ RETRIEVED       │   │
│  │ 2. Chunk A (Embed #2 + BM25 #7 = balanced)    ✓ RETRIEVED       │   │
│  │ 3. Chunk D (Embed #15 + BM25 #1 = imbalanced) ✗ Lower rank      │   │
│  └──────────────────────────────────────────────────────────────────┘   │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
```

### 6.2 Optimisation Strategies

The system employs **four complementary optimisation strategies**:

| Strategy | Target Signal | Technique | Implementation |
|----------|---------------|-----------|----------------|
| **Semantic Hedging** | Embeddings | Include synonyms, variations, question echoes | Primary term + 2–3 variations in first 500 chars |
| **BM25 Optimisation** | Keywords | Distribute exact terms throughout | Key term every 500–800 characters |
| **Chunk Resilience** | Both | Write paragraphs that survive arbitrary splits | Paragraph autonomy, inverted pyramid structure |
| **Rank Fusion Balance** | Combined | Score well on both signals, not just one | Natural prose with distributed technical terms |

### 6.3 Chunk Resilience Pattern

```
┌──────────────────────────────────────────────────────────────────────────┐
│                    CHUNK RESILIENCE PATTERN                               │
├──────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  ❌ BAD: Context-dependent paragraph                                     │
│  ┌──────────────────────────────────────────────────────────────────┐   │
│  │ "To do this, follow these steps. First, go to the settings.     │   │
│  │ Then find the authentication option. Click on it."              │   │
│  └──────────────────────────────────────────────────────────────────┘   │
│  Problem: If retrieved alone, "this" has no referent                    │
│                                                                          │
│  ✓ GOOD: Self-contained paragraph                                       │
│  ┌──────────────────────────────────────────────────────────────────┐   │
│  │ "To configure OAuth authentication in the Admin Console,        │   │
│  │ navigate to Settings > Authentication > OAuth. OAuth            │   │
│  │ configuration requires credentials from your identity provider." │   │
│  └──────────────────────────────────────────────────────────────────┘   │
│  Solution: Context in every paragraph, keywords distributed             │
│                                                                          │
│  ┌──────────────────────────────────────────────────────────────────┐   │
│  │ INVERTED PYRAMID STRUCTURE                                       │   │
│  │                                                                  │   │
│  │ Sentence 1: THE ANSWER (most important)                         │   │
│  │ Sentence 2: Essential context                                    │   │
│  │ Sentence 3+: Supporting details                                  │   │
│  │                                                                  │   │
│  │ If chunk gets truncated, key information was already delivered   │   │
│  └──────────────────────────────────────────────────────────────────┘   │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
```

---

## 7. Component Relationships

### 7.1 Dependency Graph

```
┌──────────────────────────────────────────────────────────────────────────┐
│                      COMPONENT DEPENDENCY GRAPH                           │
├──────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│                         ┌─────────────────┐                              │
│                         │ INSTRUCTIONS    │                              │
│                         │ (System Prompt) │                              │
│                         └────────┬────────┘                              │
│                                  │ routes to                             │
│                    ┌─────────────┼─────────────┐                        │
│                    ▼             ▼             ▼                        │
│            ┌───────────┐ ┌───────────┐ ┌───────────┐                   │
│            │ PHASES    │ │ MECHANICS │ │ TEMPLATES │                   │
│            │ (7 files) │ │ (5 files) │ │ (5 files) │                   │
│            └─────┬─────┘ └─────┬─────┘ └─────┬─────┘                   │
│                  │             │             │                          │
│                  │ uses        │ informs     │ structures               │
│                  ▼             ▼             ▼                          │
│            ┌─────────────────────────────────────────┐                  │
│            │              OUTPUT SYSTEM              │                  │
│            │  ┌───────────────────────────────────┐  │                  │
│            │  │ {system-name}/                    │  │                  │
│            │  │ ├── INSTRUCTIONS.md               │  │                  │
│            │  │ ├── operational/                  │  │                  │
│            │  │ ├── reference/                    │  │                  │
│            │  │ ├── supporting/                   │  │                  │
│            │  │ └── TEST-MATRIX.md                │  │                  │
│            │  └───────────────────────────────────┘  │                  │
│            └─────────────────────────────────────────┘                  │
│                                  │                                       │
│                                  │ verified by                           │
│                                  ▼                                       │
│                         ┌───────────────┐                               │
│                         │ QUALITY FILES │                               │
│                         │ (2 files)     │                               │
│                         └───────────────┘                               │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
```

### 7.2 File Interaction Matrix

| Component | Depends On | Used By | Purpose |
|-----------|------------|---------|---------|
| **Instructions** | — | All phases | Orchestrates execution, routes queries |
| **phase-0** | Instructions | phase-1 | Classification methodology |
| **phase-1** | phase-0 output | phase-2 | Extraction methodology |
| **phase-2** | phase-1 output | phase-3, phase-4 | Architecture design |
| **phase-3** | phase-2 output, routing-template | Output system | Instructions creation |
| **phase-4** | phase-2 output, domain-templates | Output system | Domain file creation |
| **phase-5** | phase-4 output, mechanics files | Output system | Optimisation pass |
| **phase-6** | All outputs, test-template | Final delivery | Verification |
| **retrieval-mechanics** | — | All phases | Explains retrieval system |
| **chunk-resilience** | retrieval-mechanics | phase-4, phase-5 | Writing guidelines |
| **semantic-hedging** | retrieval-mechanics | phase-4, phase-5 | Embedding optimisation |
| **bm25-optimisation** | retrieval-mechanics | phase-4, phase-5 | Keyword optimisation |
| **rank-fusion** | semantic + bm25 | phase-5 | Balance strategy |
| **anti-patterns** | — | All phases | Failure prevention |
| **quality-gates** | All deliverables | phase-6, delivery | Final checklist |

---

## 8. Key Design Principles

### 8.1 The Seven Principles

| # | Principle | Description | Implementation |
|---|-----------|-------------|----------------|
| 1 | **Conductor, Not Container** | Instructions route to content, don't hold it | Target 4,000–8,000 char Instructions |
| 2 | **Universal Decomposition** | Both operational AND reference content become files | Operational/ and Reference/ directories |
| 3 | **Chunk Resilience** | Content survives arbitrary splits | Paragraph autonomy, distributed keywords |
| 4 | **Hybrid Optimisation** | Balance embedding and BM25 signals | Natural prose with technical terms |
| 5 | **Concrete Over Abstract** | Examples beat patterns | 10–15 specific query examples in Instructions |
| 6 | **Checkpoint Discipline** | Get user approval before major work | Mandatory pause after Phase 2 |
| 7 | **Observable Verification** | Test retrieval, don't assume it works | 25+ query test matrix |

### 8.2 Anti-Pattern Categories

The system documents **16 anti-patterns** across four categories:

**Instruction Anti-Patterns (3):**
1. Abstract query instructions (no concrete examples)
2. No vague query handling
3. No knowledge gap handling

**Conductor Anti-Patterns (3):**
4. Conductor contains sheet music (Instructions too long)
5. Orphaned operational file (not routed to)
6. Orphaned reference file (not routed to)

**Operational File Anti-Patterns (4):**
7. Context-free operational steps
8. Broken cross-layer connection
9. Descriptive operational content (passive voice)
10. Vague decision criteria

**Reference File Anti-Patterns (6):**
11. Context-free paragraphs
12. Front-loaded keywords
13. Keyword deserts
14. Giant sections (>4,000 chars)
15. Cross-referential content
16. Paraphrased error messages

---

## 9. Quality Assurance Framework

### 9.1 Quality Gates

The system defines **7 quality gates** that must pass before delivery:

```
┌──────────────────────────────────────────────────────────────────────────┐
│                       QUALITY GATE CHECKLIST                              │
├──────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  Gate 1: Instructions Quality                                            │
│  ├── □ System overview present (2–3 sentences)                          │
│  ├── □ Query example table (10–15 concrete mappings)                    │
│  ├── □ Vague query handling section                                     │
│  ├── □ Knowledge gap handling section                                   │
│  ├── □ Domain map covering all domains                                  │
│  └── □ Size: 4,000–8,000 characters                                     │
│                                                                          │
│  Gate 2: Domain File Quality                                             │
│  ├── □ Opening has question echoes + key terms                          │
│  ├── □ Every paragraph opens with context                               │
│  ├── □ Keywords distributed every 500–800 chars                         │
│  ├── □ Inverted pyramid structure                                       │
│  ├── □ No cross-references                                              │
│  └── □ File size: 2,000–6,000 characters                                │
│                                                                          │
│  Gate 3: Supporting Files Quality                                        │
│  ├── □ quick-reference.md present                                       │
│  └── □ terminology-index.md present                                     │
│                                                                          │
│  Gate 4: Test Matrix Quality                                             │
│  ├── □ 25+ test queries                                                 │
│  ├── □ All 5 query types represented                                    │
│  └── □ Expected behaviour documented                                    │
│                                                                          │
│  Gate 5: Coverage                                                        │
│  ├── □ 100% source topics represented                                   │
│  └── □ Knowledge gaps documented                                        │
│                                                                          │
│  Gate 6: Anti-Pattern Verification                                       │
│  ├── □ No instruction anti-patterns                                     │
│  └── □ No domain file anti-patterns                                     │
│                                                                          │
│  Gate 7: Deployment Package                                              │
│  ├── □ Correct directory structure                                      │
│  └── □ README with deployment instructions                              │
│                                                                          │
│  ═══════════════════════════════════════════════════════════════════    │
│  ALL GATES MUST PASS BEFORE DELIVERY                                     │
└──────────────────────────────────────────────────────────────────────────┘
```

### 9.2 Test Query Types

| Type | Purpose | Example | Pass Criteria |
|------|---------|---------|---------------|
| **Direct Keyword** | Test BM25 matching | "Configure OAuth" | Exact term chunks retrieved |
| **Semantic Variation** | Test embeddings | "Let users sign in with Google" | Semantically related chunks retrieved |
| **Troubleshooting** | Test verbatim errors | "error: invalid_client" | Error-specific chunks retrieved |
| **Vague** | Test clarification handling | "It's not working" | Claude asks for specifics |
| **Out-of-Scope** | Test gap handling | "[uncovered topic]" | Claude acknowledges gap |

---

## 10. Output Specification

### 10.1 Directory Structure

```
{system-name}/
├── INSTRUCTIONS.md                 # Routing layer
│   Target: 4,000–8,000 characters
│   Contains: Purpose, index, routing, edge cases
│
├── operational/                    # HOW the system works
│   ├── [phase-1].md               # 2,000–5,000 chars each
│   ├── [phase-2].md
│   ├── [decision-framework].md
│   └── [constraints].md
│
├── reference/                      # WHAT the system knows
│   ├── [methodology].md           # 2,000–5,000 chars each
│   ├── [examples].md
│   ├── [patterns].md
│   └── [troubleshooting].md
│
├── supporting/
│   ├── quick-reference.md         # Most common lookups
│   └── terminology-index.md       # Terms → domains
│
├── TEST-MATRIX.md                  # 25+ verification queries
│
└── README.md                       # Deployment instructions
```

### 10.2 Deployment Process

```
┌──────────────────────────────────────────────────────────────────────────┐
│                       DEPLOYMENT WORKFLOW                                 │
├──────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  Step 1: Create Claude Project                                           │
│  ├── Navigate to Claude Projects                                        │
│  ├── Create new project                                                 │
│  └── Name appropriately                                                 │
│                                                                          │
│  Step 2: Set Instructions                                                │
│  ├── Copy INSTRUCTIONS.md content                                       │
│  ├── Paste into Project Instructions field                              │
│  └── Save                                                               │
│                                                                          │
│  Step 3: Upload Knowledge Files                                          │
│  ├── Upload all files from operational/                                 │
│  ├── Upload all files from reference/                                   │
│  └── Upload all files from supporting/                                  │
│                                                                          │
│  Step 4: Verify                                                          │
│  ├── Run TEST-MATRIX.md queries                                         │
│  ├── Expand project_knowledge_search output                             │
│  ├── Verify correct files retrieved                                     │
│  └── Iterate if failures detected                                       │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
```

---

## 11. Version History

| Version | Key Changes |
|---------|-------------|
| **v3.4** | YAML frontmatter, tiered source authority, cross-cutting domains, prompt templates, supporting file templates |
| **v3.3** | Three-layer architecture, conductor pattern, universal decomposition |
| **v3.2** | Prompt classification, hybrid templates |
| **v3.1** | Chunk resilience, concrete query examples |
| **v3.0** | Initial chunk-aware architecture |

> **Full changelog with migration guides:** See [CHANGELOG.md](../CHANGELOG.md)
> **Detailed release notes:** See [docs/release-notes/](release-notes/)

---

## 12. Glossary

| Term | Definition |
|------|------------|
| **BM25** | Best Matching 25 — keyword-based ranking algorithm |
| **Chunk** | ~400–800 token segment of content indexed for retrieval |
| **Conductor Pattern** | Design where Instructions route to content rather than containing it |
| **Contextual Retrieval** | Anthropic's hybrid retrieval system combining embeddings and BM25 |
| **Embedding** | Vector representation of text capturing semantic meaning |
| **Inverted Pyramid** | Writing structure with most important information first |
| **Operational Knowledge** | HOW content — processes, workflows, decisions |
| **Paragraph Autonomy** | Writing paragraphs that make sense if retrieved alone |
| **Rank Fusion** | Combining multiple ranking signals into a final score |
| **Reference Knowledge** | WHAT content — methodology, examples, patterns |
| **Semantic Hedging** | Including synonyms and variations to improve embedding matches |
| **Three-Layer Architecture** | Instructions + Operational + Reference decomposition |

---

*Document generated by Progressive Disclosure Knowledge System Generator v3.4*
