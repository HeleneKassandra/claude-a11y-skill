---
name: wcag-audit
description: >
  Use this skill whenever the user wants to audit, check, review, or scan any HTML, React/JSX, CSS, or code snippet for accessibility issues, WCAG violations, or improvements. Trigger this skill for any of the following: check my code for accessibility, WCAG audit, a11y review, is this accessible, screen reader issues, keyboard navigation problems, color contrast check, ARIA errors, accessibility score, fix my accessibility issues, does this pass WCAG, accessibility compliance, or any question about WCAG guidelines and how to meet them. Also trigger when the user pastes code and asks for a review even if they do not mention accessibility if the code is clearly UI/frontend and accessibility is an obvious concern. Always use this skill for accessibility-related tasks. Never skip it.
---

# WCAG Accessibility Audit Skill

Conduct structured WCAG 2.1/2.2 accessibility audits on HTML, React/JSX, CSS, and code snippets. Target **Level AA compliance**, and flag small-effort **Level AAA upgrades** where applicable.

> **Primary guidance source:** [Tilsynet for universell utforming av ikt (uutilsynet.no)](https://www.uutilsynet.no/veiledning/losningsforslag-krav/1366) — the Norwegian supervisory authority for universal design of ICT. Their løsningsforslag (solution proposals) cover all 48 WCAG criteria and are the authoritative reference for this skill. Apply their test rules and expected outcomes when auditing, and link to their criterion pages where relevant.

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
| Semantic HTML | Correct element usage, heading hierarchy, landmark regions |
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

```
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
**WCAG Success Criterion:** [1.1.1 Non-text Content](https://www.w3.org/WAI/WCAG21/Understanding/non-text-content)

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

### ✅ What's Already Accessible
List what the code does correctly. Be specific — name the elements and patterns that pass.

---

### 📋 Quick Fix Checklist
- [ ] Fix 1 (Critical)
- [ ] Fix 2 (High)
- [ ] Fix 3 (Medium)
```

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

*Code examples sourced from [uutilsynet.no løsningsforslag](https://www.uutilsynet.no/veiledning/losningsforslag-krav/1366)*

### 4.1.2 — Forms: Name, Role, Value
uutilsynet: *"Skjemaelementet er koblet til en ledetekst i koden"* (The form element must be connected to a label in the code.)

```html
<!-- ❌ No label -->
<input type="text" name="Fornavn">

<!-- ✅ Explicit for/id pairing (uutilsynet recommended) -->
<label for="Fornavn">Fornavn:</label>
<input type="text" name="Fornavn" id="Fornavn">

<!-- ✅ Checkbox with label after -->
<input type="checkbox" name="abonner" id="abonner">
<label for="abonner">Abonner på nyhetsbrev</label>

<!-- ✅ Search field with aria-label -->
<input type="text" name="søk" aria-label="Søk">
<button type="submit">Søk</button>

<!-- ✅ Search field with aria-labelledby -->
<input type="text" name="søk" aria-labelledby="sokknapp">
<button id="sokknapp" type="submit">Søk</button>
```

### 4.1.2 — Grouped Form Fields (fieldset/legend)
uutilsynet: *"Dersom skjemaelementet hører til en gruppe, er det i tillegg koblet til en ledetekst som gjelder for gruppa."*

```html
<!-- ✅ Radio group with fieldset + legend (uutilsynet example) -->
<fieldset>
  <legend>Filformat</legend>
  <div>
    <input type="radio" name="format" id="tekst" value="txt" checked>
    <label for="tekst">Tekstfil</label>
  </div>
  <div>
    <input type="radio" name="format" id="csv" value="csv">
    <label for="csv">CSV-fil</label>
  </div>
</fieldset>
```

### 4.1.2 — Buttons
uutilsynet: *"Knapper som ikke er kodet med standardelementene i (X)HTML, skal ha informasjon om rolle."* Always prefer native elements.

```html
<!-- ✅ Native button elements (uutilsynet recommended) -->
<button>Send inn</button>
<button>Avbryt</button>
<input type="submit" value="Send inn">
<input type="button" value="Avbryt">

<!-- ✅ Icon button with aria-label -->
<button aria-label="Send inn"><img src="img.png" alt=""></button>
```

### 4.1.2 — iframes
```html
<!-- ✅ iframe with title (uutilsynet example) -->
<iframe src="https://www.w3schools.com" title="W3Schools Gratis nettveiledning"></iframe>
```

### 2.4.7 — Visible Focus
uutilsynet: *"Alt innhold brukeren kan navigere til med tastatur skal få synlig fokusmarkering."* Acceptable forms include: border, underline, colour change, shadow, or text cursor. The browser default is often insufficient — supplement it with CSS.

```css
/* ❌ Removes browser default without replacement */
*:focus { outline: none; }

/* ✅ uutilsynet example: change colour on focus */
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
uutilsynet compliance paths:
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
uutilsynet: *"Ting skal være kodet som det ser ut som."* (Things must be coded as they appear.)

```html
<!-- ❌ Visual heading not coded as heading -->
<div class="heading-style">Om oss</div>

<!-- ✅ Heading coded correctly with right level -->
<h2>Om oss</h2>

<!-- ❌ Visual list without list semantics -->
<div>• Punkt en</div>
<div>• Punkt to</div>

<!-- ✅ Unordered list -->
<ul>
  <li>Punkt en</li>
  <li>Punkt to</li>
</ul>

<!-- ✅ Data table with caption and th scope -->
<table>
  <caption>Månedlig salg</caption>
  <thead>
    <tr>
      <th scope="col">Måned</th>
      <th scope="col">Salg</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Januar</td>
      <td>120 000 kr</td>
    </tr>
  </tbody>
</table>
```

### Skip Navigation (2.4.1)
```html
<!-- ✅ Skip link — must be at top of body -->
<a href="#main-content" class="skip-link">Hopp til hovedinnhold</a>

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

When auditing React components, also verify:

- **`key` props** don't use index when list is reorderable (can disrupt screen reader order)
- **`onClick` on non-interactive elements** → replace with `<button>` or add `role`, `tabIndex`, `onKeyDown`
- **Dynamic content updates** → use `aria-live` regions or focus management after async operations
- **Images via JSX** → `alt` prop always required (empty string `""` for decorative)
- **Custom hooks** managing focus → ensure `useEffect` restores focus after modal/dialog closes
- **Fragment usage** → don't wrap `<label>` and `<input>` in ways that break association

```jsx
// ❌ onClick on div — not keyboard accessible
<div onClick={handleClick}>Open Menu</div>

// ✅ Proper button with ARIA
<button
  type="button"
  onClick={handleClick}
  aria-expanded={isOpen}
  aria-controls="menu-list"
>
  Open Menu
</button>
```

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

## uutilsynet.no Criterion Pages

Direct links to the Norwegian supervisory authority's løsningsforslag (solution proposals) for each criterion. Use these as authoritative references when explaining violations or fixes:

| Criterion | URL |
|---|---|
| 1.1.1 Non-text Content | https://www.uutilsynet.no/veiledning/111-ikke-tekstlig-innhold/1231 |
| 1.3.1 Info and Relationships | https://www.uutilsynet.no/veiledning/131-informasjon-og-relasjoner/1234 |
| 1.3.5 Input Purpose | https://www.uutilsynet.no/veiledning/135-identifiser-formal-med-inndata/1238 |
| 1.4.1 Use of Color | https://www.uutilsynet.no/veiledning/141-bruk-av-farge/1239 |
| 1.4.3 Contrast (Minimum) | https://www.uutilsynet.no/veiledning/143-kontrast-minimum/1245 |
| 1.4.11 Non-text Contrast | https://www.uutilsynet.no/veiledning/1411-kontrast-ikke-tekstlig-innhold/1241 |
| 1.4.12 Text Spacing | https://www.uutilsynet.no/veiledning/1412-tekstavstand/1242 |
| 1.4.13 Hover/Focus Content | https://www.uutilsynet.no/veiledning/1413-pekerfolsomt-innhold-eller-innhold-ved-tastaturfokus/1243 |
| 2.1.1 Keyboard | https://www.uutilsynet.no/veiledning/211-tastatur/1036 |
| 2.1.2 No Keyboard Trap | https://www.uutilsynet.no/veiledning/212-ingen-tastaturfelle/1247 |
| 2.4.1 Bypass Blocks | https://www.uutilsynet.no/veiledning/241-hoppe-over-blokker/1041 |
| 2.4.2 Page Titled | https://www.uutilsynet.no/veiledning/242-sidetitler/1250 |
| 2.4.3 Focus Order | https://www.uutilsynet.no/veiledning/243-fokusrekkefolge/1043 |
| 2.4.4 Link Purpose | https://www.uutilsynet.no/veiledning/244-formal-med-lenke-i-kontekst/1251 |
| 2.4.7 Visible Focus | https://www.uutilsynet.no/veiledning/247-synlig-fokus/1047 |
| 3.1.1 Language of Page | https://www.uutilsynet.no/veiledning/311-sprak-pa-siden/1258 |
| 3.3.1 Error Identification | https://www.uutilsynet.no/veiledning/331-identifikasjon-av-feil/1262 |
| 3.3.2 Labels or Instructions | https://www.uutilsynet.no/veiledning/332-ledetekster-eller-instruksjoner/1263 |
| 3.3.3 Error Suggestion | https://www.uutilsynet.no/veiledning/333-forslag-ved-feil/1264 |
| 4.1.2 Name, Role, Value | https://www.uutilsynet.no/veiledning/412-navn-rolle-verdi/1267 |
| 4.1.3 Status Messages | https://www.uutilsynet.no/veiledning/413-statusbeskjeder/1268 |