---
domain: bm25-optimisation
type: reference
semantic_triggers: BM25, keyword, keyword matching, term frequency,
  TF-IDF, exact match, verbatim, keyword density
related_domains:
  - retrieval-mechanics.md
  - semantic-hedging.md
  - rank-fusion.md
last_updated: 2025-01
---

# BM25 Optimisation — Keyword Matching for Retrieval

How do I optimise content for keyword search? This reference covers BM25 mechanics and how to write content that scores well on keyword matching queries. BM25 is the keyword-based component of Claude Projects' hybrid retrieval system.

Key terms: BM25, keyword matching, term frequency, TF-IDF, inverse document frequency, document length, keyword density, exact match

## How BM25 Works

BM25 (Best Matching 25) is a ranking function that scores documents based on keyword matching. Understanding its mechanics helps you write content that ranks well.

### Term Frequency (TF)

More occurrences of a search term = higher score, but with diminishing returns. The first occurrence of "OAuth" in your content contributes significantly. The second and third occurrences help, but less so. The tenth occurrence adds almost nothing.

**Practical guideline:** Use key terms 2-3 times per section. More than that provides diminishing returns and risks making content feel keyword-stuffed (which hurts embedding quality).

### Inverse Document Frequency (IDF)

Rare terms score higher than common terms. "OAuth" is more valuable for matching than "configuration" because "OAuth" appears in fewer documents overall, making it more discriminating.

**Practical guideline:** Prioritise domain-specific terminology over generic words. "OAuth" beats "settings". "401 Unauthorized" beats "error". Technical terms and exact error codes have high IDF value.

### Document Length Normalisation

Shorter documents get boosted relative to longer ones (all else being equal). A 2,000-character section with 3 mentions of "OAuth" scores better than a 6,000-character section with 3 mentions.

**Practical guideline:** Keep sections focused. Don't pad content with filler. Each section should cover one topic thoroughly but concisely.

## BM25 Optimisation Strategies

### Strategy 1: Include Exact Technical Terms

BM25 matches exact terms. Include the precise terminology users will search for.

**Include:**
- Technical terms: OAuth, SAML, OIDC, JWT, MFA
- Product/feature names: Admin Console, User Portal, API Gateway
- Configuration parameters: client_id, redirect_uri, scope
- API endpoints: /auth/token, /users/me
- Status codes: 200 OK, 401 Unauthorized, 403 Forbidden

### Strategy 2: Include Common Variations

Users type things differently. Include variations to catch different search patterns.

**For "authentication":**
- authentication
- auth
- login
- log in
- signin
- sign-in
- sign in

**For error codes:**
- 401
- 401 error
- 401 Unauthorized
- HTTP 401
- Error 401

### Strategy 3: Include VERBATIM Error Messages

Users copy-paste exact error messages into searches. Include error text exactly as it appears.

**Bad (paraphrased):**
```
If you see an unauthorized error, check your credentials.
```

**Good (verbatim):**
```
Error: 401 Unauthorized - Invalid or expired token

If you see the error "401 Unauthorized - Invalid or expired token", 
your authentication credentials have expired. Generate a new access 
token to resolve this 401 error.
```

The verbatim error message ensures BM25 matches when users search the exact error text.

### Strategy 4: Distribute Keywords Throughout

Don't front-load all keywords in the opening. Distribute them every 500-800 characters so every potential chunk contains relevant terms.

**Why:** If chunk boundaries split your content, you want each resulting chunk to contain keywords. A chunk with no domain terms won't match keyword queries.

See `mechanics/chunk-resilience.md` for detailed guidance on keyword distribution.

### Strategy 5: Keywords in High-Weight Positions

BM25 implementations often weight term position. Terms appearing in certain positions may receive higher scores.

**High-weight positions:**
- Section headings (H1, H2, H3)
- First sentence of sections
- First sentence of paragraphs
- Bold/emphasised text (in some implementations)

Place your most important keywords in these positions:

```
## OAuth Authentication Troubleshooting

OAuth authentication errors typically occur when tokens expire or 
credentials are misconfigured. The most common OAuth error is 
"401 Unauthorized - Invalid token"...
```

"OAuth", "authentication", and "401 Unauthorized" all appear in high-weight positions.

## Keyword Density Guidelines

| Section Length | Recommended Keyword Mentions |
|---------------|------------------------------|
| ~1,000 chars | 2-3 mentions of primary term |
| ~2,000 chars | 3-4 mentions of primary term |
| ~3,000 chars | 4-5 mentions of primary term |

Beyond these counts, you hit diminishing returns. Focus additional space on variations and related terms rather than repeating the same keyword.

## BM25 Optimisation Checklist

For each domain file section, verify:

- [ ] Primary domain term appears 2-3 times
- [ ] Key term appears in section heading
- [ ] Key term appears in first sentence
- [ ] Keywords distributed throughout (not front-loaded only)
- [ ] Exact error messages included verbatim
- [ ] Technical terms included (high IDF value)
- [ ] Common variations/abbreviations included
- [ ] Section is focused (not padded with filler)

## Common BM25 Mistakes

**Keyword deserts:** Sections of flowing prose with no technical terms. These sections won't match keyword queries.

**Over-stuffing:** Repeating the same term 10+ times. Diminishing returns kick in, and the content sounds unnatural (hurting embedding quality).

**Paraphrased errors:** Writing "authentication error" when users search for "401 Unauthorized". Exact matches matter.

**Generic terms only:** Using "settings" and "configuration" without specific terms like "OAuth" or "SAML". Generic terms have low IDF and don't discriminate.

## Related References

- `mechanics/retrieval-mechanics.md` — How BM25 fits into hybrid retrieval
- `mechanics/semantic-hedging.md` — Complementary embedding optimisation
- `mechanics/rank-fusion.md` — Balancing keyword and semantic signals
- `mechanics/chunk-resilience.md` — Keyword distribution across chunks
