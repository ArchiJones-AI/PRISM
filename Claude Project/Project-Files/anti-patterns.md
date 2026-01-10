---
domain: anti-patterns
type: reference
semantic_triggers: anti-pattern, mistake, failure, avoid, wrong,
  common errors, what not to do, problems
related_domains:
  - quality-gates.md
  - phase-6-verification.md
last_updated: 2025-01
---

# Anti-Patterns — Common Failures to Avoid

What mistakes should I avoid? This reference catalogues 16 common anti-patterns that cause retrieval failures in Claude Projects knowledge systems. Use this as a checklist when reviewing your work.

Key terms: anti-patterns, mistakes, failures, context-free, keyword desert, cross-reference, verbatim, vague query, knowledge gap, conductor pattern, operational files

## Instruction Anti-Patterns

### Anti-Pattern 1: Abstract Query Instructions

**Symptom:** Instructions describe patterns abstractly without concrete examples.

**Example:**
```markdown
## How to Search
Search for [component] + [issue] when troubleshooting.
Search for [action] + [feature] for how-to questions.
```

**Why it fails:** Claude interprets abstract patterns variably. Query generation is stochastic—abstract guidance doesn't reliably bias query formulation.

**Fix:** Include a concrete example table with 15-20 specific mappings:

```markdown
| User Question | Good Search Query |
|---------------|-------------------|
| "Why can't I log in?" | "authentication login failure troubleshooting" |
| "Set up OAuth" | "OAuth identity provider configuration setup" |
```

### Anti-Pattern 2: No Vague Query Handling

**Symptom:** Instructions don't address what to do with vague user questions.

**Example:** Instructions only cover how to search, not when NOT to search.

**Why it fails:** When a user asks "it's not working", Claude searches with vague terms like "not working error" and retrieves irrelevant chunks. Then it may synthesise an answer from those irrelevant chunks.

**Fix:** Add explicit vague query handling:

```markdown
## Handling Vague Questions
If the question is too vague (e.g., "it's not working", "help me"):
1. DO NOT search with vague terms
2. ASK the user to specify the component and symptoms
3. THEN search with specific information
```

### Anti-Pattern 3: No Knowledge Gap Handling

**Symptom:** Instructions don't address what to do when retrieval returns irrelevant content.

**Why it fails:** For out-of-scope questions, retrieval returns "least bad" chunks. Without gap handling instructions, Claude may synthesise answers from tangentially related content (hallucination).

**Fix:** Add explicit gap handling:

```markdown
## When Content Doesn't Answer the Question
If retrieved chunks don't directly answer the question:
1. State clearly that this topic isn't covered
2. Offer general knowledge with caveats
3. DO NOT synthesise from tangentially related chunks
```

---

## Conductor Anti-Patterns

These anti-patterns relate to the routing layer (Project Instructions) and the conductor pattern.

### Anti-Pattern 4: Conductor Contains Sheet Music

**Symptom:** Project Instructions exceed 8,000 characters and contain operational workflows, decision frameworks, or detailed methodology.

**Example:**
```markdown
# Project Instructions (12,000 characters)

## Phase 1: Intake Analysis
1. Parse the input for explicit requirements
2. Identify implicit assumptions
3. Classify complexity level using this framework:
   - Low: Single clear task...
   [continues for 3,000 characters]

## Phase 2: Transformation
[another 3,000 characters of workflow]
```

**Why it fails:**
- Instructions become too long, diluting the routing signal
- Operational content in Instructions can't be selectively retrieved
- Competes with query biasing for attention

**Fix:** Instructions are a CONDUCTOR, not a container. Move operational workflows to operational files, keep only routing:

```markdown
## Query Routing — Operational

| Situation | Retrieve | Search Query |
|-----------|----------|--------------|
| Starting new transformation | intake-analysis.md | "intake analysis process" |
| Moving to phase 2 | transformation-process.md | "transformation workflow" |
```

### Anti-Pattern 5: Orphaned Operational File

**Symptom:** An operational file exists but is not referenced in the Instructions routing table.

**Example:** `operational/constraints.md` exists but Instructions have no routing entry for constraints.

**Why it fails:** The file will never be retrieved because Claude doesn't know to look for it. The conductor doesn't know this section of the orchestra exists.

**Fix:** Every operational file MUST have a corresponding routing entry in Instructions:

```markdown
## Content Index — Operational

**constraints.md**
- Contains: Output constraints, forbidden patterns, required elements
- Retrieve when: Checking output requirements, validating deliverables
- Key terms: constraints, requirements, forbidden, required, validation
```

### Anti-Pattern 6: Orphaned Reference File

**Symptom:** A reference file exists but is not referenced in the Instructions routing table.

**Why it fails:** Same as orphaned operational file—the conductor doesn't direct the orchestra to this section.

**Fix:** Every reference file MUST have a corresponding routing entry in Instructions.

---

## Operational File Anti-Patterns

These anti-patterns relate specifically to operational knowledge files (HOW the system works).

### Anti-Pattern 7: Context-Free Operational Steps

**Symptom:** Process steps start without establishing which process they belong to.

**Example:**
```markdown
**Step 3:** Next, apply the framework to the parsed input.

**Step 4:** Then validate the output against constraints.
```

**Why it fails:** If this chunk is retrieved alone, the reader has no idea which phase, which process, or what framework. The step is useless in isolation.

**Fix:** Every step includes context:

```markdown
**Step 3: Apply Complexity Framework (Intake Phase)**

In the Intake Analysis phase, after parsing requirements, apply the
complexity framework to the parsed input. The complexity framework
classifies input as Low, Medium, or High based on...

**Step 4: Validate Output (Generation Phase)**

During Output Generation, validate the generated output against the
output constraints documented in constraints.md. Check for...
```

### Anti-Pattern 8: Broken Cross-Layer Connection

**Symptom:** Operational file references a reference file that doesn't exist.

**Example:**
```markdown
# Transformation Process

Apply the methodology from `reference/xml-blueprint.md` to structure output.
Use examples from `reference/prompt-examples.md` for guidance.
```

But `prompt-examples.md` was never created.

**Why it fails:** When Claude retrieves the operational file and tries to follow the cross-layer connection, retrieval fails. The process can't be completed.

**Fix:** Before delivery, verify every referenced file exists:

- [ ] All reference files mentioned in operational files exist
- [ ] All operational files mentioned in other operational files exist

### Anti-Pattern 9: Descriptive Operational Content

**Symptom:** Operational file uses passive voice or descriptive language instead of imperative instructions.

**Example:**
```markdown
## Analysis Phase

Input is parsed for requirements. Complexity is classified. Gaps are
identified and documented. The framework is applied to determine
settings.
```

**Why it fails:** This describes what happens but doesn't tell Claude what to DO. Operational content should be actionable instructions.

**Fix:** Use imperative voice throughout:

```markdown
## Analysis Phase

Parse the input for explicit and implicit requirements. Classify the
complexity using the complexity framework. Identify gaps and document
them. Apply the framework to determine THINKING_MODE and
INTERACTION_STANCE settings.
```

### Anti-Pattern 10: Vague Decision Criteria

**Symptom:** Decision frameworks lack specific criteria.

**Example:**
```markdown
### Choosing THINKING_MODE

Use Extended thinking when the task is complex.
Use Standard thinking for simpler tasks.
```

**Why it fails:** "Complex" and "simpler" are subjective. Claude may interpret differently each time. Decision criteria must be specific.

**Fix:** Include specific, objective criteria:

```markdown
### Choosing THINKING_MODE

| Indicator | Complexity | THINKING_MODE |
|-----------|------------|---------------|
| Single clear task, no ambiguity | Low | Standard |
| Multiple steps OR some ambiguity | Medium | Extended |
| Vague input, multiple interpretations | High | Extended |
```

---

## Reference File Anti-Patterns

### Anti-Pattern 11: Context-Free Paragraphs

**Symptom:** Paragraphs start without establishing what they're about.

**Example:**
```markdown
To do this, follow these steps:
1. Open the settings panel
2. Navigate to the third option
3. Enable the toggle
```

**Why it fails:** If this paragraph becomes its own chunk, the contextual summary has no idea what "this" refers to. The chunk is useless in isolation.

**Fix:** Every paragraph opens with explicit context:

```markdown
To configure OAuth authentication in the Admin Console, follow these steps:
1. Open the Admin Console settings panel
2. Navigate to Authentication > OAuth
3. Enable the OAuth integration toggle
```

### Anti-Pattern 12: Front-Loaded Keywords

**Symptom:** All domain terms appear in the first paragraph only; rest of content is generic.

**Example:**
```markdown
# OAuth Authentication — SSO, Identity Providers, SAML, OIDC

OAuth (Open Authorization) enables SSO through identity providers...

## Setting Up the Integration

To set up the integration, go to settings. Find the authentication
section. Select your provider. Enter your credentials. Save and test.
```

**Why it fails:** If chunk boundaries fall after the first paragraph, subsequent chunks contain no domain keywords. BM25 queries for "OAuth" won't match those chunks.

**Fix:** Distribute keywords every 500-800 characters:

```markdown
## Setting Up OAuth Integration

To set up OAuth integration, navigate to Admin Console > Authentication >
OAuth. The OAuth setup requires credentials from your identity provider.
Configure your OAuth client ID and secret, then test the OAuth connection.
```

### Anti-Pattern 13: Keyword Deserts

**Symptom:** Sections of flowing prose with no technical terms.

**Example:**
```markdown
When users want to access the system using their existing accounts from
other services, they can connect those external accounts. This eliminates
the need to remember additional passwords and makes getting into the
application much easier for everyone involved.
```

**Why it fails:** BM25 can't match queries like "OAuth" or "SSO" because those terms aren't present. Good embedding similarity but zero keyword matching.

**Fix:** Include specific terms naturally:

```markdown
When users want to access the system using OAuth single sign-on (SSO),
they can connect their Google, Microsoft, or other identity provider
accounts. This OAuth integration eliminates separate passwords and
streamlines SSO authentication.
```

### Anti-Pattern 14: Giant Sections

**Symptom:** Sections exceed 4,000 characters.

**Why it fails:** Large sections span multiple chunks. If chunk boundaries split mid-section, coherence may be lost. Users retrieving one chunk may miss critical context in adjacent chunks.

**Fix:** Break into subsections of 1,500-2,500 characters. More importantly, ensure each paragraph is self-contained (see Anti-Pattern 11).

### Anti-Pattern 15: Cross-Referential Content

**Symptom:** Content references other sections.

**Examples:**
- "As mentioned above..."
- "See the previous section..."
- "Refer to the authentication section for details..."
- "We'll cover this below..."

**Why it fails:** Retrieved chunks don't include "above" or "below". Cross-references become dangling pointers.

**Fix:** Restate context instead of referencing:

```markdown
"As mentioned in the authentication section, tokens expire after 24 hours."
"OAuth tokens expire after 24 hours. When a token expires..."
```

### Anti-Pattern 16: Paraphrased Error Messages

**Symptom:** Troubleshooting describes errors generically instead of verbatim.

**Example:**
```markdown
If you see an authentication error, check your credentials.
If the system says access is denied, verify your permissions.
```

**Why it fails:** Users search exact error text: "401 Unauthorized - Invalid token". Paraphrased descriptions don't match BM25 queries for exact error strings.

**Fix:** Include VERBATIM error messages:

```markdown
### Error: 401 Unauthorized - Invalid token

**Error message:** "401 Unauthorized - Invalid or expired token"

**Cause:** The OAuth token has expired or is invalid...
```

---

## Anti-Pattern Checklist

Before finalising, verify none of these are present:

**Instructions (Conductor):**
- [ ] No abstract query patterns without concrete examples
- [ ] Vague query handling present
- [ ] Knowledge gap handling present
- [ ] Instructions under 8,000 characters (no sheet music in conductor)
- [ ] No orphaned operational files
- [ ] No orphaned reference files

**Operational Files:**
- [ ] No context-free steps ("Next, do..." without phase/process)
- [ ] No broken cross-layer connections
- [ ] Imperative voice throughout (not passive/descriptive)
- [ ] Decision criteria are specific (not "when complex")
- [ ] Process names distributed throughout

**Reference Files:**
- [ ] No context-free paragraph openings
- [ ] Keywords distributed (not front-loaded)
- [ ] No keyword deserts (specific terms throughout)
- [ ] No sections over 4,000 characters
- [ ] No cross-references ("see above")
- [ ] Error messages verbatim
- [ ] Synonyms/variations included
- [ ] Balanced (natural prose + keywords)

## Related References

- `phases/phase-3-instructions.md` — Fixing instruction anti-patterns
- `templates/operational-domain-template.md` — Proper operational file structure
- `phases/phase-4-domain-files.md` — Fixing reference file anti-patterns
- `phases/phase-5-hybrid-optimisation.md` — Achieving balance
- `quality-gates.md` — Complete quality checklist
