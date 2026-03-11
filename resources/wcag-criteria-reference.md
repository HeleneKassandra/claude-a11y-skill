# WCAG 2.1 / 2.2 Criteria Reference

All Level A and Level AA success criteria, with plain-language summaries. AAA criteria that are achievable with small effort are also listed.

---

## Principle 1 — Perceivable

### 1.1 Text Alternatives
| SC | Level | Requirement |
|---|---|---|
| 1.1.1 Non-text Content | A | All images, icons, charts, and non-text elements must have a text alternative. Decorative images use `alt=""`. |

### 1.2 Time-based Media
| SC | Level | Requirement |
|---|---|---|
| 1.2.1 Audio-only / Video-only | A | Pre-recorded audio must have a transcript. Pre-recorded silent video must have a text alternative. |
| 1.2.2 Captions (Prerecorded) | A | Pre-recorded video with audio must have synchronized captions. |
| 1.2.3 Audio Description or Media Alternative | A | Pre-recorded video must have audio description or full text transcript. |
| 1.2.4 Captions (Live) | AA | Live video with audio must have live captions. |
| 1.2.5 Audio Description (Prerecorded) | AA | Pre-recorded video must have audio description for visual-only information. |

### 1.3 Adaptable
| SC | Level | Requirement |
|---|---|---|
| 1.3.1 Info and Relationships | A | Structure conveyed visually must also be conveyed in code (headings, lists, tables, labels, fieldsets). |
| 1.3.2 Meaningful Sequence | A | Reading order in code must match visual reading order. |
| 1.3.3 Sensory Characteristics | A | Instructions must not rely solely on shape, color, size, or location ("click the round button"). |
| 1.3.4 Orientation | AA | Content must not be locked to portrait or landscape unless essential. |
| 1.3.5 Identify Input Purpose | AA | Form fields for personal info must use `autocomplete` attributes. |

### 1.4 Distinguishable
| SC | Level | Requirement |
|---|---|---|
| 1.4.1 Use of Color | A | Color must not be the only visual means of conveying information. |
| 1.4.2 Audio Control | A | Auto-playing audio must be stoppable or controllable within 3 seconds. |
| 1.4.3 Contrast (Minimum) | AA | Normal text: 4.5:1. Large text: 3:1. |
| 1.4.4 Resize Text | AA | Text must be resizable up to 200% without loss of content or functionality. |
| 1.4.5 Images of Text | AA | Avoid using images of text; use real text instead. |
| 1.4.10 Reflow | AA | Content must not require horizontal scrolling at 320px viewport width. |
| 1.4.11 Non-text Contrast | AA | UI components and graphics must have 3:1 contrast against adjacent colors. |
| 1.4.12 Text Spacing | AA | Content must remain readable when letter-spacing, word-spacing, line-height, and paragraph spacing are increased. |
| 1.4.13 Content on Hover or Focus | AA | Hover/focus-triggered content must be dismissible, hoverable, and persistent. |
| 1.4.6 Contrast (Enhanced) | **AAA** | Normal text: 7:1. Large text: 4.5:1. |

---

## Principle 2 — Operable

### 2.1 Keyboard Accessible
| SC | Level | Requirement |
|---|---|---|
| 2.1.1 Keyboard | A | All functionality must be operable by keyboard alone. |
| 2.1.2 No Keyboard Trap | A | Users must be able to navigate away from any component using only the keyboard. |
| 2.1.4 Character Key Shortcuts | A | Single-character keyboard shortcuts must be remappable, disableable, or only active on focus. |

### 2.2 Enough Time
| SC | Level | Requirement |
|---|---|---|
| 2.2.1 Timing Adjustable | A | Users must be able to turn off, adjust, or extend time limits. |
| 2.2.2 Pause, Stop, Hide | A | Moving, blinking, or scrolling content must be pausable. |

### 2.3 Seizures and Physical Reactions
| SC | Level | Requirement |
|---|---|---|
| 2.3.1 Three Flashes or Below Threshold | A | Content must not flash more than 3 times per second. |

### 2.4 Navigable
| SC | Level | Requirement |
|---|---|---|
| 2.4.1 Bypass Blocks | A | A skip navigation link (or equivalent) must be provided to bypass repeated content. |
| 2.4.2 Page Titled | A | Pages must have a descriptive `<title>`. |
| 2.4.3 Focus Order | A | Tab order must preserve meaning and operability. |
| 2.4.4 Link Purpose (In Context) | A | Link purpose must be determinable from the link text or context. |
| 2.4.5 Multiple Ways | AA | Multiple ways to find pages must be provided (search, sitemap, nav). |
| 2.4.6 Headings and Labels | AA | Headings and labels must be descriptive. |
| 2.4.7 Focus Visible | AA | Keyboard focus indicator must be visible. |
| 2.4.11 Focus Not Obscured (Minimum) | AA *(2.2)* | Focused component must not be entirely hidden by sticky headers/overlays. |
| 2.4.12 Focus Not Obscured (Enhanced) | AAA *(2.2)* | Focused component must be fully visible. |

### 2.5 Input Modalities
| SC | Level | Requirement |
|---|---|---|
| 2.5.1 Pointer Gestures | A | Multipoint or path-based gestures must have a single-pointer alternative. |
| 2.5.2 Pointer Cancellation | A | Single-pointer actions must be cancellable (use `mouseup` not `mousedown` for destructive actions). |
| 2.5.3 Label in Name | A | Accessible name of a component must contain its visible label text. |
| 2.5.4 Motion Actuation | A | Functions triggered by device motion must have a UI alternative and be disableable. |
| 2.5.7 Dragging Movements | AA *(2.2)* | Drag operations must have a single-pointer alternative. |
| 2.5.8 Target Size (Minimum) | AA *(2.2)* | Touch targets must be at least 24×24 CSS pixels (44×44 strongly recommended). |

---

## Principle 3 — Understandable

### 3.1 Readable
| SC | Level | Requirement |
|---|---|---|
| 3.1.1 Language of Page | A | The `lang` attribute on `<html>` must identify the primary language. |
| 3.1.2 Language of Parts | AA | Passages in a different language must be identified with `lang` on the element. |

### 3.2 Predictable
| SC | Level | Requirement |
|---|---|---|
| 3.2.1 On Focus | A | Receiving focus must not trigger a context change. |
| 3.2.2 On Input | A | Changing a setting must not cause a context change unless user is informed in advance. |
| 3.2.3 Consistent Navigation | AA | Navigation must appear in the same order across pages. |
| 3.2.4 Consistent Identification | AA | Components with the same function must be identified consistently. |
| 3.2.6 Consistent Help | AA *(2.2)* | Help mechanisms (chatbot, phone, FAQ) must appear in a consistent location. |

### 3.3 Input Assistance
| SC | Level | Requirement |
|---|---|---|
| 3.3.1 Error Identification | A | Input errors must be identified in text and described to the user. |
| 3.3.2 Labels or Instructions | A | Labels or instructions must be provided for user input. |
| 3.3.3 Error Suggestion | AA | When an input error is detected, suggestions for correction must be provided. |
| 3.3.4 Error Prevention (Legal, Financial, Data) | AA | Submissions must be reversible, checkable, or confirmable. |
| 3.3.7 Redundant Entry | AA *(2.2)* | Information already provided must not be re-entered in the same session. |
| 3.3.8 Accessible Authentication (Minimum) | AA *(2.2)* | Authentication must not require cognitive function tests (CAPTCHA) unless an alternative is provided. |

---

## Principle 4 — Robust

### 4.1 Compatible
| SC | Level | Requirement |
|---|---|---|
| 4.1.1 Parsing | A | HTML must have complete start/end tags, no duplicate attributes, properly nested elements. (Deprecated in WCAG 2.2 but still good practice.) |
| 4.1.2 Name, Role, Value | A | All UI components must have a name and role exposed to assistive technologies, and state/properties must be programmable. |
| 4.1.3 Status Messages | AA | Status messages (success, error, loading) must be conveyed to AT without receiving focus. Use `role="status"`, `role="alert"`, or `aria-live`. |

---

## Quick Decision Tree: Which Criterion Applies?

**Image without text?** → 1.1.1  
**Video or audio?** → 1.2.x  
**Heading or list structure?** → 1.3.1  
**Color only conveys info?** → 1.4.1  
**Text contrast?** → 1.4.3  
**UI component contrast?** → 1.4.11  
**Hover/tooltip behavior?** → 1.4.13  
**Can't use keyboard?** → 2.1.1  
**No skip link?** → 2.4.1  
**No page title?** → 2.4.2  
**Focus not visible?** → 2.4.7  
**Touch target too small?** → 2.5.8  
**Missing form label?** → 1.3.1, 3.3.2  
**Error not described?** → 3.3.1  
**Missing lang attribute?** → 3.1.1  
**ARIA name/role missing?** → 4.1.2  
**Status message not announced?** → 4.1.3