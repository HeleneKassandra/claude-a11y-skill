---
name: wcag-audit
description: >
  Use this skill whenever the user wants to audit, check, review, or scan any HTML, React/JSX, CSS, or code snippet for accessibility issues, WCAG violations, or improvements. Trigger this skill for any of the following: check my code for accessibility, WCAG audit, a11y review, is this accessible, screen reader issues, keyboard navigation problems, color contrast check, ARIA errors, accessibility score, fix my accessibility issues, does this pass WCAG, accessibility compliance, or any question about WCAG guidelines and how to meet them. Also trigger when the user pastes code and asks for a review even if they do not mention accessibility if the code is clearly UI/frontend and accessibility is an obvious concern. Always use this skill for accessibility-related tasks.
---

# WCAG Accessibility Audit Skill
Conduct structured WCAG 2.1/2.2 accessibility audits on HTML, React/JSX, CSS, and code snippets. Target **Level AA compliance**, and flag **Level AAA improvements** where applicable.

> **Primary guidance source:** [WCAG 2.1/2.2 documentation](https://www.w3.org/WAI/WCAG22/quickref/) for official definitions and examples.

---

## Audit Workflow

### Step 1 — Identify Input Type
Determine what you're auditing:
- **HTML file or snippet** → audit structure, semantics, ARIA, contrast cues
- **React/JSX component** → audit same as HTML, plus prop and state accessibility
- **CSS file or snippet** → audit focus styles, contrast values, motion, responsive targets
- **WCAG question** → answer directly, citing the relevant success criterion

### Step 2 — Scan All WCAG Domains
Check every domain below. Do **not** skip a domain because it seems irrelevant — scan and confirm no issues before moving on.

| Domain | What to Check |
|---|---|
| Semantic HTML | Correct element usage, heading hierarchy, landmark regions; prefer native HTML5 elements over ARIA-enhanced divs/spans |
| ARIA | Valid roles, required properties, no redundant or conflicting ARIA |
| Keyboard Navigation | Focusable elements, logical tab order, no keyboard traps |
| Focus Visibility | Visible focus indicators on all interactive elements |
| Color & Contrast | Text contrast ratios, non-text contrast, color-not-alone rule |
| Images & Media | Alt text, SVG accessibility, video/audio alternatives |
| Forms | Labels, fieldsets, error messages, autocomplete |
| Screen Reader | Live regions, announcements, hidden content correctness |
| Motion & Animation | `prefers-reduced-motion` respect |
| Touch & Responsive | 44×44px minimum touch targets |
| Language & Page Structure | `lang` attribute, page `<title>`, skip links |
| Dynamic Content | Route change announcements, loading states, modals/dialogs |

### Step 3 — Format the Report
Structure all output using the report format below. Always group by severity (Critical → Minor). Always include the fixed code snippet for every violation found.

---

## Report Format
Reply by using this following format:
## ♿ Accessibility Audit Report

### Summary
- **WCAG Level Targeted:** AA (with AAA upgrade notes)
- **Critical Violations:** N
- **High Violations:** N
- **Medium Violations:** N
- **Minor / AAA Opportunities:** N
- **Overall Status:** ❌ Fails AA / ⚠️ Needs Work / ✅ Passes AA

---

### 🔴 Critical — Must Fix (Blocks Access)
Issues that make content completely inaccessible to one or more user groups.

#### [SHORT TITLE] · WCAG [SC number] ([Level])
**Issue:** What is wrong and why it's a problem.
**Affects:** Who is impacted (e.g., screen reader users, keyboard-only users).
**WCAG Success Criterion:** [1.1.1 Non-text Content](https://www.w3.org/WAI/WCAG22/quickref/#non-text-content)

**Before (Problematic Code):**
```html
<!-- original code here -->
```

**After (Fixed Code):**
```html
<!-- corrected code here -->
```

---

### 🟠 High — Should Fix (Significant Barrier)
Issues that create significant difficulty but don't fully block access.

[Same structure as Critical]

---

### 🟡 Medium — Recommended Fix (Degrades Experience)
Issues that degrade the experience for users with disabilities.

[Same structure as Critical]

---

### 🔵 Minor / AAA Opportunities — Nice to Have
Small effort improvements that would reach Level AAA or polish the experience.

#### [SHORT TITLE] · WCAG [SC number] (AAA)
**Improvement:** What can be improved and why.
**Effort:** Low / Medium
**Before:** `inline code snippet`
**After:** `inline code snippet`

---

### 📋 Quick Fix Checklist
- [ ] Fix 1 (Critical)
- [ ] Fix 2 (High)
- [ ] Fix 3 (Medium)

---

## Severity Classification

| Severity | Criteria | Examples |
|---|---|---|
| 🔴 **Critical** | Completely blocks a user group | Missing alt on informative image, no keyboard access to interactive element, keyboard trap, missing form label, empty page title |
| 🟠 **High** | Significant barrier, partial workaround exists | Contrast ratio below 3:1, focus indicator completely removed, missing error identification, no skip link on page with many nav items |
| 🟡 **Medium** | Degrades experience, workaround exists | Contrast between 3:1–4.5:1, heading hierarchy skipped, ARIA attribute missing but role present, touch target slightly under 44px |
| 🔵 **Minor/AAA** | Polish or Level AAA improvement | Enhance contrast to 7:1, add `autocomplete` hints, provide extended audio description, increase touch target to 48px |

---

## Common Violation Patterns & Fixes

*Code examples based on [WCAG 2.2 Quick Reference](https://www.w3.org/WAI/WCAG22/quickref/)*

### 4.1.2 — Forms: Name, Role, Value

```html
<!-- ❌ No label -->
<input type="text" name="first-name">

<!-- ✅ Explicit for/id pairing -->
<label for="first-name">First name:</label>
<input type="text" name="first-name" id="first-name">

<!-- ✅ Checkbox with label after -->
<input type="checkbox" name="subscribe" id="subscribe">
<label for="subscribe">Subscribe to newsletter</label>

<!-- ✅ Search field with aria-label -->
<input type="text" name="search" aria-label="Search">
<button type="submit">Search</button>

<!-- ✅ Search field with aria-labelledby -->
<input type="text" name="search" aria-labelledby="search-btn">
<button id="search-btn" type="submit">Search</button>
```

### 4.1.2 — Grouped Form Fields (fieldset/legend)

```html
<!-- ✅ Radio group with fieldset + legend -->
<fieldset>
  <legend>File format</legend>
  <div>
    <input type="radio" name="format" id="text" value="txt" checked>
    <label for="text">Text file</label>
  </div>
  <div>
    <input type="radio" name="format" id="csv" value="csv">
    <label for="csv">CSV file</label>
  </div>
</fieldset>
```

### 4.1.2 — Buttons
Always prefer native elements.

```html
<!-- ✅ Native button elements -->
<button>Submit</button>
<button>Cancel</button>
<input type="submit" value="Submit">
<input type="button" value="Cancel">

<!-- ✅ Icon button with aria-label -->
<button aria-label="Submit"><img src="img.png" alt=""></button>
```

### Native HTML5 Elements
Prefer native HTML5 elements over ARIA-enhanced divs/spans. Native elements provide built-in semantics, keyboard behavior, and focus management for free — ARIA roles only add semantics without the behavior.

```html
<!-- ❌ ARIA-enhanced divs for landmark regions -->
<div role="navigation">...</div>
<div role="main">...</div>
<div role="banner">...</div>
<div role="contentinfo">...</div>
<div role="complementary">...</div>

<!-- ✅ Native landmark elements -->
<nav>...</nav>
<main>...</main>
<header>...</header>
<footer>...</footer>
<aside>...</aside>

<!-- ❌ Custom accordion with ARIA -->
<div role="button" aria-expanded="false" tabindex="0">Section title</div>
<div role="region" hidden>Section content</div>

<!-- ✅ Native details/summary accordion -->
<details>
  <summary>Section title</summary>
  <p>Section content</p>
</details>

<!-- ❌ Custom modal with ARIA role -->
<div role="dialog" aria-modal="true" aria-labelledby="modal-title" tabindex="-1">
  <h2 id="modal-title">Confirm action</h2>
  ...
</div>

<!-- ✅ Native dialog element (built-in focus trap and accessibility) -->
<dialog aria-labelledby="modal-title">
  <h2 id="modal-title">Confirm action</h2>
  ...
</dialog>

<!-- ❌ Button used for navigation -->
<button onclick="window.location='/about'">About us</button>

<!-- ✅ Link for navigation, button for actions -->
<a href="/about">About us</a>
<button type="button" onclick="submitForm()">Submit</button>

<!-- ❌ Div used for role="status" aria-live="polite" -->
<div role="status" aria-live="polite">Loading...</div>

<!-- ✅ Output element sets aria-live="polite" and aria-atomic="true" automatically. It's supposed to also set role="status" but some screen readers have been found not to announce it without it being set explicitly-->
<output role="status">Loading..</output>
```

### 4.1.2 — iframes
```html
<!-- ✅ iframe with title -->
<iframe src="https://www.w3schools.com" title="W3Schools Free Web Tutorials"></iframe>
```

### 2.4.7 — Visible Focus
Acceptable forms include: border, underline, colour change, shadow, or text cursor. The browser default is often insufficient — supplement it with CSS.

```css
/* ❌ Removes browser default without replacement */
*:focus { outline: none; }

/* ✅ Change colour on focus */
.clickable {
  color: black;
  background-color: #CCCCCC;
}
.clickable:focus {
  color: white;
  background-color: #333333;
}

/* ✅ Strong focus ring for all interactive elements */
:focus-visible {
  outline: 3px solid #005FCC;
  outline-offset: 2px;
}
```

### 1.4.3 — Contrast (Minimum)
Compliance paths:
1. Text contrast ≥ 4.5:1 (normal text) or ≥ 3:1 (large/bold text ≥ 24px or ≥ 19px bold)
2. Provide a high-contrast version with a clearly marked toggle near the top of the page

```css
/* ❌ Contrast ~2.3:1 — fails AA */
color: #999999;
background: #ffffff;

/* ✅ Contrast 4.6:1 — passes AA for normal text */
color: #767676;
background: #ffffff;

/* ✅ Contrast 7:1 — passes AAA */
color: #595959;
background: #ffffff;
```

### 1.3.1 — Information and Relationships

```html
<!-- ❌ Visual heading not coded as heading -->
<div class="heading-style">About us</div>

<!-- ✅ Heading coded correctly with right level -->
<h2>About us</h2>

<!-- ❌ Visual list without list semantics -->
<div>• Item one</div>
<div>• Item two</div>

<!-- ✅ Unordered list -->
<ul>
  <li>Item one</li>
  <li>Item two</li>
</ul>

<!-- ✅ Data table with caption and th scope -->
<table>
  <caption>Monthly sales</caption>
  <thead>
    <tr>
      <th scope="col">Month</th>
      <th scope="col">Sales</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>January</td>
      <td>$12,000</td>
    </tr>
  </tbody>
</table>
```

### Skip Navigation (2.4.1)
```html
<!-- ✅ Skip link — must be at top of body -->
<a href="#main-content" class="skip-link">Jump to main content</a>

<style>
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: #000;
  color: #fff;
  padding: 8px 16px;
  z-index: 100;
  transition: top 0.2s;
}
.skip-link:focus {
  top: 0;
}
</style>
```

### Live Regions (4.1.3)
```html
<!-- Polite: status messages, search results count -->
<div aria-live="polite" id="status" class="sr-only"></div>

<!-- Assertive: error alerts that need immediate attention -->
<div role="alert" id="error-msg"></div>
```

### Reduced Motion (2.2.2)
```css
/* ❌ Animation that ignores user preference */
.spinner { animation: spin 1s linear infinite; }

/* ✅ Respects prefers-reduced-motion */
@media (prefers-reduced-motion: no-preference) {
  .spinner { animation: spin 1s linear infinite; }
}
```

---

## React/JSX Specific Checks
Read the file `resources/react-a11y-patterns.md` for React-specific pattern examples, but also apply all the general WCAG checks to the rendered output of React components.

---

## Contrast Ratio Reference

Use these thresholds when evaluating color combinations:

| Text Type | AA Minimum | AAA Enhanced |
|---|---|---|
| Normal text (< 18pt or 14pt bold) | 4.5:1 | 7:1 |
| Large text (≥ 18pt or 14pt bold) | 3:1 | 4.5:1 |
| UI components & graphics | 3:1 | — |
| Disabled / decorative | No requirement | — |

**How to calculate:** If CSS color values are available, compute the relative luminance of foreground and background using the WCAG formula. If values are ambiguous (e.g., variables or Tailwind classes), note the uncertainty and suggest using a tool like the [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/).

---

## WCAG Answering Mode

When the user asks a question about WCAG guidelines (not auditing code), answer directly:

1. State the **Success Criterion number and name**
2. State the **Level (A / AA / AAA)**
3. Explain **what it requires** in plain language
4. Provide a **code example** showing compliant implementation
5. Note any **common mistakes** to avoid
6. If applicable, mention the **AAA enhancement** version

---

## Additional Resources in This Skill

For detailed code examples beyond what's in this file, read:
- `resources/react-a11y-patterns.md` — React-specific patterns: modals, dynamic lists, focus management
- `resources/wcag-criteria-reference.md` — Full list of all WCAG 2.1/2.2 AA criteria with plain-language summaries

Load these only when the topic directly requires the extra detail.

---

## WCAG 2.2 Quick Reference — Criterion Links
See  `resources/wcag-criteria-reference.md` for a comprehensive list of all criteria, and use the following direct links to the WCAG 2.2 Quick Reference for each criterion if additional information is necessary.


| Criterion | URL |
|---|---|
| 1.1.1 Non-text Content | https://www.w3.org/WAI/WCAG22/quickref/#non-text-content |
| 1.3.1 Info and Relationships | https://www.w3.org/WAI/WCAG22/quickref/#info-and-relationships |
| 1.3.5 Input Purpose | https://www.w3.org/WAI/WCAG22/quickref/#identify-input-purpose |
| 1.4.1 Use of Color | https://www.w3.org/WAI/WCAG22/quickref/#use-of-color |
| 1.4.3 Contrast (Minimum) | https://www.w3.org/WAI/WCAG22/quickref/#contrast-minimum |
| 1.4.11 Non-text Contrast | https://www.w3.org/WAI/WCAG22/quickref/#non-text-contrast |
| 1.4.12 Text Spacing | https://www.w3.org/WAI/WCAG22/quickref/#text-spacing |
| 1.4.13 Hover/Focus Content | https://www.w3.org/WAI/WCAG22/quickref/#content-on-hover-or-focus |
| 2.1.1 Keyboard | https://www.w3.org/WAI/WCAG22/quickref/#keyboard |
| 2.1.2 No Keyboard Trap | https://www.w3.org/WAI/WCAG22/quickref/#no-keyboard-trap |
| 2.4.1 Bypass Blocks | https://www.w3.org/WAI/WCAG22/quickref/#bypass-blocks |
| 2.4.2 Page Titled | https://www.w3.org/WAI/WCAG22/quickref/#page-titled |
| 2.4.3 Focus Order | https://www.w3.org/WAI/WCAG22/quickref/#focus-order |
| 2.4.4 Link Purpose | https://www.w3.org/WAI/WCAG22/quickref/#link-purpose-in-context |
| 2.4.7 Visible Focus | https://www.w3.org/WAI/WCAG22/quickref/#focus-visible |
| 3.1.1 Language of Page | https://www.w3.org/WAI/WCAG22/quickref/#language-of-page |
| 3.3.1 Error Identification | https://www.w3.org/WAI/WCAG22/quickref/#error-identification |
| 3.3.2 Labels or Instructions | https://www.w3.org/WAI/WCAG22/quickref/#labels-or-instructions |
| 3.3.3 Error Suggestion | https://www.w3.org/WAI/WCAG22/quickref/#error-suggestion |
| 4.1.2 Name, Role, Value | https://www.w3.org/WAI/WCAG22/quickref/#name-role-value |
| 4.1.3 Status Messages | https://www.w3.org/WAI/WCAG22/quickref/#status-messages |