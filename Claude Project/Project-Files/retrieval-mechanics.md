---
domain: retrieval-mechanics
type: reference
semantic_triggers: retrieval, contextual retrieval, how it works,
  embeddings, BM25, chunking, rank fusion, search
related_domains:
  - chunk-resilience.md
  - semantic-hedging.md
  - bm25-optimisation.md
  - rank-fusion.md
last_updated: 2025-01
---

# Retrieval Mechanics — How Claude Projects Contextual Retrieval Works

How does Claude Projects retrieve information? This reference explains the Contextual Retrieval system, including embeddings, BM25, chunking, and rank fusion. Use this when you need to understand what you're optimising for.

Key terms: Contextual Retrieval, embeddings, BM25, chunking, chunks, rank fusion, semantic search, keyword search, project_knowledge_search

## System Overview

Claude Projects uses Anthropic's Contextual Retrieval system. Contextual Retrieval combines multiple retrieval signals to find relevant content. The system does NOT retrieve whole files—it retrieves chunks of approximately 400-800 tokens (roughly 1,500-3,000 characters).

### The Two Retrieval Signals

Contextual Retrieval in Claude Projects uses two complementary search methods:

**Semantic Embeddings:** The system converts both your content and the search query into high-dimensional vectors. Content that is semantically similar to the query (even without exact keyword matches) scores well. Embeddings excel at understanding meaning and handling synonyms.

**BM25 Keyword Search:** The system also performs traditional keyword matching using the BM25 algorithm. Content containing the exact terms from the query scores well. BM25 excels at matching specific terminology, error codes, and technical terms.

### Rank Fusion

Results from embedding search and BM25 are combined using rank fusion (likely Reciprocal Rank Fusion). Both signals contribute to the final ranking. A chunk that ranks #3 on both signals will typically beat a chunk that ranks #1 on one signal but #20 on the other. This means you must optimise for BOTH signals, not just one.

## How Chunking Works

The retrieval system chunks your uploaded files into segments of approximately 400-800 tokens. Each chunk receives a pre-computed contextual summary that gets prepended during indexing. This contextual summary helps the chunk be understood even when retrieved in isolation.

**Critical Reality:** You cannot control where chunk boundaries fall. Different chunking strategies exist (fixed-size, recursive, semantic, markdown-aware), and you don't know which Anthropic uses. Your carefully-structured 2,000-character section might get split at character 1,500 anyway.

**Optimisation Strategy:** Don't try to align your content with chunk boundaries. Instead, write content that remains useful regardless of where boundaries fall. This is chunk-boundary resilience, covered in detail in `mechanics/chunk-resilience.md`.

## How Query Generation Works

When a user asks a question, Claude generates a search query for the `project_knowledge_search` tool. This generated query is what the retrieval system actually searches for. Project Instructions influence this query generation—they bias what query Claude formulates.

**Important:** Project Instructions don't modify the retrieval index. They only influence the search queries Claude generates. Instructions are a nudge, not a guarantee. Query generation is stochastic.

**Optimisation Strategy:** Include concrete query examples in your Project Instructions. Claude learns better from examples than abstract rules. See `phases/phase-3-instructions.md` for detailed guidance.

## Observable Verification

You can observe what chunks were retrieved by expanding the `project_knowledge_search` tool output in Claude's response. This observability is your primary debugging tool. When retrieval goes wrong, check:

1. What search query did Claude generate?
2. What chunks were retrieved?
3. Were the expected chunks missing?

This feedback loop lets you adjust either Project Instructions (if query generation was bad) or domain file content (if chunks didn't match the query).

## Summary Table

| Mechanism | What It Does | How to Optimise |
|-----------|--------------|-----------------|
| Embeddings | Matches semantic meaning | Natural language, synonyms, question echoes |
| BM25 | Matches exact keywords | Specific terms, error codes, technical vocabulary |
| Rank Fusion | Combines both signals | Balance both—don't over-optimise one |
| Chunking | Splits content into ~400-800 token pieces | Paragraph autonomy, distributed keywords |
| Query Generation | Claude formulates search queries | Concrete examples in Instructions |
| Contextual Summaries | Prepended to chunks during indexing | Front-load context in every paragraph |

## Related References

- `mechanics/chunk-resilience.md` — How to write chunk-boundary resilient content
- `mechanics/semantic-hedging.md` — Optimising for embedding similarity
- `mechanics/bm25-optimisation.md` — Optimising for keyword matching
- `mechanics/rank-fusion.md` — Balancing the two signals
