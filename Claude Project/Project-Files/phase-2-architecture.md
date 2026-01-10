# Phase 2: Architecture — Designing Domain Structure

How do I design the domain architecture? This reference covers Phase 2 of the decomposition process: structuring extracted knowledge into BOTH operational and reference domains optimised for retrieval, and getting user approval before proceeding.

Key terms: domain architecture, domain design, semantic triggers, query patterns, domain map, user approval, checkpoint, operational domains, reference domains

## Phase 2 Overview

Phase 2 transforms the knowledge inventory from Phase 1 into a concrete domain architecture with two layers:

- **Operational domains:** HOW the system works (workflows, processes, decision frameworks)
- **Reference domains:** WHAT the system knows (methodology, examples, definitions)

This is a critical checkpoint: get user approval before creating files.

**Deliverable:** Domain architecture table (operational + reference) for user approval.

**⚠️ CHECKPOINT:** Pause and wait for user confirmation before proceeding to Phase 3.

## Step 1: Finalise Domain Structure

Refine the domains identified in Phase 1. Target 8-15 domains total across both layers:
- 3-6 operational domains (workflows, processes)
- 5-10 reference domains (knowledge, examples)

### Domain Sizing Guidelines

**Too few domains (< 6):**
- Files become too large
- Multiple unrelated concepts in one chunk
- Retrieval returns irrelevant content alongside relevant content

**Too many domains (> 20):**
- Overhead in Project Instructions domain map
- Fragmented knowledge harder to synthesise
- Related concepts separated unnecessarily

**Right-sized domains:**
- Each domain covers one coherent topic area
- Users would naturally ask questions "about" this domain
- Content fits in 2,000-6,000 characters per domain file

### Domain Naming

Use clear, searchable names:
- ✅ `authentication-basics`
- ✅ `oauth-sso-integration`
- ✅ `session-management`
- ❌ `misc-settings`
- ❌ `other-stuff`
- ❌ `part-2`

## Step 2: Define Domain Specifications

For each domain, specify:

### 2.1 Semantic Triggers

Words and phrases that should trigger retrieval from this domain:

```
Domain: oauth-sso
Triggers: OAuth, OAuth2, SSO, single sign-on, social login, Google login, 
Microsoft login, identity provider, IdP, SAML, OIDC, OpenID Connect, 
third-party authentication, federated identity
```

### 2.2 Concrete Query Examples

CRITICAL: Define specific example mappings, not abstract patterns. Claude learns better from examples.

```
Domain: oauth-sso

| User Question | Good Search Query |
|---------------|-------------------|
| "How do I set up Google login?" | "OAuth Google identity provider setup" |
| "Configure SSO" | "SSO single sign-on configuration" |
| "SAML vs OIDC?" | "SAML OIDC comparison difference" |
| "OAuth error invalid_client" | "OAuth invalid_client error troubleshooting" |
```

### 2.3 Key Terms with Variations

List primary terms AND their variations:

```
Domain: oauth-sso

| Primary Term | Variations |
|--------------|------------|
| OAuth | OAuth2, Open Authorization |
| SSO | single sign-on, single-sign-on |
| identity provider | IdP, auth provider |
| SAML | Security Assertion Markup Language |
```

### 2.4 Question-Form Echoes

Questions to embed in the domain content:

```
Domain: oauth-sso

Questions to echo:
- "How do I set up OAuth?"
- "How do I configure SSO?"
- "Why is OAuth not working?"
- "What's the difference between SAML and OIDC?"
```

### 2.5 Related Domains

Cross-references to related domains:

```
Domain: oauth-sso
Related: authentication-basics, session-management, troubleshooting
```

## Step 2B: Define Operational Domain Specifications

For operational domains (workflows, processes), specify:

### Process Triggers

Words and phrases that trigger retrieval of this workflow:

```
Domain: intake-analysis (operational)
Triggers: intake, analysis, parse, classify, complexity, start, begin,
new task, input processing, requirements extraction
```

### Process Steps Overview

Outline the major steps (details go in the file):

```
Domain: intake-analysis (operational)
Steps:
1. Parse input for explicit requirements
2. Identify implicit assumptions
3. Classify complexity level
4. Document gaps and questions
```

### Decision Frameworks

If the domain contains decision logic:

```
Domain: intake-analysis (operational)
Decision Framework: Complexity Classification
| Indicator | Classification |
|-----------|---------------|
| Single clear task | Low complexity |
| Multiple steps | Medium complexity |
| Vague, multi-interpretation | High complexity |
```

### Cross-Layer Connections

Reference domains this operational domain needs:

```
Domain: intake-analysis (operational)
Needs Reference: methodology.md, constraints.md
```

---

## Step 3: Create Domain Architecture Table

Compile all domain specifications into a table showing BOTH layers:

```markdown
# Domain Architecture: [System Name]

## Operational Domains (HOW)

| ID | Domain Name | Processes | Key Terms |
|----|-------------|-----------|-----------|
| O1 | intake-analysis | Parse, classify, document gaps | intake, analysis, parse, classify |
| O2 | transformation-process | Apply methodology, generate | transform, apply, generate |
| O3 | output-generation | Format, validate, deliver | output, format, validate |
| O4 | constraints | Rules, requirements, forbidden | constraint, rule, requirement |

## Reference Domains (WHAT)

| ID | Domain Name | Primary Topics | Key Terms |
|----|-------------|----------------|-----------|
| R1 | authentication-basics | Login, logout, passwords | auth, login, password |
| R2 | oauth-sso | OAuth, SSO, identity providers | OAuth, SSO, IdP, SAML |
| R3 | session-management | Tokens, expiry, refresh | session, token, expiry |
| R4 | mfa-setup | Multi-factor auth, recovery | MFA, 2FA, TOTP, recovery |
| R5 | troubleshooting | Errors, common issues | error, 401, 403, fix |

## Detailed Domain Specifications

### Domain 01: authentication-basics

**Description:** Basic authentication concepts including login, logout, and password management.

**Semantic Triggers:** authentication, auth, login, log in, sign in, signin, logout, password, credentials

**Query Examples:**
| User Question | Search Query |
|---------------|--------------|
| "How do I log in?" | "login authentication access" |
| "Reset my password" | "password reset change" |

**Terms with Variations:**
| Primary | Variations |
|---------|------------|
| login | log in, sign in, signin |
| password | credentials, passphrase |

**Question Echoes:**
- "How do I log in to the system?"
- "How do I reset my password?"
- "Why can't I log in?"

**Related Domains:** oauth-sso, session-management, troubleshooting

---

### Domain 02: oauth-sso
[Continue for each domain...]
```

## Step 3B: Identify Cross-Cutting Domains (v3.4)

Cross-cutting domains span multiple reference or operational domains. These go in `supporting/` rather than `reference/` or `operational/`.

### When to Create Cross-Cutting Domains

Create a cross-cutting file when content:
- Applies equally to 3+ domains
- Users frequently need to compare across domains
- Doesn't fit cleanly as a "primary home" in any one domain

### Common Cross-Cutting Types

| Type | Purpose | Example |
|------|---------|---------|
| **Decision Matrix** | When to use component A vs B | component-comparison.md |
| **Source Index** | Master catalogue of authoritative sources | source-index.md |
| **Mode/Model Selection** | Intelligence or mode allocation guide | model-selection.md |
| **Quick Reference** | Fast lookups across all domains | quick-reference.md |
| **Terminology Index** | Term-to-domain mapping | terminology-index.md |

### Cross-Cutting Domain Specifications

For cross-cutting domains, specify:

```
Domain: component-comparison (cross-cutting)
Type: supporting
Purpose: Help users choose between extension types
Spans Domains: plugins, skills, hooks, subagents, mcp
Retrieval Triggers: "which should I use", "difference between",
  "compare", "vs", "or"
```

### Sizing Guidelines

- Target 1-3 cross-cutting files per system
- Quick reference and terminology index are REQUIRED (Gate 3)
- Decision matrices only when meaningful distinctions exist

### Routing Cross-Cutting Content

In Instructions, route to cross-cutting files for comparison queries:

```markdown
| User Question | Retrieve | Search Query |
|---------------|----------|--------------|
| "Should I use a skill or hook?" | component-comparison.md | "skill hook comparison" |
| "What does X mean?" | terminology-index.md | "X definition meaning" |
```

---

## Step 4: Validate Architecture

Before presenting to user, verify:

### Coverage Check
- [ ] All source material topics are assigned to a domain
- [ ] No significant gaps in coverage
- [ ] Cross-cutting concepts have clear primary homes

### Retrieval Check
- [ ] Each domain has distinct semantic triggers
- [ ] Query examples are concrete and specific
- [ ] Terms have variations for BM25 matching

### Balance Check
- [ ] No domain is disproportionately large
- [ ] No domain is too small (< 1,000 chars of content)
- [ ] Related concepts are grouped logically

## Step 5: Present for User Approval

Present the domain architecture table to the user with:

1. **Summary table** — Quick overview of all domains
2. **Detailed specifications** — Full spec for each domain
3. **Coverage notes** — What's covered, what's not
4. **Questions for user:**
   - Does this domain structure make sense?
   - Are there domains to merge or split?
   - Any missing topics to add?
   - Any topics to exclude?

### Example Presentation

```markdown
## Domain Architecture for Review

I've analysed your source material and propose the following 
8-domain structure:

| Domain | Topics | Est. Size |
|--------|--------|-----------|
| 01-authentication-basics | Login, passwords | ~2,500 chars |
| 02-oauth-sso | OAuth, SSO, IdP | ~3,500 chars |
| 03-session-management | Tokens, expiry | ~2,000 chars |
[...]

### Coverage Notes
- All authentication topics from source are covered
- Identified gap: API key authentication not in source
- Cross-cutting: Error codes appear in troubleshooting domain

### Questions
1. Does this structure align with how your users think?
2. Should oauth-sso be split into separate OAuth and SAML domains?
3. Any topics I should add or remove?

**Please confirm or suggest changes before I proceed to create files.**
```

## ⚠️ CHECKPOINT: Wait for User Approval

**Do NOT proceed to Phase 3 until user confirms the domain architecture.**

User might:
- Approve as-is → Proceed to Phase 3
- Request changes → Revise architecture and re-present
- Ask questions → Answer and re-confirm

## Phase 2 Checklist

Before presenting to user:

**Operational Domains:**
- [ ] 3-6 operational domains defined
- [ ] Each has process triggers
- [ ] Each has process steps overview
- [ ] Decision frameworks documented where applicable
- [ ] Cross-layer connections noted

**Reference Domains:**
- [ ] 5-10 reference domains defined
- [ ] Each has semantic triggers
- [ ] Each has concrete query examples (not just patterns)
- [ ] Each has terms with variations
- [ ] Each has question echoes
- [ ] Related domains noted

**Cross-Cutting Domains (v3.4):**
- [ ] Cross-cutting needs identified (content spanning 3+ domains)
- [ ] Decision matrices created where comparison queries expected
- [ ] Quick reference planned (required)
- [ ] Terminology index planned (required)
- [ ] 1-3 cross-cutting files total

**General:**
- [ ] Coverage validated (all source material assigned)
- [ ] Architecture table created (both layers + cross-cutting)
- [ ] Total 8-15 domains across operational + reference layers

Before proceeding to Phase 3:
- [ ] User has reviewed architecture
- [ ] User has confirmed or changes incorporated
- [ ] Explicit approval received

## Next Phase

After user approval, proceed to **Phase 3: Create Instructions** — Write Project Instructions with routing for both layers.

## Related References

- `phases/phase-1-ingestion.md` — Source for domain architecture
- `phases/phase-3-instructions.md` — Using domain map in Instructions
- `templates/routing-instruction-template.md` — Instructions template
- `templates/operational-domain-template.md` — Operational file template
- `templates/domain-file-template.md` — Reference file template
