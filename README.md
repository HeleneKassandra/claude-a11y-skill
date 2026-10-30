# claude-a11y-skill
A Claude skill (`wcag-audit`) for checking and validating accessibility and WCAG guideline compliance. It runs structured WCAG 2.1/2.2 audits targeting **Level AA**, and flags **Level AAA** improvements where applicable.

## What it does
- **Audits code** — HTML, React/JSX, and CSS files or snippets.
- **Answers WCAG questions** — explains success criteria and how to meet them, with links to the official W3C documentation.
- **Scans every WCAG domain** — semantic HTML and native HTML5 elements, ARIA, keyboard navigation, visible focus, color contrast, forms and labels, images and alt text, live regions, reduced motion, skip links and more.
- **Produces a structured report** with issues ranked by severity:
  - 🔴 **Critical** — blocks access entirely
  - 🟠 **High** — significant barrier
  - 🟡 **Medium** — degrades the experience
  - 🔵 **Minor / AAA** — nice-to-have improvements

  Each issue includes the affected WCAG success criterion, who is impacted, and before/after code showing the fix. The report ends with a quick-fix checklist.

## Repository contents
| File | Purpose |
|---|---|
| `SKILL.md` | The skill definition: audit workflow, report format, severity rules and common fix patterns |
| `resources/wcag-criteria-reference.md` | Reference of WCAG success criteria |
| `resources/react-a11y-patterns.md` | Accessibility patterns specific to React/JSX |

## Installation

### Claude Code — personal (all projects)
```bash
git clone git@github.com:HeleneKassandra/claude-a11y-skill.git ~/.claude/skills/wcag-audit
```

### Claude Code — single project
From the root of your project:
```bash
git clone git@github.com:HeleneKassandra/claude-a11y-skill.git .claude/skills/wcag-audit
```

### Claude.ai / Claude Desktop
1. Download this repository as a ZIP (or zip the folder containing `SKILL.md`).
2. Go to **Settings → Capabilities → Skills** and upload the ZIP.

Restart Claude Code (or start a new conversation) after installing so the skill is picked up.

## Usage
The skill triggers automatically when you ask Claude about accessibility. You can also paste UI code and ask for a review. Example prompts:

- "Audit this component for WCAG issues"
- "Is this form accessible?"
- "Check `src/components/Header.tsx` for a11y problems"
- "Does this colour combination pass WCAG AA contrast?"
- "What does WCAG 2.4.7 require?"

In Claude Code you can also invoke it explicitly with `/wcag-audit`.

## Example output
```markdown
## ♿ Accessibility Audit Report

### Summary
- **WCAG Level Targeted:** AA (with AAA upgrade notes)
- **Critical Violations:** 1
- **High Violations:** 2
- **Medium Violations:** 1
- **Minor / AAA Opportunities:** 1
- **Overall Status:** ❌ Fails AA

### 🔴 Critical — Must Fix (Blocks Access)
#### Input missing label · WCAG 4.1.2 (A)
**Issue:** The email input has no accessible name.
**Affects:** Screen reader users, voice control users.
...

### 📋 Quick Fix Checklist
- [ ] Add a `<label>` to the email input (Critical)
...
```
