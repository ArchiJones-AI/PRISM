---
domain: chunk-resilience
type: reference
semantic_triggers: chunk, boundary, resilience, paragraph autonomy,
  inverted pyramid, keyword distribution, self-contained, splits
related_domains:
  - retrieval-mechanics.md
  - bm25-optimisation.md
  - semantic-hedging.md
last_updated: 2025-01
---

# Chunk Boundary Resilience — Writing Content That Survives Arbitrary Splits

How do I write content that works regardless of chunk boundaries? This reference covers the critical principle of chunk-boundary resilience: writing so that ANY arbitrary split still produces useful, retrievable chunks.

Key terms: chunk boundary, chunk resilience, paragraph autonomy, inverted pyramid, keyword distribution, self-contained, cross-reference, context-free

## The Core Reality

You CANNOT control where chunk boundaries fall. Chunking strategies vary between systems (fixed-size, recursive character splitting, semantic chunking, markdown-aware chunking). Anthropic's exact chunking method is unknown. Your carefully-sized 2,000-character section might get split at character 1,500 anyway.

**Therefore:** Don't optimise for chunk boundary alignment. Optimise for chunk boundary RESILIENCE. Write content so that ANY arbitrary split still produces useful chunks.

## Strategy 1: Paragraph Autonomy

Every PARAGRAPH should be useful if retrieved alone—not just every section. If a chunk boundary falls mid-section, each resulting chunk should still be retrievable and useful.

### Bad Example (Context-Free Opening)
```
To do this, follow these steps:
1. Open the settings panel
2. Navigate to Authentication
3. Enable OAuth
```

If this paragraph becomes its own chunk, the retrieval system has no idea what "this" refers to. The chunk is useless in isolation.

### Good Example (Context in Every Paragraph)
```
To configure OAuth authentication in the Admin Console, follow these steps:
1. Open the Admin Console settings panel
2. Navigate to Authentication > OAuth
3. Enable the OAuth integration toggle
```

Now even if retrieved alone, the chunk clearly communicates what it's about. The contextual summary will capture "OAuth authentication" and "Admin Console".

## Strategy 2: Keyword Distribution

Distribute key terms every 500-800 characters throughout your content. Do NOT front-load all keywords in the opening paragraph only.

### Why This Matters
BM25 keyword matching happens on whatever chunk contains the term. If all your domain terms appear only in the first paragraph, and the chunk boundary falls after that paragraph, subsequent chunks won't match keyword queries.

### Bad Example (Front-Loaded Keywords)
```
# OAuth Authentication — SSO, Single Sign-On, Identity Provider Setup

OAuth (Open Authorization) enables SSO (single sign-on) through identity 
providers (IdP) like Google, Microsoft, and Okta. This covers SAML, OIDC, 
OpenID Connect, client credentials, and redirect URIs.

## Setting Up OAuth

To set up the integration, first navigate to the settings panel. Open the 
authentication section and look for the third-party options. You'll see 
several choices for connecting external services. Select the appropriate 
provider and enter your credentials. The system will validate the connection 
and confirm when ready.
```

The second section has almost no domain terminology. If that section becomes its own chunk, BM25 won't match queries about "OAuth" or "SSO".

### Good Example (Distributed Keywords)
```
# OAuth Authentication — SSO, Single Sign-On, Identity Provider Setup

OAuth (Open Authorization) enables SSO (single sign-on) through identity 
providers (IdP) like Google, Microsoft, and Okta. This covers SAML, OIDC, 
OpenID Connect, client credentials, and redirect URIs.

## Setting Up OAuth in the Admin Console

To set up OAuth authentication, navigate to the Admin Console settings. 
Open the Authentication section and locate the OAuth/SSO configuration 
options. You'll see identity provider choices including Google OAuth, 
Microsoft Azure AD, and Okta SAML. Select your OAuth provider and enter 
the client ID and client secret credentials. The OAuth system validates 
the identity provider connection and confirms when SSO is ready.
```

Now every paragraph contains domain terms. Any chunk will match relevant queries.

## Strategy 3: Inverted Pyramid Structure

Structure every paragraph using the inverted pyramid from journalism:

1. **First sentence:** The answer or key point
2. **Second sentence:** Essential context
3. **Third+ sentences:** Supporting details

If the chunk gets cut short, the most important information was already delivered.

### Bad Example (Buried Lead)
```
When users attempt to access the system and encounter issues with their 
credentials, particularly after a period of inactivity or when switching 
between devices, they may notice that their access is no longer valid. 
This typically occurs because session tokens expire after 24 hours of 
inactivity. To resolve expired session issues, users should log out 
completely and log back in to obtain fresh authentication tokens.
```

The actual answer (log out and back in) is buried at the end.

### Good Example (Inverted Pyramid)
```
To resolve expired session issues, log out completely and log back in to 
obtain fresh authentication tokens. Session tokens in the system expire 
after 24 hours of inactivity, which causes access to become invalid. This 
commonly occurs when users switch between devices or return after a period 
of not using the system.
```

The answer comes first. If the chunk gets truncated, users still have the solution.

## Strategy 4: No Cross-References

Never write content that references other sections:

- ❌ "As mentioned above..."
- ❌ "See the previous section..."
- ❌ "We'll cover this below..."
- ❌ "The following section explains..."
- ❌ "Refer to the authentication section for details..."

The retrieved chunk might not include "above" or "below". Cross-references become dangling pointers.

### Instead: Restate Context
If information from another section is needed, briefly restate it rather than referencing it. Each paragraph should be self-contained.

## Chunk Resilience Checklist

Before finalising any domain file, verify:

- [ ] Every PARAGRAPH opens with context-setting sentence (not just sections)
- [ ] Domain terms appear every 500-800 characters throughout
- [ ] Keywords are NOT concentrated only in the opening
- [ ] Every paragraph uses inverted pyramid (answer first)
- [ ] No cross-references ("as mentioned above", "see below")
- [ ] Procedures are complete within paragraphs (not split across)
- [ ] Any paragraph, if retrieved alone, would make sense

## Related References

- `mechanics/retrieval-mechanics.md` — How chunking and retrieval work
- `mechanics/bm25-optimisation.md` — Keyword distribution for BM25 matching
- `phases/phase-4-domain-files.md` — Full domain file creation guidance
- `anti-patterns.md` — Common chunk-related mistakes to avoid
