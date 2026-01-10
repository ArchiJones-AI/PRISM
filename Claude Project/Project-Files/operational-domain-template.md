# Operational Domain Template — HOW the System Works

Use this template for operational knowledge files that document HOW the system performs its task: phases, workflows, decision frameworks, principles, constraints, and output specifications.

Key terms: operational knowledge, workflow, phase, decision framework, principles, constraints, process, procedural, execution logic

---

## What Operational Knowledge Contains

Operational knowledge answers: "HOW do I perform this task?"

| Operational Content Type | What It Contains |
|-------------------------|------------------|
| Phases/Workflows | Step-by-step processes for each stage |
| Decision Frameworks | Logic for making choices during execution |
| Principles | Guiding rules that shape behaviour |
| Constraints | Boundaries and requirements that must be followed |
| Output Specifications | Exact format and structure requirements |

**Contrast with reference knowledge:**
- Operational: "Here's how to analyse input" (process)
- Reference: "Here's what good analysis looks like" (examples)
- Operational: "Apply the XML blueprint structure" (directive)
- Reference: "The XML blueprint has three parts..." (explanation)

---

## Template

```markdown
---
domain: [domain-name]
type: operational
semantic_triggers: [action-verb-1], [action-verb-2], [process-name],
  [phase-name], [how-to-keywords], [when-to-keywords]
cross_layer_refs:
  - reference/[related-reference-domain].md
related_operational:
  - [related-operational-domain].md
last_updated: YYYY-MM
---

# [Operational Domain Name] — [System Name]

[Question echo: "How do I [action]?"] [Question echo: "What's the process for [topic]?"] This document covers the [domain description] for [System Name], including [list key processes/frameworks]. Use this when [specific situations that trigger this knowledge].

Key terms: [action verbs], [process names], [framework names], [phase names], [constraint terms]

---

## [Process/Workflow/Framework Name]

### Purpose

[One sentence: What does this process accomplish?]

### When to Use

[Specific triggers: When should this process be retrieved and executed?]

### Process Steps

**Step 1: [Action-Oriented Name]**

[Direct instruction: "Do X." Not "X should be done."]

[Specific detail about HOW to do this step. Include decision points.]

[If conditional]: When [condition], do [action]. Otherwise, do [alternative].

**Step 2: [Action-Oriented Name]**

[Direct instruction]

[Specific detail]

**Step 3: [Action-Oriented Name]**

[Continue pattern...]

### Decision Points

At [specific point in process], decide based on:

| Condition | Action |
|-----------|--------|
| [If this] | [Do this] |
| [If that] | [Do that] |
| [Otherwise] | [Default action] |

### Output of This Process

[What should exist after completing this process? What's the deliverable?]

---

## [Second Process/Framework]

[Continue pattern for each major operational component...]

---

## Principles for [Domain]

[If this domain has guiding principles, list them with explanations]

### Principle 1: [Name]

[Statement of principle]

[How to apply: Specific guidance on applying this principle during execution]

### Principle 2: [Name]

[Continue pattern...]

---

## Constraints and Requirements

[If this domain has constraints, document them explicitly]

### [Constraint Category 1]

**Requirement:** [What must be true]

**Rationale:** [Why this constraint exists]

**Enforcement:** [How to check/ensure compliance]

### [Constraint Category 2]

[Continue pattern...]

---

## Common Situations

[If-then guidance for situations that arise during this operational domain]

### When [Situation 1]

[What to do, specifically]

### When [Situation 2]

[What to do, specifically]

---

## Related Operational Knowledge

- **[Related Domain 1]** — [How it connects: "Use before/after this", "When X, switch to Y"]
- **[Related Domain 2]** — [Connection description]

## Related Reference Knowledge

- **[Reference Domain]** — [What reference knowledge supports this operation]
```

---

## Writing Guidelines for Operational Content

### Use Imperative Voice

Operational knowledge tells Claude what to DO.

**Good (imperative):**
```markdown
Parse the input for explicit requirements. Identify gaps and ambiguities. 
Document each finding with its location in the source.
```

**Bad (descriptive):**
```markdown
The input should be parsed for explicit requirements. Gaps and ambiguities 
are identified. Each finding is documented with its location.
```

### Be Specific About Decisions

Decision points need clear criteria, not vague guidance.

**Good (specific):**
```markdown
### Choosing THINKING_MODE

| Input Complexity | User Expectation | Set THINKING_MODE |
|------------------|------------------|-------------------|
| Single-step task | Quick response | Standard |
| Multi-step reasoning | Thorough analysis | Extended |
| Ambiguous input | Need interpretation | Extended |
```

**Bad (vague):**
```markdown
### Choosing THINKING_MODE

Use Extended when the task is complex. Use Standard for simpler tasks.
```

### Include Sequencing

Operational processes have ORDER. Make sequence explicit.

**Good (sequenced):**
```markdown
**Phase 1: Intake** — Complete before Phase 2
**Phase 2: Analysis** — Requires Phase 1 output
**Phase 3: Generation** — Uses Phase 2 decisions
```

**Bad (unsequenced):**
```markdown
The system includes intake, analysis, and generation phases.
```

### Connect to Other Knowledge

Operational domains often need reference knowledge. Make connections explicit.

**Good (connected):**
```markdown
## Structuring Output

Apply the XML Blueprint structure (see reference/xml-blueprint.md for 
detailed specification). The three required sections are:
1. purpose_and_stance
2. instructions_and_context  
3. output_format
```

---

## Chunk Resilience for Operational Content

Operational files need the same chunk resilience as reference files:

### Paragraph Autonomy

Each paragraph should make sense if retrieved alone. Don't start with "Next, do..."

**Bad:**
```markdown
Next, apply the framework to the parsed input.
```

**Good:**
```markdown
In the Analysis Phase, apply the complexity framework to the parsed input 
from the Intake Phase. This determines THINKING_MODE and INTERACTION_STANCE.
```

### Distributed Keywords

Process names and action terms should appear throughout, not just in headings.

**Target:** Process/phase name every 400-600 characters within its section.

### Self-Contained Steps

Each step should be complete even if retrieved without surrounding steps.

**Bad:**
```markdown
Step 3: Do the same for implicit requirements.
```

**Good:**
```markdown
Step 3: Extract implicit requirements from the input. Look for unstated 
assumptions, implied constraints, and inferred user expectations. Document 
each implicit requirement separately from explicit requirements.
```

---

## Size Guidelines

| Operational Domain Type | Target Size |
|------------------------|-------------|
| Single workflow/process | 2,000-4,000 chars |
| Complex phase with decisions | 3,000-5,000 chars |
| Decision framework | 2,000-3,500 chars |
| Principles collection | 2,000-4,000 chars |
| Constraints/requirements | 1,500-3,000 chars |

If a domain exceeds 5,000 characters, consider splitting into sub-domains.

---

## Customisation Checklist

When creating operational knowledge files:

### YAML Frontmatter (v3.4)
- [ ] `domain:` matches filename (without .md)
- [ ] `type: operational` set correctly
- [ ] `semantic_triggers:` includes action verbs + process names + how-to keywords
- [ ] `cross_layer_refs:` links to reference domains this operation needs
- [ ] `related_operational:` links to related operational domains
- [ ] `last_updated:` set to current YYYY-MM

### Opening Section
- [ ] Opening has question echoes for retrieval
- [ ] Opening describes what operational process this covers
- [ ] Key terms include action verbs and process names

### Process Content
- [ ] Steps are in imperative voice ("Do X")
- [ ] Decision points have specific criteria tables
- [ ] Sequence/ordering is explicit
- [ ] Each paragraph is self-contained
- [ ] Process names distributed throughout (not just headings)
- [ ] Connections to other operational domains noted
- [ ] Connections to reference domains noted
- [ ] Size is 2,000-5,000 characters

---

## Example: Intake Analysis Domain

```markdown
# Intake Analysis — Prompt Architect

How do I analyse user input? What's the process for understanding a prompt 
request? This document covers the intake analysis phase for the Prompt 
Architect, including parsing requirements, classifying complexity, and 
identifying gaps. Use this when receiving new prompt transformation requests.

Key terms: intake, analysis, parse, requirements, complexity, gaps, 
ambiguity, explicit, implicit, classification

---

## Intake Analysis Process

### Purpose

Transform raw user input into structured requirements that inform all 
subsequent transformation phases.

### When to Use

Retrieve and execute intake analysis at the START of every new prompt 
transformation request. This is always Phase 1.

### Process Steps

**Step 1: Parse Explicit Requirements**

Read the user input and extract all explicitly stated requirements. 
Explicit requirements are directly stated: "I want X", "It should do Y", 
"Make it Z". Document each explicit requirement with a quote from the input.

In intake analysis, explicit requirements take priority. If explicit and 
implicit requirements conflict, explicit wins.

**Step 2: Infer Implicit Requirements**

After parsing explicit requirements, identify implicit requirements. 
Implicit requirements are unstated but inferable: assumed context, implied 
constraints, standard expectations for the task type.

The intake analysis should flag each implicit requirement as INFERRED, 
distinguishing from EXPLICIT requirements documented in Step 1.

**Step 3: Classify Complexity**

Using the parsed requirements, classify input complexity:

| Indicator | Complexity Level |
|-----------|------------------|
| Single clear task, no ambiguity | Low |
| Multiple tasks OR some ambiguity | Medium |
| Vague input, multiple interpretations | High |

Intake analysis complexity classification determines THINKING_MODE selection 
in the subsequent Analysis Phase.

**Step 4: Document Gaps**

Identify what's MISSING from the input. Gaps are requirements that should 
exist but weren't stated explicitly or implicitly. Document each gap with 
a recommendation for how to resolve it.

### Output of Intake Analysis

Completed intake analysis produces:
1. List of explicit requirements (with source quotes)
2. List of implicit requirements (flagged as INFERRED)
3. Complexity classification (Low/Medium/High)
4. Gap list with resolution recommendations

---

## Related Operational Knowledge

- **transformation-process.md** — Uses intake analysis output; execute after
- **output-generation.md** — Final phase; uses all prior phase outputs

## Related Reference Knowledge

- **lever-configuration.md** — Complexity classification informs lever selection
- **common-failures.md** — Gap documentation helps avoid "whiffs"
```

---

## Related References

- `phase-0-classification.md` — Identifying operational content
- `phase-1-ingestion.md` — Extracting operational content
- `routing-instruction-template.md` — How Instructions route to operational files
- `reference-domain-template.md` — Contrast with reference knowledge
