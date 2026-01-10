# Phase 4: Create Domain Files — Chunk-Resilient Content

How do I write domain files optimised for retrieval? This reference covers Phase 4: creating REFERENCE domain files with chunk-boundary resilience, distributed keywords, semantic hedging, and inverted pyramid structure.

Key terms: domain files, reference files, chunk resilience, paragraph autonomy, keyword distribution, semantic hedging, inverted pyramid, verbatim errors

## File Types in This Phase

**Reference Files:** Covered in this document. These contain WHAT the system knows—methodology, examples, definitions, troubleshooting.

**Operational Files:** See `templates/operational-domain-template.md`. These contain HOW the system works—workflows, processes, decision frameworks. Operational files use imperative voice and focus on actions.

---

## Phase 4 Overview

Reference files contain the actual knowledge that gets chunked and indexed. Write content that remains useful regardless of where chunk boundaries fall.

**Deliverable:** All reference files in /reference/ directory

**Before starting:** Load `mechanics/chunk-resilience.md` for detailed principles.

## Core Writing Principles

### Principle 1: Paragraph Autonomy

Every PARAGRAPH must be useful if retrieved alone—not just every section.

**Test:** Take any paragraph from your content. If that paragraph were the only thing retrieved, would it make sense? Would it answer a query?

**Implementation:**
- Open every paragraph with context (what domain, what topic)
- Don't rely on previous paragraphs for meaning
- Complete thoughts within paragraphs

### Principle 2: Distributed Keywords

Place domain terms every 500-800 characters throughout content. Don't front-load all keywords in the opening only.

**Test:** Take characters 1500-2500 of your content. Does this section contain domain keywords? Would it match a keyword query?

**Implementation:**
- Mention primary term 2-3 times per section
- Sprinkle variations throughout
- Every paragraph should contain at least one domain term

### Principle 3: Inverted Pyramid

Structure every paragraph: answer first, context second, details third.

**Test:** If the paragraph got cut after the first sentence, would the user have the key information?

**Implementation:**
- First sentence = the answer or key point
- Second sentence = essential context
- Remaining sentences = supporting details

### Principle 4: Semantic Hedging

Include synonyms, variations, and question-form echoes within the first 500 characters of each section.

**Test:** If a user searches with a synonym instead of the primary term, will this content still embed close to their query?

**Implementation:**
- Primary term + 2-3 synonyms in opening
- Acronym AND expanded form
- Question echoes embedded

### Principle 5: No Cross-References

Never write "as mentioned above" or "see the previous section". The retrieved chunk won't include "above".

**Test:** Does any sentence reference content outside itself?

**Implementation:**
- Restate context instead of referencing
- Each paragraph is self-contained

## Domain File Structure

Use `templates/domain-file-template.md` for the full template. Key structure:

```markdown
# [Domain Name] — [System Name]

[Opening paragraph with semantic hedging: description + question echoes + 
key terms + variations. Target: 400-600 characters of dense, retrievable 
context.]

## [Topic 1] — [Keywords in Heading]

[Paragraph 1: Context-setting opening + answer first + distributed keywords]

[Paragraph 2: Additional detail with keywords repeated naturally]

---

## [Topic 2] — [Keywords in Heading]

[Continue pattern...]

---

## Troubleshooting [Domain] Issues

[Question echo]? Common [domain] problems and solutions:

### [Error/Symptom] — [VERBATIM error text if applicable]

**Error:** "[Exact error message as users would see it]"

**Cause:** [Explanation with domain terms]

**Solution:** [Steps with domain terms distributed]

---

## Related Topics

- [Related domain 1] — for questions about [topic]
- [Related domain 2] — for questions about [topic]
```

## Opening Paragraph Template

The opening paragraph is critical—it informs the contextual summary for early chunks:

```markdown
# OAuth & SSO Integration — [System Name]

How do I set up OAuth? How do I configure single sign-on? This document 
covers OAuth (Open Authorization) and SSO (single sign-on) integration, 
including identity provider setup, SAML and OIDC configuration, and 
troubleshooting OAuth errors. Use this for questions about third-party 
authentication, social login, Google login, Microsoft Azure AD, and 
federated identity.

Key terms: OAuth, OAuth2, SSO, single sign-on, SAML, OIDC, OpenID Connect, 
identity provider, IdP, social login, federated auth
```

**This opening achieves:**
- Question echoes for embedding similarity
- Semantic description of topics
- Primary terms + variations
- Acronyms AND expanded forms
- Explicit key terms list for BM25

## Section Writing Examples

### Bad Section (Violates Principles)

```markdown
## Configuration

To do this, follow these steps. First, go to the settings. Then find the 
authentication option. Click on it and you'll see some choices. Pick the 
one you need and save. If it doesn't work, try again or check the other 
section for help.
```

**Problems:**
- No context in opening ("To do this" — what?)
- No domain keywords
- Generic language
- Cross-reference ("check the other section")
- No inverted pyramid

### Good Section (Follows Principles)

```markdown
## Configuring OAuth in the Admin Console

To configure OAuth authentication in the Admin Console, navigate to 
Settings > Authentication > OAuth. OAuth configuration requires a client 
ID and client secret from your identity provider. The OAuth setup process 
involves three steps: registering your application with the IdP, entering 
OAuth credentials in the Admin Console, and testing the OAuth connection.

First, register your application with your OAuth identity provider (Google, 
Microsoft, Okta, or another IdP). The provider will give you a client ID 
and client secret for OAuth authentication. These OAuth credentials are 
required for the next step.

Next, in the Admin Console OAuth settings, enter the client ID and client 
secret. Configure the redirect URI to match what you registered with the 
OAuth provider. Save the OAuth configuration.

Finally, test the OAuth integration using the Test Connection button. A 
successful OAuth test will show "Connected" status. If OAuth testing fails, 
check the troubleshooting section for common OAuth errors.
```

**Achieves:**
- Context in opening (OAuth configuration, Admin Console)
- Keywords distributed throughout (OAuth appears in every paragraph)
- Inverted pyramid (answer "navigate to Settings..." comes first)
- Self-contained paragraphs
- No cross-references (says "troubleshooting section" not "see above")

## Troubleshooting Section Template

Troubleshooting sections need VERBATIM error messages:

```markdown
## Troubleshooting OAuth Errors

Why is OAuth not working? Common OAuth authentication errors and solutions:

### Error: invalid_client

**Error message:** "error": "invalid_client", "error_description": "Client 
authentication failed"

**Cause:** The OAuth client ID or client secret is incorrect or doesn't 
match the identity provider's records. This OAuth error occurs when 
credentials are mistyped or the OAuth application was deleted from the IdP.

**Solution:** To fix the invalid_client OAuth error, verify your client ID 
and client secret in the Admin Console OAuth settings. Regenerate OAuth 
credentials from your identity provider if needed. Ensure the OAuth 
application still exists in your IdP dashboard.

### Error: redirect_uri_mismatch

**Error message:** "error": "redirect_uri_mismatch", "error_description": 
"The redirect URI in the request did not match a registered redirect URI"

**Cause:** The redirect URI configured in the Admin Console doesn't match 
what's registered with the OAuth identity provider. OAuth requires exact 
URI matching including protocol (http vs https) and trailing slashes.

**Solution:** To fix redirect_uri_mismatch, compare the redirect URI in 
Admin Console OAuth settings with the registered URI in your identity 
provider. Ensure exact match including https:// and any trailing slash. 
Update either location to match.
```

**Key elements:**
- Question echo opening
- VERBATIM error messages (users copy-paste these)
- Domain terms in every paragraph
- Complete solutions within each subsection

## Phase 4 Checklist

For each domain file:

- [ ] Opening paragraph has question echoes
- [ ] Opening paragraph has primary term + variations
- [ ] Opening paragraph has key terms list
- [ ] Every paragraph opens with context
- [ ] Every paragraph contains domain keywords
- [ ] Keywords distributed every 500-800 characters
- [ ] Inverted pyramid in every paragraph
- [ ] No cross-references ("as mentioned above")
- [ ] Error messages verbatim in troubleshooting
- [ ] Related topics section present

## Common Mistakes

**Context-free paragraphs:** Starting with "To do this..." instead of "To configure OAuth..."

**Front-loaded keywords:** All terms in first paragraph, none in the rest

**Buried leads:** Answer at the end of paragraph instead of first sentence

**Paraphrased errors:** "If you see an auth error" instead of verbatim "invalid_client"

**Cross-references:** "See above" when the chunk won't include "above"

## Next Phase

Proceed to **Phase 5: Hybrid Optimisation** — Final review for balance.

Then **Phase 6: Verification** — Test retrieval accuracy.

## Related References

- `templates/domain-file-template.md` — Full reference file template
- `templates/operational-domain-template.md` — Operational file template
- `mechanics/chunk-resilience.md` — Detailed chunk resilience principles
- `mechanics/semantic-hedging.md` — Semantic optimisation details
- `mechanics/bm25-optimisation.md` — Keyword optimisation details
- `anti-patterns.md` — Reference and operational file anti-patterns to avoid
