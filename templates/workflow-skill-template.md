# Workflow Skill Template

Copy this template when building a skill that orchestrates a multi-step process — rather than wrapping a single API or tool. Workflow skills guide Claude through a defined sequence of steps, often combining file I/O, user confirmations, tool calls, and structured output.

---

## When to Use This Template

Use the workflow template when your skill:
- Has 3+ distinct phases (gather → process → deliver)
- Requires user input at decision points
- Produces a structured artifact (report, file, PR, commit)
- Coordinates multiple tools or services
- Has branching logic based on user choices

Use the [api-skill-template.md](./api-skill-template.md) instead if your skill is primarily wrapping a single external API.

---

## File Structure to Create

```
[workflow-name]/
├── SKILL.md                              # This template → your main skill file
├── [workflow-name]-steps.md              # Detailed step-by-step reference
├── [workflow-name]-output-formats.md     # Templates for generated outputs
└── scripts/
    └── [workflow-step].js                # Node.js helpers for complex steps
```

---

## SKILL.md Content

```yaml
---
name: [gerund-action]-[subject]
description: Use this skill when [primary workflow trigger]. This includes [scenario A], [scenario B], [scenario C], and [scenario D]. Invoke when users want to [common user phrasing 1] or [common user phrasing 2]. Works best when [key precondition, e.g., "a Git repo is present" or "a requirements doc exists"].
---

# [Workflow Name]

[One-sentence description of the workflow's goal and output.]

## What This Skill Does

1. **[Phase 1 Name]** — [What happens in this phase]
2. **[Phase 2 Name]** — [What happens in this phase]
3. **[Phase 3 Name]** — [What happens in this phase]
4. **[Phase 4 Name]** — [What happens in this phase, if needed]

Output: [Describe what the user gets at the end — a file, a PR, a report, etc.]

## Quick Start

Simply ask Claude to [primary trigger phrase], e.g.:
```
[Concrete example of how to invoke this skill]
```

Claude will guide you through each step.

---

## Phase 1: [Phase Name — Gather/Discover/Analyze]

[What Claude does in this phase — reading files, asking questions, scanning code, etc.]

### Inputs Required

Before starting, Claude needs:
- [ ] [Input 1] — [Where to find it or how to provide it]
- [ ] [Input 2] — [Where to find it or how to provide it]
- [ ] [Input 3, optional] — [Default value if not provided]

### Actions

1. [Step 1.1]
2. [Step 1.2]
3. [Step 1.3]

### Output of This Phase

[What Claude produces before moving to phase 2 — e.g., a summary, a list, a plan for review]

**Checkpoint:** Confirm with user before proceeding: "[Confirmation question]"

---

## Phase 2: [Phase Name — Process/Generate/Transform]

[What Claude does in this phase — the main work of the skill.]

### Actions

1. [Step 2.1]
2. [Step 2.2]
3. [Step 2.3]

### Decision Points

Claude will pause and ask the user if:
- [Condition A] — offer [Option X] or [Option Y]
- [Condition B] — warn and ask how to proceed

### Output of This Phase

[What Claude produces — draft content, transformed data, generated code, etc.]

---

## Phase 3: [Phase Name — Deliver/Commit/Export]

[What Claude does to package and deliver the output.]

### Actions

1. [Step 3.1 — write file, create PR, send message, etc.]
2. [Step 3.2]
3. [Step 3.3]

### Output Formats

See `./[workflow-name]-output-formats.md` for templates and examples of the final output.

---

## Full Step Reference

For detailed sub-steps, edge cases, and examples, see `./[workflow-name]-steps.md`.

---

## Examples

### Example 1: [Simple Case]

**Input:**
```
[What the user says/provides]
```

**Output:**
```
[What Claude produces]
```

### Example 2: [Complex Case]

**Input:**
```
[More complex scenario]
```

**Output:**
```
[Richer output]
```

---

## Configuration Options

| Option | Description | Default |
|--------|-------------|---------|
| `--[option1]` | [What it controls] | `[default]` |
| `--[option2]` | [What it controls] | `[default]` |
| `--dry-run` | Preview output without writing files | false |

---

## Error Recovery

| Problem | Cause | Solution |
|---------|-------|---------|
| [Error 1] | [Why it happens] | [How to fix] |
| [Error 2] | [Why it happens] | [How to fix] |
| [Error 3] | [Why it happens] | [How to fix] |

---

## When NOT to Use This Skill

- [Exclusion 1 — e.g., "when you only need a single API call, use X instead"]
- [Exclusion 2 — e.g., "for real-time data, this skill is too slow"]
- [Exclusion 3 — related skill recommendation]
```

---

## [workflow-name]-steps.md Content

```markdown
# [Workflow Name]: Detailed Steps

Expanded reference for each phase. Claude loads this file when working through edge cases or when the user asks for more detail.

## Phase 1 Detail: [Phase Name]

### Sub-step 1.1: [What Claude Does]

[Full explanation with examples, code snippets, or decision trees]

```bash
# Example command for this step
git log --oneline --since="1 week ago"
```

### Sub-step 1.2: [What Claude Does]

[Full explanation]

### Edge Cases for Phase 1

- **If [condition]**: [Do this instead]
- **If [condition]**: [Warn user and ask for clarification]

---

## Phase 2 Detail: [Phase Name]

### Sub-step 2.1: [What Claude Does]

[Full explanation]

### Sub-step 2.2: [What Claude Does]

[Full explanation]

### Edge Cases for Phase 2

- **If [condition]**: [Do this]
- **If [condition]**: [Abort and explain why]

---

## Phase 3 Detail: [Phase Name]

### Sub-step 3.1: [What Claude Does]

[Full explanation]

### Output Validation

Before delivering output, check:
- [ ] [Validation check 1]
- [ ] [Validation check 2]
- [ ] [Validation check 3]
```

---

## [workflow-name]-output-formats.md Content

```markdown
# [Workflow Name]: Output Formats

Templates for the outputs this workflow generates.

## Standard Output: [Output Type]

```[format: markdown/json/yaml/html]
[Template with {{PLACEHOLDERS}} for dynamic values]

# {{TITLE}}

**Date:** {{DATE}}
**Status:** {{STATUS}}

## Summary

{{SUMMARY}}

## Details

{{DETAILS}}
```

## Minimal Output: [Shorter Version]

```
{{KEY_METRIC}}: {{VALUE}}
```

## Full Report: [Extended Version]

```markdown
[Extended template with more sections]
```

## Export Options

The output can be saved as:
- `output.md` — Markdown (default)
- `output.json` — Machine-readable
- `output.pdf` — Via PDF skill (requires document-skills/pdf)
- Directly to Notion/GitHub/Slack — Via connect-apps skill
```

---

## scripts/[workflow-step].js Content

```javascript
#!/usr/bin/env node
/**
 * [workflow-step].js
 * [Description of what this script does in the workflow]
 * Usage: node scripts/[workflow-step].js [args]
 */
import { readFile, writeFile } from 'fs/promises';
import { execSync } from 'child_process';

// Parse arguments
const [,, ...args] = process.argv;
const inputPath = args[0] || './input.json';

// Load input data
const raw = await readFile(inputPath, 'utf8').catch(() => {
  console.error(`❌ Could not read ${inputPath}`);
  process.exit(1);
});
const input = JSON.parse(raw);

// [Core processing logic]
const result = processData(input);

// Output result
console.log(JSON.stringify(result, null, 2));

// Helper functions
function processData(data) {
  // TODO: implement
  return data;
}
```

---

## Checklist Before Publishing

- [ ] Skill name is in gerund form: `[gerund]-[subject]`
- [ ] Description starts with "Use this skill when..."
- [ ] All 3+ phases clearly defined in SKILL.md
- [ ] User confirmation required before destructive steps
- [ ] Edge cases documented in `[workflow-name]-steps.md`
- [ ] Output templates in `[workflow-name]-output-formats.md`
- [ ] Supporting scripts use Node.js ESM (not Python)
- [ ] `--dry-run` option documented (if applicable)
- [ ] No `allowed-tools`, `model`, or `tools` in YAML frontmatter
- [ ] SKILL.md under 500 lines (offload details to steps.md)
- [ ] All file names are intention-revealing (no `helpers.md`, `utils.md`)
