# Terminology Index Template — Term-to-Domain Mapping

Use this template for the terminology-index.md supporting file that maps key terms to their primary domains.

Key terms: terminology, index, glossary, term, definition, domain mapping, lookup, vocabulary

---

## Template

```markdown
---
domain: terminology-index
type: supporting
semantic_triggers: terminology, term, definition, what is, what does,
  meaning of, glossary, vocabulary, acronym
last_updated: YYYY-MM
---

# Terminology Index — [System Name]

Term-to-domain mapping for [System Name]. When you need to understand a term or find where a concept is documented, use this index.

---

## A

| Term | Definition | Primary Domain |
|------|------------|----------------|
| [Term] | [Brief definition] | [domain].md |
| [Acronym] | See [expanded form] | — |

## B

| Term | Definition | Primary Domain |
|------|------------|----------------|
| [Term] | [Brief definition] | [domain].md |

## C

[Continue alphabetically...]

---

## Common Acronyms

| Acronym | Expansion | Domain |
|---------|-----------|--------|
| [ABC] | [A Big Concept] | [domain].md |
| [XYZ] | [X Yellow Zebra] | [domain].md |

---

## Term Variations

Some terms have multiple names. This section maps variations to primary terms.

| Variation | Primary Term | Domain |
|-----------|--------------|--------|
| [Alternate name] | [Primary term] | [domain].md |
| [Synonym] | [Primary term] | [domain].md |
| [Common misspelling] | [Correct term] | [domain].md |
```

---

## Customisation Checklist

When creating the terminology-index.md file:

### Structure
- [ ] YAML frontmatter with domain, type, semantic_triggers
- [ ] Title includes system name
- [ ] Alphabetical organisation
- [ ] Separate section for acronyms
- [ ] Separate section for term variations

### Content
- [ ] All key terms from domain files included
- [ ] Definitions are brief (one sentence max)
- [ ] Each term links to primary domain
- [ ] Acronyms mapped to expansions
- [ ] Synonyms/variations mapped to primary terms

### Coverage
- [ ] Terms extracted from all domain file frontmatter
- [ ] All `semantic_triggers` represented
- [ ] Cross-domain terms have single primary home
- [ ] No orphaned terms (every term has a domain)

### Size
- [ ] Total file size 1,000-2,000 characters
- [ ] Entries are scannable (not explanatory)
- [ ] Links to domains for detail (index doesn't explain)

---

## Generation Process

The terminology index can be generated from domain file frontmatter:

### Step 1: Extract Terms
From each domain file, extract:
- `semantic_triggers` from YAML frontmatter
- `Key terms:` line from opening section

### Step 2: Deduplicate
Remove duplicate terms that appear in multiple domains. Assign each term a PRIMARY domain (the most specific/relevant).

### Step 3: Add Definitions
For each unique term, write a one-sentence definition. Don't explain—just identify.

### Step 4: Map Variations
For terms with synonyms, acronyms, or alternate spellings, add entries in the Term Variations section pointing to the primary term.

---

## Design Principles

### Index, Don't Explain
The terminology index MAPS terms to domains. Detailed explanations live in domain files.

**Good:** "OAuth — Open Authorization standard | oauth-sso.md"
**Bad:** "OAuth — OAuth is an open standard for access delegation..."

### One Primary Domain
Each term has ONE primary domain, even if mentioned in multiple files. Choose the most specific home.

**Good:** "OAuth" → oauth-sso.md (specific)
**Bad:** "OAuth" → authentication-basics.md, oauth-sso.md, troubleshooting.md

### Alphabetical, Not Categorical
Organise A-Z for fast lookup. Don't group by category—that's what domain files are for.

**Good:** A section, B section, C section...
**Bad:** Authentication terms, Configuration terms, Error terms...

### Include Variations
Users search with different terms. Map all variations to the primary term.

**Good:**
- "SSO" → See "single sign-on"
- "Single sign-on" → oauth-sso.md

---

## Example: Authentication System Terminology Index

```markdown
---
domain: terminology-index
type: supporting
semantic_triggers: terminology, term, definition, what is, what does,
  meaning of, glossary, vocabulary, acronym
last_updated: 2025-01
---

# Terminology Index — AuthSystem

Term-to-domain mapping for AuthSystem.

---

## A

| Term | Definition | Primary Domain |
|------|------------|----------------|
| Access token | Short-lived credential for API access | session-management.md |
| Authentication | Process of verifying identity | authentication-basics.md |
| Authorization | Process of granting permissions | permissions.md |

## I

| Term | Definition | Primary Domain |
|------|------------|----------------|
| Identity provider | External service that authenticates users | oauth-sso.md |
| IdP | See "Identity provider" | — |

## M

| Term | Definition | Primary Domain |
|------|------------|----------------|
| MFA | See "Multi-factor authentication" | — |
| Multi-factor authentication | Authentication requiring 2+ factors | mfa-setup.md |

## O

| Term | Definition | Primary Domain |
|------|------------|----------------|
| OAuth | Open standard for access delegation | oauth-sso.md |
| OAuth2 | See "OAuth" | — |
| OIDC | See "OpenID Connect" | — |
| OpenID Connect | Identity layer on OAuth 2.0 | oauth-sso.md |

## R

| Term | Definition | Primary Domain |
|------|------------|----------------|
| Refresh token | Long-lived token to obtain new access tokens | session-management.md |

## S

| Term | Definition | Primary Domain |
|------|------------|----------------|
| SAML | XML-based authentication standard | oauth-sso.md |
| Session | Server-side record of authenticated user | session-management.md |
| Single sign-on | One login for multiple services | oauth-sso.md |
| SSO | See "Single sign-on" | — |

## T

| Term | Definition | Primary Domain |
|------|------------|----------------|
| Token | Credential representing authentication state | session-management.md |
| TOTP | Time-based one-time password | mfa-setup.md |
| Two-factor authentication | See "Multi-factor authentication" | — |
| 2FA | See "Multi-factor authentication" | — |

---

## Common Acronyms

| Acronym | Expansion | Domain |
|---------|-----------|--------|
| 2FA | Two-factor authentication | mfa-setup.md |
| IdP | Identity provider | oauth-sso.md |
| MFA | Multi-factor authentication | mfa-setup.md |
| OAuth | Open Authorization | oauth-sso.md |
| OIDC | OpenID Connect | oauth-sso.md |
| SAML | Security Assertion Markup Language | oauth-sso.md |
| SSO | Single sign-on | oauth-sso.md |
| TOTP | Time-based One-Time Password | mfa-setup.md |

---

## Term Variations

| Variation | Primary Term | Domain |
|-----------|--------------|--------|
| Log in | Login | authentication-basics.md |
| Sign in | Login | authentication-basics.md |
| Signin | Login | authentication-basics.md |
| Sign-in | Login | authentication-basics.md |
| Auth | Authentication | authentication-basics.md |
| 2-factor | Multi-factor authentication | mfa-setup.md |
```

---

## Related References

- `domain-file-template.md` — Source of terms (semantic_triggers, key terms)
- `quality-gates.md` — Gate 3 requires terminology-index.md
- `quick-reference-template.md` — Companion supporting file
