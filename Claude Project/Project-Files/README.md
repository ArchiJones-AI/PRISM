# Progressive Disclosure Knowledge System Generator v3.4

Transform monolithic prompts of ANY size into Claude Projects-optimised knowledge systems.

## What's New in v3.4

v3.4 introduces metadata and source authority patterns:
- **YAML Frontmatter:** Machine-readable metadata in all domain files (semantic triggers, source authority, related domains)
- **Tiered Source Authority:** 4-tier system for ranking authoritative sources
- **Cross-Cutting Domains:** Explicit guidance for decision matrices and bridging files
- **Prompt Templates:** Ready-to-use prompts in reference files for user experience

## v3.3 Core Architecture

**Core insight:** Instructions should be a CONDUCTOR, not a CONTAINER.

Previous versions preserved operational logic in Instructions. This broke for large prompts where operational content itself exceeded reasonable instruction sizes.

v3.3 introduced:
- **Three-layer architecture:** Instructions + Operational Knowledge + Reference Knowledge
- **Conductor pattern:** Instructions route to content; they don't hold content
- **Universal decomposition:** BOTH operational AND reference content become files
- **Size-aware design:** Target 4,000-8,000 char Instructions regardless of source size

---

## The Three-Layer Architecture

```
┌─────────────────────────────────────────────────────────────┐
│ LAYER 1: INSTRUCTIONS (4,000-8,000 chars)                   │
│                                                             │
│ The Conductor — knows what exists, routes to it:            │
│ • Irreducible core purpose (~300 chars)                     │
│ • Content index (~1,500 chars)                              │
│ • Query routing (~3,000 chars)                              │
│ • Edge case handling (~500 chars)                           │
└─────────────────────────────────────────────────────────────┘
                          │
          ┌───────────────┴───────────────┐
          ▼                               ▼
┌─────────────────────────┐   ┌─────────────────────────┐
│ LAYER 2: OPERATIONAL    │   │ LAYER 3: REFERENCE      │
│                         │   │                         │
│ HOW the system works:   │   │ WHAT the system knows:  │
│ • Phases & workflows    │   │ • Methodology           │
│ • Decision frameworks   │   │ • Examples & templates  │
│ • Principles            │   │ • Patterns              │
│ • Constraints           │   │ • Definitions           │
│ • Output specs          │   │ • Troubleshooting       │
└─────────────────────────┘   └─────────────────────────┘
```

**Key insight:** A 60,000-character prompt becomes:
- ~6,000-character Instructions (routing)
- ~20,000 characters of operational files (decomposed workflows)
- ~34,000 characters of reference files (decomposed knowledge)

The system scales to ANY source size while keeping Instructions slim.

---

## How It Works

### Phase 0: Classify

Before decomposing, understand what you have:
- Measure total source size
- Tag content as OPERATIONAL (how) or REFERENCE (what)
- Identify the irreducible core
- Design the file architecture

### Phase 1-2: Ingest & Architect

Extract and structure content by layer:
- Group operational content into process domains
- Group reference content into knowledge domains
- Map connections between layers
- Get user approval on architecture

### Phase 3: Create Instructions

Build the routing layer using `routing-instruction-template.md`:
- Irreducible purpose (what the system fundamentally does)
- Content index (what files exist)
- Query routing (when to retrieve what)
- Edge cases (what to do when routing fails)

**Target:** 4,000-8,000 characters. If longer, you're not decomposing enough.

### Phase 4-5: Create Knowledge Files

Write operational files using `operational-domain-template.md`:
- Processes in imperative voice
- Decision frameworks with specific criteria
- Principles and constraints

Write reference files using `domain-file-template.md`:
- Methodology with semantic hedging
- Examples with distributed keywords
- Patterns with chunk resilience

### Phase 6-7: Verify & Package

Test retrieval accuracy:
- Can operational processes be retrieved when needed?
- Can reference knowledge be retrieved when needed?
- Do complex tasks retrieve from both layers correctly?

---

## The Conductor Pattern

Instructions don't contain logic—they ROUTE to logic.

### What Instructions Contain

```markdown
## Query Routing

| Situation | Retrieve | Search Query |
|-----------|----------|--------------|
| Starting transformation | intake-phase.md | "intake analysis parse" |
| Need output structure | xml-blueprint.md | "XML blueprint structure" |
| Avoiding failures | common-whiffs.md | "whiff failure avoid" |
```

### What Instructions Don't Contain

```markdown
## Intake Phase  ❌ WRONG — This belongs in a file

Step 1: Parse explicit requirements...
Step 2: Infer implicit requirements...
Step 3: Classify complexity...
[...2,000 characters of process detail...]
```

If it's more than a few sentences, it's a knowledge file, not an instruction.

---

## File Structure

```
{system-name}/
├── INSTRUCTIONS.md                 # Routing layer
├── operational/                    # HOW files
│   ├── intake-phase.md
│   ├── analysis-phase.md
│   ├── generation-phase.md
│   └── decision-framework.md
├── reference/                      # WHAT files
│   ├── xml-blueprint.md
│   ├── lever-configuration.md
│   ├── model-selection.md
│   ├── prompt-examples.md
│   └── common-whiffs.md
├── supporting/
│   ├── quick-reference.md
│   └── terminology-index.md
├── TEST-MATRIX.md
└── README.md
```

---

## Deployment

### Step 1: Create Claude Project

1. Go to Claude Projects
2. Create new project
3. Name appropriately

### Step 2: Set Instructions

1. Copy INSTRUCTIONS.md content
2. Paste into Project Instructions field
3. Save

### Step 3: Upload Knowledge Files

Upload ALL files from:
- `operational/` folder
- `reference/` folder
- `supporting/` folder

### Step 4: Verify

Run TEST-MATRIX.md queries:
1. Check `project_knowledge_search` output
2. Verify correct files retrieved
3. Fix routing or content if needed

---

## Key Principles

### 1. Instructions Are Conductors

They know the orchestra (content index) and when each section plays (query routing). They don't contain every note.

### 2. Both Layers Get Decomposed

Operational content (workflows, decisions) AND reference content (methodology, examples) become retrievable files. Neither lives in Instructions except as routing pointers.

### 3. Size Controls Decomposition

Target: Instructions ~6,000 chars, individual files 2,000-5,000 chars.

If Instructions exceed 8,000 chars → decompose more operational content
If a file exceeds 5,000 chars → split into sub-domains

### 4. Test Both Retrieval Types

Operational retrieval: "What's the process for X?" → retrieves workflow
Reference retrieval: "What methodology for Y?" → retrieves knowledge
Combined: Complex tasks retrieve from both layers

---

## Version History

- **v3.4** — YAML frontmatter, tiered source authority, cross-cutting domains, prompt templates
- **v3.3** — Three-layer architecture, conductor pattern, universal decomposition
- **v3.2** — Prompt classification, hybrid templates
- **v3.1** — Chunk resilience, concrete query examples
- **v3.0** — Initial chunk-aware architecture

> **Full changelog with migration guides:** See [CHANGELOG.md](../../CHANGELOG.md)
