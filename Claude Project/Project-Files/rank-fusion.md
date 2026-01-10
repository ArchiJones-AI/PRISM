# Rank Fusion — Balancing Embedding and BM25 Signals

How do I balance semantic and keyword optimisation? This reference covers rank fusion: how Claude Projects combines embedding similarity and BM25 keyword matching into a final retrieval ranking, and how to write content that scores well on both signals.

Key terms: rank fusion, reciprocal rank fusion, RRF, hybrid retrieval, embedding signal, BM25 signal, balanced optimisation, over-optimisation

## How Rank Fusion Works

Claude Projects' Contextual Retrieval uses hybrid search: it runs both embedding (semantic) search and BM25 (keyword) search, then combines the results using rank fusion.

The most common rank fusion method is Reciprocal Rank Fusion (RRF), which works roughly like this:

1. Embedding search returns chunks ranked by semantic similarity
2. BM25 search returns chunks ranked by keyword match
3. RRF combines rankings: chunks that rank well on BOTH signals get boosted

### The Key Implication

**A chunk ranking #3 on both signals will typically beat a chunk ranking #1 on one signal but #20 on the other.**

This means:
- You can't ignore either signal
- Balance is more important than maximising one signal
- Over-optimising for keywords hurts embeddings (and vice versa)

## The Balance Principle

### What Hurts Embeddings

Keyword-stuffed content that reads unnaturally:

```
OAuth OAuth authentication OAuth login OAuth SSO OAuth configuration 
OAuth setup OAuth credentials OAuth error OAuth troubleshooting OAuth.
```

This might score well on BM25 for "OAuth" queries, but the embedding will be bizarre. The semantic signal will be weak because this doesn't read like natural language about authentication.

### What Hurts BM25

Pure prose with no specific terminology:

```
When users want to access the system using their existing accounts from 
other services, they can connect those external accounts to enable a 
smoother sign-in experience. This eliminates the need to remember 
additional passwords and streamlines the process of getting into the 
application.
```

This embeds well (it's natural language about authentication) but contains zero domain keywords. BM25 queries for "OAuth", "SSO", or "authentication" won't match.

### The Balanced Approach

Natural language with distributed specific terms:

```
OAuth (Open Authorization) allows users to sign in using their existing 
accounts from identity providers like Google or Microsoft. This SSO 
(single sign-on) approach eliminates separate passwords. How do I set 
up OAuth? Configure your OAuth credentials in the Admin Console under 
Authentication > OAuth Settings to enable third-party authentication.
```

This content:
- Reads naturally (good embedding signal)
- Contains specific terms: OAuth, SSO, single sign-on, authentication (good BM25)
- Includes a question echo (boosts embedding similarity to user questions)
- Uses terms in context (better embedding than keyword-stuffing)

## Optimisation Strategy

### Rule of Thumb

Write naturally, then ensure keywords are present. Don't sacrifice readability for keyword density.

**Process:**
1. Write the content in natural, clear language
2. Check: Are domain-specific terms present every 500-800 characters?
3. Check: Is the primary term used 2-3 times?
4. Check: Do variations and synonyms appear?
5. If any check fails, weave in terms naturally—don't force them

### The 60/40 Mental Model

When optimising content, think:
- 60% readability and semantic clarity
- 40% keyword presence and distribution

If you have to choose between slightly more natural prose vs slightly more keyword presence, lean toward natural prose. Embeddings suffer more from unnatural text than BM25 benefits from extra keyword repetition.

## Detecting Imbalance

### Signs of Over-Optimised Keywords

- Content sounds robotic or repetitive
- Same term appears more than 5 times in a section
- Sentences feel forced or awkward
- A human would find it annoying to read

### Signs of Under-Optimised Keywords

- Technical terms are absent or rare
- Generic language dominates ("the system", "the process", "the settings")
- Error messages are paraphrased instead of verbatim
- Domain vocabulary appears only in headings, not body text

## Testing for Balance

When verifying your knowledge system (Phase 6), test with BOTH query types:

**Keyword queries:** Use exact terms like "OAuth configuration" or "401 Unauthorized"
- If these fail, your content lacks keywords

**Semantic queries:** Use natural language like "let users sign in with Google"
- If these fail, your content lacks semantic clarity

Good content retrieves well for BOTH types of queries.

## Balance Checklist

For each section, verify:

- [ ] Content reads naturally (would a human find it clear?)
- [ ] Primary domain term appears 2-3 times (not more)
- [ ] Variations and synonyms are woven in naturally
- [ ] No keyword-stuffing (same term repeated excessively)
- [ ] Technical terms present throughout (not generic language only)
- [ ] Question-form echoes included (boosts semantic signal)
- [ ] Error messages verbatim (boosts keyword signal for troubleshooting)

## Related References

- `mechanics/retrieval-mechanics.md` — How hybrid retrieval works overall
- `mechanics/semantic-hedging.md` — Detailed embedding optimisation
- `mechanics/bm25-optimisation.md` — Detailed keyword optimisation
- `phases/phase-5-hybrid-optimisation.md` — Applying balance in practice
