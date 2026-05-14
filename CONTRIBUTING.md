# Contributing to NEXUS-PRO

Thank you for helping make NEXUS-PRO better. This guide explains how to contribute effectively — from fixing a typo to adding an entire new technical reference.

---

## Table of Contents

- [Types of Contributions](#types-of-contributions)
- [Workflow: Bug Fix or Rule Correction](#workflow-bug-fix-or-rule-correction)
- [Workflow: New Reference Document](#workflow-new-reference-document)
- [Workflow: New Prompt or Template](#workflow-new-prompt-or-template)
- [Workflow: New Stack Support](#workflow-new-stack-support)
- [Reference Document Format](#reference-document-format)
- [Quality Standards](#quality-standards)
- [Labels and Governance](#labels-and-governance)
- [Code of Conduct](#code-of-conduct)

---

## Types of Contributions

| Type | File location | Complexity |
|------|--------------|-----------|
| Fix typo or clarify wording | Any `.md` file | 🟢 Easy |
| Fix incorrect rule | `references/*.md` | 🟡 Medium |
| New prompt | `examples/*.md` | 🟢 Easy |
| New template | `templates/*.md` | 🟡 Medium |
| New reference document | `references/*.md` | 🔴 Hard |
| New stack support | `NEXUS_CONFIG.template.md` + `SKILL.md` | 🔴 Hard |
| New language translation | `README.{lang}.md` + `CONTRIBUTING.{lang}.md` | 🟡 Medium |

---

## Workflow: Bug Fix or Rule Correction

Use this when a rule generates incorrect, insecure, or suboptimal code.

**Step 1 — Open an issue first**

Use the [Bug Report](./.github/ISSUE_TEMPLATE/bug_report.yml) template. Include:
- The file and rule that is wrong
- What code it currently makes Antigravity generate
- What it should generate instead

**Step 2 — Fork and branch**

```bash
git checkout -b fix/backend-ownership-validation
```

**Step 3 — Edit the rule**

Find the problematic rule in the appropriate `references/*.md` file.
Apply the correction following the existing format (see [Reference Document Format](#reference-document-format)).

**Step 4 — Update the MANIFEST.md**

If you changed a file, update its description in `MANIFEST.md` if the description no longer matches.

**Step 5 — Update CHANGELOG.md**

Add an entry under the current development version:
```markdown
### Fixed
- `references/backend.md`: Corrected ownership validation rule to require `company_id` check even on soft-deleted records (#42)
```

**Step 6 — Submit a Pull Request**

Use the PR template. Describe what was wrong and how you fixed it.

---

## Workflow: New Reference Document

Use this when an entire technical topic is missing from the skill.

**Step 1 — Open a Feature Request issue**

Describe the topic and why it matters for enterprise projects on the supported stack.
Wait for acknowledgment before writing the full document — this prevents duplicate work.

**Step 2 — Write the document**

Follow the [Reference Document Format](#reference-document-format) exactly.
Place the file in `references/your-topic.md`.

**Step 3 — Register it in MANIFEST.md**

Add an entry in the appropriate section:
```markdown
| `references/your-topic.md` | Brief description of what it covers |
```

**Step 4 — Add a trigger in SKILL.md (if applicable)**

If the new reference covers a new task type, add a trigger:
```yaml
triggers:
  - "your new trigger phrase"
```

**Step 5 — Add to NEXUS_CONFIG.template.md (if it's feature-gated)**

If the reference only applies when a specific feature is enabled, add the feature flag to the config template.

**Step 6 — Update CHANGELOG.md**

```markdown
### Added
- `references/your-topic.md`: [Topic name] patterns covering [scope] (#55)
```

---

## Workflow: New Prompt or Template

**New prompt (`examples/*.md`)**

Prompts must follow this structure:

```markdown
# [Task Name] Prompt

## When to use
[One sentence describing the exact scenario]

## Prompt

```
[The exact prompt the user should paste into Antigravity,
 referencing the skill by name and describing the task clearly]
```

## Expected output
[Describe what Antigravity should produce when this prompt is used]

## Notes
[Optional: edge cases, prerequisites, or related prompts]
```

**New template (`templates/*.md`)**

Templates must include:
- A comment header explaining the template's purpose
- All placeholder values clearly marked with `[PLACEHOLDER]`
- A "How to use" section at the top

---

## Workflow: New Stack Support

Adding support for a new stack (e.g., Django + Vue 3 + MySQL) requires:

1. **NEXUS_CONFIG.template.md** — ensure the new values are documented
2. **SKILL.md** — add adaptation rules in the "Stack adaptation rules" table
3. **At least one reference document** specific to the new framework
4. **At least one example prompt** using the new stack
5. **NEXUS_CONFIG.example.md variants** — create an example config for the new stack
6. **docs/comparison.md update** — add the new stack to the supported stacks section

New stacks require **maintainer approval** before being merged, as they add significant documentation scope.

---

## Reference Document Format

Every `references/*.md` file must follow this structure:

```markdown
# [Topic Name]

## Purpose

[One paragraph explaining what this document covers and when Antigravity should apply it.]
[Optional: mention the NEXUS_CONFIG flag that activates it.]

---

## Core Rules

### [Rule Group Name]

[Explanation of the rule — why it matters, not just what it is]

\`\`\`[language]
// ❌ FORBIDDEN — [brief reason]
[bad code example]

// ✅ REQUIRED — [brief reason]
[good code example]
\`\`\`

[Repeat for each rule]

---

## Prohibited Patterns

\`\`\`[language]
// ❌ [brief explanation of why this is prohibited]
[code to avoid]
\`\`\`

---

## Checklist

- [ ] [Verifiable condition that ensures the rule is followed]
- [ ] [Another verifiable condition]
```

**Non-negotiable format rules:**
- Every rule has BOTH a `❌ FORBIDDEN` and `✅ REQUIRED` example
- Every example includes a brief comment explaining WHY — not just what
- Every document ends with a Checklist section
- No examples containing real company names, emails, or private project references

---

## Quality Standards

Before submitting any PR, verify:

### Generic (required for all)
- [ ] No references to specific private projects or companies
- [ ] Content applies to any project using the supported stack — not just one project
- [ ] All code examples are self-contained (no undefined variables or missing imports)
- [ ] No `any` types in TypeScript examples without a comment explaining why
- [ ] No direct DB queries in controller examples

### For new reference documents
- [ ] Document has a Purpose section
- [ ] Every rule has both a forbidden and required example
- [ ] Document ends with a Checklist
- [ ] File registered in MANIFEST.md

### For new prompts
- [ ] Prompt clearly names the skill (`nexus-skill-v7-pro-enterprise`)
- [ ] Prompt includes "Expected output" description
- [ ] Prompt tested with Antigravity before submission

### For security-related content
- [ ] Content reviewed against OWASP Top 10
- [ ] No patterns that encourage skipping authorization
- [ ] No examples that log sensitive data

---

## Labels and Governance

| Label | Meaning |
|-------|---------|
| `bug` | Incorrect rule or template |
| `enhancement` | New content or improvement |
| `security` | Security-related rule or fix |
| `question` | Usage question |
| `needs-review` | Awaiting maintainer review |
| `approved` | Ready to merge |
| `duplicate` | Already exists or being worked on |
| `wontfix` | Out of scope for this skill |

**Decision authority:**
- Typos and clarifications → merge without extended review
- Rule changes → require one maintainer approval
- New references → require maintainer approval + testing with Antigravity
- New stack support → require maintainer approval + at least 2 example files

---

## Code of Conduct

- Be specific — vague feedback is not actionable
- Criticize the rule or code, never the person who wrote it
- If you disagree with a decision, explain your reasoning with a concrete example
- Maintainers have final decision authority on scope and direction

---

## Questions?

Open an issue using the [Question template](./.github/ISSUE_TEMPLATE/question.yml).

For commercial licensing inquiries: masterabraham89@gmail.com
