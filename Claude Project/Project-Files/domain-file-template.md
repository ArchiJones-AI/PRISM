# Domain File Template — Chunk-Resilient Knowledge Files

Copy and customise this template for each domain file in /reference/domains/.

---

## Template

```markdown
---
domain: [domain-name]
type: reference
semantic_triggers: [primary-term], [synonym-1], [synonym-2], [acronym],
  [expanded-form], [common-question-keywords], [error-keywords]
priority_sources:
  - "[Tier 1] [URL] — Source of truth"
  - "[Tier 2] [URL] — Authoritative docs"
  - "[Tier 3] [URL] — Context/rationale"
related_domains:
  - [related-domain-1].md
  - [related-domain-2].md
last_updated: YYYY-MM
---

# [Domain Name] — [System Name]

[Question echo 1]? [Question echo 2]? This document covers [semantic
description of what this domain includes]. Use this reference for
questions about [list key topics in natural language].

Key terms: [primary term], [synonym 1], [synonym 2], [acronym],
[expanded form], [variation 1], [variation 2]

---

## [Topic 1] — [Include Primary Keywords in Heading]

[Opening sentence with context + answer: "To [action] in [System Name], 
[direct answer or key point]."] [Second sentence adds essential context, 
mentions domain term again.] [Third sentence provides additional detail 
with another natural mention of the term.]

[Second paragraph of this section. Opens with context. Contains domain 
keywords distributed naturally. Explains the next aspect of this topic. 
Maintains inverted pyramid — most important point first in this paragraph.]

[Optional third paragraph if topic requires more depth. Continues pattern 
of context opening, distributed keywords, self-contained content.]

---

## [Topic 2] — [Keywords in Heading]

[Opening sentence restates context: "In [System Name], [topic] allows 
you to [purpose]."] [This section covers [subtopic 1], [subtopic 2], and 
[subtopic 3] for [domain term] configuration.]

[Detailed content paragraph. Context established. Keywords present. 
Natural, readable prose. Self-contained — would make sense if retrieved 
alone. Domain term appears naturally 1-2 times.]

[If procedural, include steps:]

1. [Step 1 with context — mentions component/feature being configured]
2. [Step 2 with specific terminology]
3. [Step 3 with domain terms naturally included]
4. [Step 4 completing the procedure]

[Closing context if needed. Mentions domain term. Points to related 
considerations.]

---

## [Topic 3] — [Keywords in Heading]

[Continue the pattern for each major topic in this domain...]

---

## Troubleshooting [Domain Name] Issues

[Question echo: "Why is [domain] not working?"] Common [domain term] 
problems and their solutions. If you're experiencing [domain] errors, 
find resolutions below.

### [Error/Symptom 1] — [Include exact error text if applicable]

**Error message:** "[VERBATIM error text exactly as users see it]"

**Cause:** [Explanation of why this [domain] error occurs. Mentions 
domain term in context. Clear, specific cause.]

**Solution:** To fix this [domain] error in [System Name], [specific 
steps]. [Include domain terminology in solution.] [Verify by checking 
specific thing.]

---

### [Error/Symptom 2] — [Exact error text]

**Error message:** "[VERBATIM error message]"

**Cause:** [Cause with domain terms]

**Solution:** [Solution with domain terms distributed naturally]

---

### [Error/Symptom 3]

[Continue pattern for common errors in this domain...]

---

## Authoritative Sources

[Link to official documentation and resources for this domain. Tier 1
sources take precedence over Tier 2-4 in case of conflicts.]

| Tier | Source | Description |
|------|--------|-------------|
| 1 (Official) | [Primary source URL] | Source of truth for [domain] |
| 2 (Docs) | [Documentation URL] | Authoritative documentation |
| 3 (Context) | [Blog/article URL] | Background and rationale |
| 4 (Community) | [Community URL] | Practical examples and discussions |

**Source precedence:** When sources conflict, Tier 1 overrides all others.

---

## Prompt Templates

[Ready-to-use prompts for common tasks in this domain. These help users
get started and improve retrieval matching by echoing typical queries.]

### [Task Category 1]
```
[Example prompt that users might use for this task]
```

### [Task Category 2]
```
[Example prompt for another common task]
```

### [Task Category 3]
```
[Example prompt for troubleshooting or exploration]
```

---

## Related Topics

- **[Related Domain 1]** — For questions about [topics in that domain]
- **[Related Domain 2]** — For questions about [topics in that domain]
- **[Related Domain 3]** — For questions about [topics in that domain]
```

---

## Customisation Checklist

When creating each domain file:

### YAML Frontmatter (v3.4)
- [ ] `domain:` matches filename (without .md)
- [ ] `type: reference` set correctly
- [ ] `semantic_triggers:` includes primary term + synonyms + question keywords
- [ ] `priority_sources:` lists URLs with tier badges [Tier 1-4]
- [ ] `related_domains:` links to related domain files
- [ ] `last_updated:` set to current YYYY-MM

### Opening Section
- [ ] 2+ question echoes in opening
- [ ] Semantic description of domain coverage
- [ ] Key topics listed naturally
- [ ] Key terms list with primary + variations + acronyms

### Each Topic Section
- [ ] Keywords in section heading
- [ ] Opening sentence has context (not "To do this...")
- [ ] Opening sentence contains answer/key point (inverted pyramid)
- [ ] Domain term appears 2-3 times in section
- [ ] Keywords distributed (not just in first sentence)
- [ ] Each paragraph self-contained
- [ ] No cross-references to other sections

### Troubleshooting Section
- [ ] Question echo in opening
- [ ] Error messages VERBATIM
- [ ] Each error has cause AND solution
- [ ] Domain terms in solutions (not just error descriptions)

### Authoritative Sources Section (v3.4)
- [ ] Sources ranked by tier (1-4)
- [ ] Tier 1 is official/source of truth
- [ ] Tier 2 is authoritative documentation
- [ ] Tier 3 is context/rationale (blogs, articles)
- [ ] Tier 4 is community (forums, discussions)
- [ ] Precedence rule stated

### Prompt Templates Section (v3.4)
- [ ] 2-4 ready-to-use prompts included
- [ ] Prompts cover common tasks in this domain
- [ ] Prompts echo typical user queries
- [ ] Each prompt is in code block format

### Related Topics
- [ ] Links to related domains
- [ ] Brief description of what each covers

### Overall
- [ ] Total file size 2,000-6,000 characters
- [ ] No section exceeds 2,500 characters
- [ ] Content reads naturally (not keyword-stuffed)
- [ ] Keywords distributed every 500-800 characters

---

## Writing Examples

### Bad Opening (Context-Free)

```markdown
## Configuration

To do this, follow these steps. First, go to the settings. Then find 
the right option and enable it.
```

**Problems:** No context, no domain terms, generic language.

### Good Opening (Context-Rich)

```markdown
## Configuring OAuth in the Admin Console

To configure OAuth authentication in [System Name], navigate to Admin 
Console > Authentication > OAuth. OAuth configuration requires client 
credentials from your identity provider. The OAuth setup enables 
single sign-on for your users.
```

**Achieves:** Context in opening, domain terms distributed, specific guidance.

---

### Bad Troubleshooting (Paraphrased)

```markdown
### Authentication Error

If you see an error about authentication, check your credentials.
```

**Problems:** No verbatim error, generic advice, no domain terms.

### Good Troubleshooting (Verbatim)

```markdown
### Error: 401 Unauthorized — Invalid or Expired Token

**Error message:** "401 Unauthorized - Invalid or expired token"

**Cause:** The OAuth authentication token has expired or was revoked. 
OAuth tokens in [System Name] expire after 24 hours of inactivity.

**Solution:** To fix this OAuth 401 error, log out and log back in to 
obtain a fresh authentication token. If the OAuth error persists, 
verify your identity provider connection in Admin Console > OAuth.
```

**Achieves:** Verbatim error, domain terms throughout, specific solution.

---

## Keyword Distribution Guide

For a ~2,000 character section:

| Position | What to Include |
|----------|-----------------|
| Heading | Primary keyword |
| Chars 0-300 | Primary term + 1-2 variations |
| Chars 300-800 | Primary term mention + related terms |
| Chars 800-1400 | Primary term mention + specific details |
| Chars 1400-2000 | Primary term mention + closing context |

**Target:** Primary term 3-4 times, variations 2-3 times, naturally distributed.
