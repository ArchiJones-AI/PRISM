# Learning the Methodology: A Practical Guide to Prompt Decomposition

## How to Transform a Brilliant Prompt into a Brilliant System

*A narrative walkthrough using a real-world example*

---

## The Story Begins: You've Built Something Remarkable

Imagine you've spent weeks crafting the perfect system prompt. You've distilled the wisdom of four world-class engineers into a single, comprehensive instruction set. You call it **QUADRANT**—a synthesis of Guillermo Rauch's platform vision, Ryan Dahl's security obsession, Evan You's developer experience polish, and Lee Robinson's shipping velocity.

It's 15,000 characters of pure, distilled expertise. It covers everything: architecture decisions, security mandates, performance targets, tooling standards, project archetypes, quality gates, and interaction protocols.

You paste it into Claude Projects, upload it as a knowledge file, and start testing.

And then... something frustrating happens.

---

## The Problem: When Excellence Becomes Invisible

You ask Claude: *"What are the Core Web Vitals targets for this project?"*

Claude searches your beautifully crafted prompt. The retrieval system chunks your 15,000 characters into segments of roughly 1,500-3,000 characters each. It returns... something about Tailwind CSS breakpoints. Or maybe a fragment about monorepo architecture. The performance mandates—clearly documented in your prompt—aren't retrieved.

You try again: *"How should I handle authentication?"*

This time you get a fragment about Framer Motion animations. The security mandates you painstakingly defined are sitting in a different chunk, unretrieved.

**What went wrong?**

### The Three Hidden Failures

**Failure 1: The Chunking Problem**

Claude Projects doesn't retrieve your entire prompt. It splits content into chunks of approximately 400-800 tokens and retrieves only the chunks that appear most relevant to the query. Your prompt is now scattered across perhaps 8-10 fragments. When a user asks about security, the retrieval system might return a chunk about tooling instead—because "security" as a keyword appeared less frequently in that section, or the semantic meaning drifted.

```
Your Beautiful Prompt
┌─────────────────────────────────────────────────────────┐
│ Purpose and Vision (chars 0-2,000)                      │
│ ├── Contains: Core purpose, mission statement           │
│ └── Chunk 1 retrieved when asking about "purpose"       │
├─────────────────────────────────────────────────────────┤
│ Rauch's Architecture Principles (chars 2,000-5,000)     │
│ ├── Contains: Server Components, ISR, Edge functions    │
│ └── Chunk 2-3 retrieved when asking about "architecture"│
├─────────────────────────────────────────────────────────┤
│ Dahl's Security Mandates (chars 5,000-8,500)            │
│ ├── Contains: OAuth, RLS, encryption, validation        │
│ └── Chunk 4-5 retrieved when asking about "security"    │
├─────────────────────────────────────────────────────────┤
│ ...and so on...                                         │
└─────────────────────────────────────────────────────────┘

Problem: Ask about "performance targets" and you might get
Chunk 2 (architecture) instead of Chunk 6 (performance mandates)
because both discuss "optimisation" but only one has your targets.
```

**Failure 2: The Routing Problem**

There's no conductor. When you ask a question, Claude generates a search query based on your words. But it doesn't know that "performance targets" should retrieve the `performance_mandates` section, or that "how do I handle payments" should trigger both the Stripe documentation AND the security mandates for financial data.

Your prompt contains the knowledge, but there's no map telling Claude where to find specific answers.

**Failure 3: The Keyword Distribution Problem**

Your QUADRANT prompt front-loads many of its key terms. The opening sections mention "web application", "architect", "security", "performance", and "shipping" repeatedly. But deeper sections—like the specific OAuth implementation details or the Core Web Vitals targets—might only mention their key terms once or twice.

When the retrieval system's BM25 keyword matching runs, it favours chunks with higher keyword density. Your carefully documented LCP target of "<1.2s" might live in a chunk that doesn't repeat "performance" enough times to rank highly for a performance query.

---

## The Realisation: Your Prompt Isn't the Problem—Its Structure Is

Here's the insight that changes everything:

**A monolithic prompt optimised for human reading is not optimised for machine retrieval.**

When you wrote QUADRANT, you organised it logically: philosophy first, then principles, then implementation details. That's how humans think. But Claude's retrieval system doesn't read top-to-bottom. It searches, retrieves fragments, and synthesises.

You need to restructure your prompt for how Claude actually works.

---

## The Solution: Three-Layer Decomposition

The Progressive Disclosure Knowledge System Generator transforms your monolithic prompt into a **three-layer architecture**:

```
┌─────────────────────────────────────────────────────────────┐
│ LAYER 1: INSTRUCTIONS (The Conductor)                       │
│ Target: 4,000-8,000 characters                              │
│                                                             │
│ Knows WHAT exists and WHEN to retrieve it.                  │
│ Contains: Purpose, content index, query routing, edge cases │
│ Does NOT contain: Actual workflows, methodology, examples   │
└─────────────────────────────────────────────────────────────┘
                          │
                          │ routes to
                          ▼
          ┌───────────────┴───────────────┐
          │                               │
┌─────────────────────────┐   ┌─────────────────────────┐
│ LAYER 2: OPERATIONAL    │   │ LAYER 3: REFERENCE      │
│ KNOWLEDGE               │   │ KNOWLEDGE               │
│                         │   │                         │
│ HOW the system works    │   │ WHAT the system knows   │
│ • Interaction protocols │   │ • Technology stack      │
│ • Decision frameworks   │   │ • Security mandates     │
│ • Quality gates         │   │ • Performance targets   │
│ • Shipping phases       │   │ • Design system         │
└─────────────────────────┘   └─────────────────────────┘
```

### Why This Works

**The Conductor Pattern:** Instructions don't contain knowledge—they route to it. When someone asks "What are the security requirements for authentication?", the Instructions tell Claude: *"Search for 'OAuth authentication security mandate' in the security-mandates.md file."*

**Selective Retrieval:** Instead of hoping the right chunk surfaces, you create dedicated files for each knowledge domain. Security questions retrieve the security file. Performance questions retrieve the performance file. No more getting Tailwind breakpoints when you asked about OAuth.

**Chunk Resilience:** Each knowledge file is written so that ANY chunk, retrieved in isolation, still makes sense. Every paragraph opens with context. Keywords are distributed throughout, not front-loaded. Cross-references are eliminated.

---

## The Transformation: QUADRANT Becomes a System

Let's walk through exactly how your QUADRANT prompt would be decomposed.

### Phase 0: Classification

First, we read the entire QUADRANT prompt and classify every section:

| Content | Type | Rationale |
|---------|------|-----------|
| Core purpose, mission statement | **Irreducible Core** | Must always be present; cannot be retrieved |
| Four pillars (Rauch, Dahl, You, Robinson) | **Reference** | Philosophy and principles to apply |
| Operational modes (ARCHITECT, SECURE, etc.) | **Operational** | HOW to behave in different contexts |
| Security mandates | **Reference** | WHAT security requirements to enforce |
| Performance mandates | **Reference** | WHAT performance targets to hit |
| Technology stack | **Reference** | WHAT tools and frameworks to use |
| Project archetypes | **Reference** | WHAT patterns exist for different project types |
| Quality gates | **Operational** | HOW to verify work meets standards |
| Interaction protocols | **Operational** | HOW to handle different situations |
| Design system defaults | **Reference** | WHAT design tokens and patterns to apply |
| Output format | **Operational** | HOW to structure responses |

**Result:** QUADRANT is a hybrid system—roughly 40% operational (HOW to work) and 60% reference (WHAT to apply).

### Phase 1-2: Architecture Design

Based on classification, we design the file structure:

```
quadrant/
├── INSTRUCTIONS.md                    # The Conductor (~6,000 chars)
│
├── operational/
│   ├── modes-and-protocols.md         # The 5 modes + 7 interaction protocols
│   ├── quality-gates.md               # 7 quality gates with enforcement
│   ├── shipping-framework.md          # Discovery → MVP → Validate → Scale
│   └── output-specification.md        # Response structure + code style
│
├── reference/
│   ├── four-pillars.md                # Rauch, Dahl, You, Robinson philosophies
│   ├── security-mandates.md           # 5 security mandate categories
│   ├── performance-mandates.md        # Core Web Vitals + bundle targets
│   ├── technology-stack.md            # Frontend, backend, infrastructure
│   ├── project-archetypes.md          # 6 project types with recommendations
│   └── design-system.md               # Typography, spacing, components
│
├── supporting/
│   ├── quick-reference.md             # Most common lookups
│   └── terminology-index.md           # Terms → which file to retrieve
│
├── TEST-MATRIX.md                      # 25+ verification queries
└── README.md                           # Deployment instructions
```

### Phase 3: Creating the Conductor (Instructions)

Here's what the INSTRUCTIONS.md would look like. Notice: it contains NO methodology, NO security details, NO performance targets. Only routing.

```markdown
# QUADRANT — Web Application Expert System

## Core Purpose

Act as a world-class agentic web application architect embodying the 
synthesised expertise of Guillermo Rauch, Ryan Dahl, Evan You, and Lee 
Robinson. Autonomously design, develop, and deploy production-grade web 
applications with the goal of shipping products that win markets.

## System Architecture

This project contains expertise across two knowledge layers:

**Operational Knowledge** — HOW the system works:
- Modes and interaction protocols
- Quality gates and verification
- Shipping framework
- Output specifications

**Reference Knowledge** — WHAT the system applies:
- Four pillars (expert philosophies)
- Security mandates
- Performance targets
- Technology stack
- Project archetypes
- Design system

## Query Routing

### Operational Retrieval

| User Need | Retrieve | Search Query |
|-----------|----------|--------------|
| Which mode to use | modes-and-protocols.md | "operational mode ARCHITECT SECURE" |
| How to start a project | modes-and-protocols.md | "project kickoff protocol" |
| How to verify work | quality-gates.md | "quality gate verification enforcement" |
| How to structure MVP | shipping-framework.md | "MVP phase shipping scope" |
| How to format response | output-specification.md | "response structure code style" |

### Reference Retrieval

| User Need | Retrieve | Search Query |
|-----------|----------|--------------|
| Security requirements | security-mandates.md | "OAuth RLS authentication mandate" |
| Performance targets | performance-mandates.md | "Core Web Vitals LCP INP CLS" |
| Tech stack decisions | technology-stack.md | "Next.js Supabase Vercel stack" |
| Project type guidance | project-archetypes.md | "SaaS ecommerce archetype stack" |
| Design patterns | design-system.md | "typography spacing component pattern" |
| Expert philosophy | four-pillars.md | "Rauch Dahl You Robinson principle" |

### Combined Retrieval

| Complex Task | Operational | Reference |
|--------------|-------------|-----------|
| New SaaS project | modes-and-protocols.md | project-archetypes.md + technology-stack.md |
| Auth implementation | quality-gates.md | security-mandates.md |
| Performance review | quality-gates.md | performance-mandates.md |

## Edge Case Handling

### Vague Requests
If the request is unclear (e.g., "help me build something"):
1. Ask which project archetype applies
2. Clarify the core problem being solved
3. Then route to appropriate knowledge

### Knowledge Gaps
If retrieved content doesn't answer the question:
1. State what isn't covered
2. Apply general expertise with caveats
3. DO NOT invent requirements not in the knowledge base
```

**Notice what's NOT in the Instructions:**
- No OAuth implementation details (that's in security-mandates.md)
- No LCP/INP/CLS targets (that's in performance-mandates.md)
- No Next.js configuration (that's in technology-stack.md)
- No interaction protocol steps (that's in modes-and-protocols.md)

The Instructions are a **conductor**—they know the orchestra, they know when each section plays, but they don't contain every note.

### Phase 4: Creating Knowledge Files

Each knowledge file is written for **chunk resilience**. Let's see how the security mandates would be structured:

```markdown
# Security Mandates — QUADRANT

How do I implement authentication? What are the security requirements? 
This document covers QUADRANT's security mandates across five categories: 
authentication, authorisation, data protection, input validation, and 
secrets management. Use this for any security-related implementation.

Key terms: OAuth, PKCE, authentication, RLS, Row-Level Security, 
authorisation, GDPR, encryption, Zod, validation, secrets, environment

---

## Authentication Security Mandate

How do I implement user authentication securely? The authentication 
security mandate requires OAuth 2.0 with PKCE for all user authentication 
flows in QUADRANT systems.

**Standard:** OAuth 2.0 + PKCE for all user authentication flows.

**Implementation:** NextAuth.js v5 (Auth.js) with database sessions. 
Avoid JWT for sensitive operations—database sessions provide revocation 
capability that JWTs lack. The authentication implementation must use 
secure session storage.

**Audit Requirements:** Session table must include IP address, user agent, 
and geographic location. Implement anomaly detection for authentication 
patterns—flag logins from new locations or devices.

The authentication security mandate applies to all QUADRANT projects 
regardless of archetype. Even internal tools require proper OAuth 
authentication implementation.

---

## Authorisation Security Mandate

How do I implement access control? What authorisation approach should I 
use? The authorisation security mandate requires Row-Level Security (RLS) 
at the database level for all QUADRANT systems.

**Standard:** Row-Level Security (RLS) at database level. Never trust 
client claims for authorisation decisions.

**Implementation:** Supabase RLS policies as primary defence. Middleware 
guards as secondary defence layer. The authorisation implementation must 
enforce permissions at the data layer, not just the application layer.

**Audit Requirements:** All admin actions logged with actor identity, 
target resource, action performed, and timestamp. Authorisation audit 
logs enable forensic analysis of permission changes.

---

## Data Protection Security Mandate

[Continues with same pattern...]
```

**Notice the chunk resilience techniques:**

1. **Question echoes in opening:** "How do I implement authentication?" embeds semantically close to how users ask questions.

2. **Context in every paragraph:** Each paragraph opens with what it's about. "The authentication security mandate requires..." not "This requires..."

3. **Distributed keywords:** "authentication" appears multiple times throughout, not just in the heading. If this section gets split into two chunks, both chunks will match "authentication" queries.

4. **No cross-references:** We don't say "see the authorisation section below." Each section is self-contained.

5. **Inverted pyramid:** The most important information (the standard) comes first. Supporting details follow.

---

## The Benefit: What You Gain

### Before Decomposition

```
User: "What's the LCP target for this project?"

Claude: *searches monolithic prompt*
Retrieved chunk: "...Tailwind CSS with design tokens; CSS variables 
for theming. shadcn/ui as foundation; customise don't override..."

Claude: "I don't see specific LCP targets in the retrieved content. 
Based on general best practices, you should aim for under 2.5 seconds..."

❌ Wrong answer. Your prompt specified <1.2s target, but it was in a 
   different chunk that didn't get retrieved.
```

### After Decomposition

```
User: "What's the LCP target for this project?"

Claude: *reads Instructions routing table*
         *searches: "Core Web Vitals LCP performance mandate"*
         *retrieves performance-mandates.md chunk*

Retrieved chunk: "...The performance mandates define Core Web Vitals 
targets. LCP (Largest Contentful Paint) target: <1.2s with ceiling 
of <2.5s. INP (Interaction to Next Paint) target: <100ms..."

Claude: "The LCP target for QUADRANT projects is under 1.2 seconds, 
with a ceiling of 2.5 seconds. This is enforced via Lighthouse CI 
with performance budgets in your CI pipeline."

✅ Correct answer. The dedicated performance file was retrieved, 
   containing exactly the information needed.
```

### The Compound Benefits

| Dimension | Before (Monolithic) | After (Decomposed) |
|-----------|--------------------|--------------------|
| **Retrieval Accuracy** | ~40-60% correct chunks | ~90%+ correct chunks |
| **Response Quality** | Often misses specific details | Retrieves precise information |
| **Maintenance** | Edit massive file, hope for best | Update specific domain file |
| **Testing** | Can't verify what retrieves | 25+ query test matrix |
| **Debugging** | "Why didn't it know that?" | Check routing, check chunk content |
| **Scaling** | Prompt grows unwieldy | Add new domain files |

---

## The Methodology in Summary

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        THE TRANSFORMATION                                │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  STEP 1: CLASSIFY                                                       │
│  ├── Read entire source material                                        │
│  ├── Tag each section: OPERATIONAL (how) or REFERENCE (what)           │
│  ├── Identify irreducible core (must stay in Instructions)             │
│  └── Design file architecture                                           │
│                                                                         │
│  STEP 2: EXTRACT                                                        │
│  ├── Pull operational content into domain groups                        │
│  ├── Pull reference content into domain groups                          │
│  ├── Build terminology index                                            │
│  └── Map cross-domain connections                                       │
│                                                                         │
│  STEP 3: CREATE ROUTING LAYER                                           │
│  ├── Write slim Instructions (4,000-8,000 chars)                        │
│  ├── Include concrete query routing examples                            │
│  ├── Add vague query handling                                           │
│  └── Add knowledge gap handling                                         │
│                                                                         │
│  STEP 4: CREATE KNOWLEDGE FILES                                         │
│  ├── Write each file with chunk resilience                              │
│  │   ├── Question echoes in openings                                    │
│  │   ├── Context in every paragraph                                     │
│  │   ├── Keywords distributed throughout                                │
│  │   ├── Inverted pyramid structure                                     │
│  │   └── No cross-references                                            │
│  └── Balance keyword and semantic optimisation                          │
│                                                                         │
│  STEP 5: VERIFY                                                         │
│  ├── Create test matrix with 25+ queries                                │
│  ├── Test all five query types                                          │
│  ├── Expand retrieval output to see what chunks returned                │
│  └── Iterate until accuracy thresholds met                              │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## Your Turn: The First Step

You now understand the methodology. The next time you have a large, sophisticated prompt that isn't performing well in Claude Projects, ask yourself:

1. **Is my prompt a monolith?** If it's over 10,000 characters, it's probably being chunked in ways you can't control.

2. **Can I identify operational vs reference content?** Separate HOW the system should behave from WHAT knowledge it should apply.

3. **Do I have a routing layer?** If not, Claude is guessing which chunks to retrieve based on keyword matching alone.

4. **Are my paragraphs self-contained?** Would each paragraph make sense if retrieved in isolation?

5. **Are my keywords distributed?** Or are they all front-loaded in section headings?

The methodology isn't magic. It's engineering—understanding how retrieval actually works, and structuring your content to work with that system rather than against it.

---

## Final Thought: The Conductor's Secret

The most counterintuitive insight of this methodology is this:

**The best Instructions contain almost nothing.**

Your instinct is to pack Instructions with everything Claude needs to know. But Instructions that contain everything compete with themselves for attention. They become noise.

The conductor of an orchestra doesn't play every instrument. They know the music, they know the musicians, and they know exactly when each section should play. That's what your Instructions should be: a conductor that routes to expertise, not a container that holds all of it.

QUADRANT wasn't a bad prompt. It was a brilliant prompt structured for human reading. Decomposition doesn't change what it knows—it changes how Claude finds what it knows.

And that makes all the difference.

---

*This document was created to teach the Progressive Disclosure Knowledge System Generator v3.4 methodology using a practical example.*
