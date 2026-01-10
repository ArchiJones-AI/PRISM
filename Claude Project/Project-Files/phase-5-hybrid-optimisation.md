# Phase 5: Hybrid Optimisation — Balancing Retrieval Signals

How do I ensure content scores well on both retrieval signals? This reference covers Phase 5: final review of domain files to balance embedding similarity and BM25 keyword matching.

Key terms: hybrid optimisation, balance, embedding signal, BM25 signal, rank fusion, natural language, keyword density

## Phase 5 Overview

Phase 5 is a final review pass ensuring domain files balance both retrieval signals. Content should score well on embeddings AND BM25 without over-optimising either.

**These optimisation principles apply to BOTH operational and reference files.** Whether the file contains workflows (operational) or knowledge (reference), the same balance between keyword and semantic signals is required.

**Deliverable:** Optimised operational and reference files ready for deployment.

**Before starting:** Load `mechanics/rank-fusion.md` for balance principles.

## The Balance Goal

Rank fusion combines embedding and BM25 rankings. A chunk ranking #3 on both signals typically beats a chunk ranking #1 on one but #20 on the other.

**Therefore:** Moderate performance on both signals beats excellent performance on one signal alone.

## Review Process

### Step 1: Readability Pass

Read each domain file as a human would. Ask:

- Does this read naturally?
- Would a human find it clear and helpful?
- Are there awkward phrasings forced by keyword insertion?

**If content sounds robotic or forced:** You've over-optimised for keywords. Rewrite to be more natural while keeping key terms present but not forced.

### Step 2: Keyword Density Check

For each section, verify:

- [ ] Primary domain term appears 2-3 times (not more)
- [ ] Term appears in heading
- [ ] Term appears in first sentence
- [ ] Term appears every 500-800 characters
- [ ] Variations/synonyms present
- [ ] No keyword stuffing (same term 5+ times)

**If keyword-sparse:** Add domain terms naturally. "The system validates..." → "The OAuth system validates..."

**If keyword-stuffed:** Remove repetition. Natural inclusion of terms, not forced repetition.

### Step 3: Semantic Coverage Check

For each section, verify:

- [ ] Natural language that reads well
- [ ] Question echoes present
- [ ] Synonyms and variations included
- [ ] Complete sentences (not keyword lists)

**If robotic/stilted:** Rewrite for natural flow. You can include keywords while still reading naturally.

### Step 4: Verbatim Check

For troubleshooting sections:

- [ ] Error messages are VERBATIM (exact text users see)
- [ ] Error codes included (401, 403, ERR_*)
- [ ] Status messages included exactly

**If paraphrased:** Replace with exact error text. Users search verbatim.

## Balance Indicators

### Signs of Good Balance ✅

- Content reads naturally to a human
- Domain terms appear 2-3 times per section
- Terms are woven into sentences, not listed
- Question echoes feel natural, not forced
- A mix of technical terms and plain language

### Signs of Over-Optimised Keywords ❌

- Same term appears 5+ times in a section
- Sentences sound forced or awkward
- Content feels like a keyword list
- Repetitive phrasing
- Human would find it annoying to read

### Signs of Under-Optimised Keywords ❌

- Generic language dominates ("the system", "the process")
- Technical terms only in headings, not body text
- Error messages paraphrased
- Long stretches without domain vocabulary
- Variations/synonyms missing

## The Combined Pattern

Well-balanced content follows this pattern:

```markdown
## [Topic Heading with Primary Term]

[Natural opening with question echo]. [Primary term] ([variation], also 
known as [synonym]) allows you to [purpose]. This section covers [topic] 
configuration, common [term] issues, and [related concept].

[Paragraph with natural language. Includes primary term mention. Explains 
concept clearly. Uses term again in context. Maintains readability.]

[Continuation paragraph. Primary term woven in naturally. Technical detail 
with specific terms. Complete thoughts in readable sentences.]
```

**Example:**

```markdown
## Configuring OAuth Identity Providers

How do I add an identity provider? OAuth identity providers (IdPs) like 
Google, Microsoft, and Okta allow users to sign in using existing accounts. 
This section covers adding OAuth providers, configuring SSO connections, 
and testing identity provider integration.

To add an OAuth identity provider, navigate to Admin Console > Authentication 
> Identity Providers. Click "Add Provider" and select your OAuth provider 
type. The system supports Google OAuth, Microsoft Azure AD, Okta SAML, and 
generic OIDC providers for flexible identity provider options.

Enter the OAuth credentials (client ID and client secret) from your identity 
provider. Configure the redirect URI to match your IdP registration. The 
OAuth connection requires exact URI matching, so verify both locations show 
identical URLs including protocol and trailing slashes.
```

**This achieves:**
- Natural, readable prose
- "OAuth" appears 6 times across 3 paragraphs (distributed, not stuffed)
- Variations: IdP, identity provider, SSO, SAML, OIDC
- Question echo in opening
- Technical specifics (client ID, redirect URI)
- Complete, clear sentences

## Quick Reference: Term Frequency Guidelines

| Section Length | Primary Term Target | Variation Target |
|---------------|--------------------|--------------------|
| ~1,000 chars | 2-3 mentions | 2-3 variations |
| ~2,000 chars | 3-4 mentions | 3-4 variations |
| ~3,000 chars | 4-5 mentions | 4-5 variations |

Beyond these counts, focus on natural prose rather than additional repetition.

## Phase 5 Checklist

Before finalising domain files:

**Readability:**
- [ ] Content reads naturally (human would find it clear)
- [ ] No awkward phrasing from forced keywords
- [ ] Complete sentences throughout

**Keyword Balance:**
- [ ] Primary term 2-3 times per section (not more)
- [ ] Terms distributed (not front-loaded)
- [ ] Variations and synonyms present
- [ ] No keyword stuffing

**Semantic Balance:**
- [ ] Question echoes present
- [ ] Natural language (not keyword lists)
- [ ] Semantic neighbourhood covered

**Troubleshooting:**
- [ ] Error messages verbatim
- [ ] Error codes included
- [ ] Specific solutions with domain terms

## Next Phase

Proceed to **Phase 6: Verification** — Test retrieval accuracy with `project_knowledge_search`.

## Related References

- `mechanics/rank-fusion.md` — Balance principles in detail
- `mechanics/semantic-hedging.md` — Embedding optimisation
- `mechanics/bm25-optimisation.md` — Keyword optimisation
- `phases/phase-6-verification.md` — Testing your optimisation
