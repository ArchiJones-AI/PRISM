# Quick Reference Template — Fast Lookup Supporting File

Use this template for the quick-reference.md supporting file that provides fast lookups for common operations.

Key terms: quick reference, cheatsheet, shortcuts, commands, common operations, fast lookup

---

## Template

```markdown
---
domain: quick-reference
type: supporting
semantic_triggers: quick, reference, cheatsheet, shortcut, command,
  how do I, what's the command, remind me
last_updated: YYYY-MM
---

# Quick Reference — [System Name]

Fast lookup for common [System Name] operations. Use this reference for quick answers to "How do I...?" and "What's the command for...?" questions.

---

## Common Commands

| Action | Command/Method | Notes |
|--------|----------------|-------|
| [Action 1] | `[command]` | [Brief note] |
| [Action 2] | `[command]` | [Brief note] |
| [Action 3] | `[command]` | [Brief note] |
| [Action 4] | `[command]` | [Brief note] |

---

## Keyboard Shortcuts

| Shortcut | Action | Context |
|----------|--------|---------|
| `[key combo]` | [Action] | [When available] |
| `[key combo]` | [Action] | [When available] |
| `[key combo]` | [Action] | [When available] |

---

## Quick Answers

### How do I [common task 1]?
[One-line answer with command or method]

### How do I [common task 2]?
[One-line answer with command or method]

### How do I [common task 3]?
[One-line answer with command or method]

### What's the difference between [X] and [Y]?
[Brief comparison: X does A, Y does B]

---

## Common Patterns

### [Pattern Category 1]
```
[Code or configuration pattern]
```

### [Pattern Category 2]
```
[Code or configuration pattern]
```

---

## Emergency Troubleshooting

| Symptom | Quick Fix |
|---------|-----------|
| [Common problem 1] | [Quick solution] |
| [Common problem 2] | [Quick solution] |
| [Common problem 3] | [Quick solution] |

For detailed troubleshooting, see [troubleshooting domain].

---

## Where to Find More

| Topic | Domain File |
|-------|-------------|
| [Topic 1] | [domain-1].md |
| [Topic 2] | [domain-2].md |
| [Topic 3] | [domain-3].md |
```

---

## Customisation Checklist

When creating the quick-reference.md file:

### Structure
- [ ] YAML frontmatter with domain, type, semantic_triggers
- [ ] Title includes system name
- [ ] Opening describes purpose (fast lookup)

### Content Sections
- [ ] Common Commands table (5-15 entries)
- [ ] Keyboard Shortcuts table (if applicable)
- [ ] Quick Answers section (5-10 FAQ-style entries)
- [ ] Common Patterns section (2-4 code blocks)
- [ ] Emergency Troubleshooting table (3-5 quick fixes)
- [ ] "Where to Find More" domain index

### Format
- [ ] Tables for scannable content
- [ ] One-line answers (not paragraphs)
- [ ] Code blocks for commands/patterns
- [ ] Links to detailed domain files

### Size
- [ ] Total file size 1,500-2,500 characters
- [ ] Scannable at a glance
- [ ] No detailed explanations (those go in domain files)

---

## Design Principles

### Optimise for Speed
Quick reference is for FAST lookups. Users scanning for a command or shortcut should find it in seconds.

**Good:** Table with command and one-word description
**Bad:** Paragraph explaining when and why to use the command

### Link, Don't Explain
Quick reference points TO detailed content, doesn't duplicate it.

**Good:** "For OAuth setup, see oauth-sso.md"
**Bad:** "OAuth is configured by first going to settings, then..."

### Most Common First
Order content by frequency of use, not alphabetically or by category importance.

**Good:** Most-used commands at top of table
**Bad:** Commands alphabetised A-Z

### Consistent Format
Every entry follows the same pattern for scannability.

**Good:** All shortcuts in same format: `Cmd+K` | Action | Context
**Bad:** Mixed formats: "Press Command-K", "Ctrl+K", "⌘K"

---

## Example: Developer Tool Quick Reference

```markdown
---
domain: quick-reference
type: supporting
semantic_triggers: quick, reference, cheatsheet, shortcut, command,
  how do I, what's the command, remind me, help
last_updated: 2025-01
---

# Quick Reference — DevTool Pro

Fast lookup for common DevTool Pro operations.

---

## Common Commands

| Action | Command | Notes |
|--------|---------|-------|
| Start server | `devtool serve` | Default port 3000 |
| Run tests | `devtool test` | All tests |
| Run single test | `devtool test [name]` | Partial match |
| Build production | `devtool build --prod` | Outputs to /dist |
| Check config | `devtool config --validate` | Shows errors |

---

## Keyboard Shortcuts

| Shortcut | Action | Context |
|----------|--------|---------|
| `Cmd+K` | Command palette | Any view |
| `Cmd+Shift+P` | Quick open file | Any view |
| `Cmd+B` | Toggle sidebar | Any view |
| `F5` | Start debugging | Editor |

---

## Quick Answers

### How do I reset my config?
Delete `~/.devtool/config.json` and restart.

### How do I update to latest version?
Run `devtool update` or `npm update -g devtool`.

### What's the difference between `serve` and `build`?
`serve` runs development server with hot reload. `build` creates production bundle.

---

## Emergency Troubleshooting

| Symptom | Quick Fix |
|---------|-----------|
| "Port in use" error | `devtool serve --port 3001` |
| Config not loading | `devtool config --reset` |
| Build fails silently | `devtool build --verbose` |

For detailed troubleshooting, see troubleshooting.md.

---

## Where to Find More

| Topic | Domain File |
|-------|-------------|
| Configuration | config-settings.md |
| Plugins | plugins.md |
| Debugging | debugging.md |
| Deployment | deployment.md |
```

---

## Related References

- `domain-file-template.md` — Detailed domain content (quick-reference links to these)
- `quality-gates.md` — Gate 3 requires quick-reference.md
- `terminology-index-template.md` — Companion supporting file
