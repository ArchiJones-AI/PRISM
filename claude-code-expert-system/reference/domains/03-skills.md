---
domain: skills
semantic_triggers: skill, skills, SKILL.md, agent skills, .claude/skills/, skill creation, skill loading, auto-invoke, skill frontmatter, progressive disclosure, skill scripts, skill vs command, skill vs agent, built-in skills, docx skill, pptx skill, xlsx skill, pdf skill
priority_sources:
  - https://code.claude.com/docs/en/skills
  - https://github.com/anthropics/skills
  - https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills
  - https://claude.com/blog/skills
related_domains:
  - 02-subagents.md
  - 01-plugins.md
  - 06-memory.md
---

# Skills Reference

## Quick Answer

Skills are **auto-invoked capabilities** that Claude loads based on task context. Unlike commands (user-triggered) or subagents (separate context), skills extend Claude's knowledge **within the current conversation**. They use progressive disclosure: only name/description loaded at startup, full content loaded when relevant. Define in `.claude/skills/{name}/SKILL.md`.

## Key Concepts

### Skills vs Commands vs Subagents

| Aspect | Skills | Commands | Subagents |
|--------|--------|----------|-----------|
| **Trigger** | Auto (model decides) | Manual (`/command`) | Manual or Task tool |
| **Context** | Same conversation | Same conversation | Isolated context |
| **Purpose** | Add knowledge/capability | Execute workflow | Delegate task |
| **When to use** | Claude should "just know" | User wants control | Need isolation/parallelism |

### Progressive Disclosure Architecture
```
Startup:
  ├─ Load all SKILL.md names + descriptions (lightweight)
  └─ Full content NOT loaded yet

When relevant task detected:
  └─ Full SKILL.md content loaded into context
```

This means 100 skills only add ~100 lines at startup, not 10,000.

### Skill Structure
```
.claude/skills/
└── my-skill/
    ├── SKILL.md              # Main skill definition (required)
    ├── scripts/              # Executable Python/bash scripts
    │   ├── generate.py
    │   └── validate.sh
    ├── references/           # Additional knowledge files
    │   ├── api-docs.md
    │   └── examples.json
    └── assets/               # Templates, images, data files
        └── template.html
```

### SKILL.md Format
```markdown
---
name: api-integration
description: Guides integration with external REST APIs including auth, pagination, and error handling
---

# API Integration Skill

## When to Use
Invoke this skill when the user needs to:
- Connect to external APIs
- Handle authentication (OAuth, API keys)
- Implement pagination
- Handle rate limiting and errors

## Key Patterns

### Authentication
[Detailed auth patterns...]

### Pagination
[Pagination handling...]

## Scripts Available
- `scripts/generate-client.py` — Generate API client boilerplate
- `scripts/test-endpoint.sh` — Test API connectivity
```

### Built-in Skills

| Skill | Purpose | Location |
|-------|---------|----------|
| **docx** | Create/edit Word documents | `/mnt/skills/public/docx/` |
| **pptx** | Create/edit PowerPoint presentations | `/mnt/skills/public/pptx/` |
| **xlsx** | Create/edit Excel spreadsheets | `/mnt/skills/public/xlsx/` |
| **pdf** | Create/manipulate PDF files | `/mnt/skills/public/pdf/` |

## Authoritative Sources

| Source | Type | URL |
|--------|------|-----|
| Skills Documentation | Official Docs | https://code.claude.com/docs/en/skills |
| Skills Repository | GitHub | https://github.com/anthropics/skills |
| Skills Engineering Post | Blog | https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills |
| Skills Announcement | Blog | https://claude.com/blog/skills |
| Skills Deep Dive | Community | https://leehanchung.github.io/blogs/2025/10/26/claude-skills-deep-dive/ |
| Simon Willison Analysis | Community | https://simonwillison.net/2025/Oct/16/claude-skills/ |

## Common Patterns

### Creating a Documentation Skill
```markdown
# Save to: .claude/skills/documentation/SKILL.md
---
name: documentation
description: Writes comprehensive technical documentation following project conventions
---

# Documentation Skill

## When to Use
- User asks to document code, APIs, or features
- README updates needed
- API reference generation required

## Documentation Standards

### File Structure
- README.md — Overview, quickstart, installation
- docs/api.md — API reference
- docs/guides/ — How-to guides
- CHANGELOG.md — Version history

### Writing Style
- Use active voice
- Include code examples for every feature
- Add troubleshooting sections
- Keep paragraphs short (3-4 sentences)

### Templates

#### Function Documentation
```
## function_name(param1, param2)

Brief description of what the function does.

**Parameters:**
- `param1` (type): Description
- `param2` (type): Description

**Returns:** type — Description

**Example:**
\`\`\`python
result = function_name("value", 123)
\`\`\`
```
```

### Creating a Skill with Scripts
```markdown
# Save to: .claude/skills/code-generator/SKILL.md
---
name: code-generator
description: Generates boilerplate code using project templates
---

# Code Generator Skill

## Available Scripts

### Generate Component
Run `scripts/generate-component.py` to create a new React component:
```bash
python scripts/generate-component.py --name MyComponent --type functional
```

### Generate API Endpoint
Run `scripts/generate-endpoint.py` to scaffold a new API route:
```bash
python scripts/generate-endpoint.py --path /users --methods GET,POST
```

## Usage
When the user asks to generate code, use the appropriate script and customise the output based on their requirements.
```

Then create the script:
```python
# Save to: .claude/skills/code-generator/scripts/generate-component.py
#!/usr/bin/env python3
import argparse

def generate_component(name: str, component_type: str):
    if component_type == "functional":
        return f'''
import React from 'react';

interface {name}Props {{
  // Define props here
}}

export const {name}: React.FC<{name}Props> = (props) => {{
  return (
    <div className="{name.lower()}">
      {name} Component
    </div>
  );
}};
'''
    # ... other types

if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument("--name", required=True)
    parser.add_argument("--type", default="functional")
    args = parser.parse_args()
    print(generate_component(args.name, args.type))
```

### Loading Skills into Subagents
```markdown
# In .claude/agents/doc-writer.md
---
name: doc-writer
description: Writes documentation
tools: Read, Write, Glob
skills: documentation, api-integration
---
```

## Prompt Templates

### Create Custom Skill
```
Create a skill called "testing" that helps write comprehensive tests. It should:

1. Know our testing conventions (Jest for unit, Playwright for e2e)
2. Include templates for common test patterns
3. Have a script that generates test boilerplate
4. Cover edge cases and error scenarios

Save to .claude/skills/testing/
```

### Use Built-in Skill
```
Create a PowerPoint presentation about our Q3 results.
Use the pptx skill to generate professional slides with:
- Executive summary
- Key metrics (use charts)
- Challenges and wins
- Q4 roadmap
```

### Skill with References
```
Create a skill for our company's API conventions. Include:

1. SKILL.md with our REST API standards
2. references/openapi-template.yaml as a base schema
3. references/error-codes.md listing our standard errors
4. scripts/validate-api.py to check conformance

The skill should auto-invoke when anyone works on API endpoints.
```

## Troubleshooting

### Skill Not Loading
```bash
# Check file location
ls -la .claude/skills/my-skill/SKILL.md

# Verify frontmatter syntax
head -20 .claude/skills/my-skill/SKILL.md

# Check for YAML errors (must have --- delimiters)
```

### Skill Not Auto-Invoking
- Ensure `description` clearly matches use cases
- Make skill name descriptive
- Check that SKILL.md exists (not skill.md)
- Restart session after creating new skills

### Scripts Not Executing
```bash
# Check permissions
chmod +x .claude/skills/my-skill/scripts/*.py
chmod +x .claude/skills/my-skill/scripts/*.sh

# Verify shebang line
head -1 .claude/skills/my-skill/scripts/generate.py
# Should be: #!/usr/bin/env python3
```

### Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| Skill not found | Wrong directory structure | Must be `.claude/skills/{name}/SKILL.md` |
| Frontmatter invalid | YAML syntax | Check `---` delimiters and indentation |
| Script fails | Missing dependencies | Install required packages |
| Skill ignored | Poor description | Make description match task types |

## Skills in Plugins

Skills can be distributed via plugins:
```
my-plugin/
└── .claude-plugin/
    └── skills/
        └── my-skill/
            └── SKILL.md
```

When plugin is installed, skills become available project-wide.

## Related Domains

- **[Subagents](02-subagents.md)** — Load skills into agents via `skills:` frontmatter
- **[Plugins](01-plugins.md)** — Distribute skills via plugins
- **[Memory](06-memory.md)** — CLAUDE.md vs skills for project knowledge
- **[Core Plugins](11-core-plugins.md)** — Official plugins with skills (frontend-design)
