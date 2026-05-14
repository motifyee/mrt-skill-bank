# UX Principles & Accessibility

Professional UX and accessibility foundations for every design system. These rules
are mandatory — aesthetic choices must never override them.

---

## Core UX Principles

### 1. Clarity over cleverness
Every interface element should communicate its purpose without explanation. If a user
has to guess what a button does, the label is wrong. If they have to hunt for the
primary action, the hierarchy is wrong.

### 2. Consistency reduces cognitive load
Same action, same appearance, everywhere. Once a user learns that orange means
"primary action," every orange element must be a primary action. Inconsistency forces
re-learning on every screen.

### 3. Progressive disclosure
Show only what's needed for the current task. Secondary options, advanced settings,
and edge-case flows hide behind intentional interaction (expand, click, hover).
The default view serves the primary use case.

### 4. Feedback for every action
Users must know: what happened, whether it worked, and what to do next.
Every interaction needs a visible response within 100ms (perceived instant),
a progress indicator within 1s, and a completion signal within 10s.

### 5. Error prevention over error recovery
Design the interface so mistakes are difficult. Constrain inputs, confirm destructive
actions, provide undo instead of "are you sure?" dialogs. When errors do occur,
explain what went wrong and how to fix it — never blame the user.

### 6. Respect user attention
Animate purposefully, not decoratively. Interrupt only for urgent information.
Default to quiet — banners, toasts, and modals should be earned by importance,
not used for routine status.

---

## Accessibility Standards (WCAG 2.2 AA baseline)

Every generated design system must meet WCAG 2.2 AA as a minimum floor.

### Color Contrast

| Element Type        | Minimum Ratio | How to Verify |
|---------------------|---------------|---------------|
| Normal text (<18px, or <14px bold) | 4.5:1 | Check fg on bg |
| Large text (>=18px, or >=14px bold)  | 3:1   | Check fg on bg |
| UI components & graphical objects   | 3:1   | Check borders, icons on bg |
| Focus indicators     | 3:1 against adjacent colors | Check ring against bg AND element |

Rules:
- Never rely on color alone to convey meaning. Pair with icons, text, or patterns.
- Semantic colors (success green, error red) must have text labels alongside.
- Test contrast for every bg/fg combination in the token system, including dark mode.

### Keyboard Navigation

All interactive elements must be:
- Reachable via Tab (in logical order matching visual order)
- Activatable via Enter/Space
- Dismissible via Escape (modals, dropdowns, overlays)
- Navigable via arrow keys (within groups: tabs, radio buttons, menus)

Focus must be:
- Visible: a clear indicator (ring, outline, underline) that meets 3:1 contrast
- Trapped appropriately: focus stays inside modals until dismissed
- Restored: when a modal closes, focus returns to the element that triggered it

### Screen Reader Support

Semantic HTML is the primary accessibility tool:
- Use `<button>` for actions, `<a>` for navigation, `<input>` for data entry
- Use heading hierarchy (`h1` -> `h2` -> `h3`) in document order
- Use `<nav>`, `<main>`, `<aside>`, `<footer>` landmark regions
- Use `<ul>`/`<ol>` for lists that screen readers should announce as lists

ARIA attributes supplement where HTML falls short:
- `aria-label` or `aria-labelledby` for elements without visible text
- `aria-describedby` for supplementary instructions
- `aria-live="polite"` for dynamic content updates (toasts, live regions)
- `aria-expanded` for disclosure widgets (accordions, dropdowns)
- `role="dialog"` + `aria-modal="true"` for modal dialogs
- `aria-hidden="true"` for decorative elements that add noise

### Motion and Vestibular

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

Include this in every generated CSS file. No exceptions.

### Touch Targets

Minimum interactive target size: 44x44px (WCAG 2.5.5 AAA), 24x24px minimum for AA (WCAG 2.2 criterion 2.5.8 "Target Size (Minimum)"), 48x48px preferred (Material).
This applies to buttons, links, checkboxes, radio buttons, and any tappable element.
Adjacent targets must have at least 8px spacing to prevent mistaps.

---

## Interaction States

Every interactive component must define these states:

| State       | What triggers it            | Visual treatment                        |
|-------------|-----------------------------|-----------------------------------------|
| Default     | Component at rest           | Base appearance from tokens              |
| Hover       | Mouse enters (desktop only) | Subtle change: color shift, lift, underline |
| Focus       | Keyboard Tab arrives        | Visible ring/outline, 3:1 contrast       |
| Active/Press| Mouse down or touch start   | Deeper color, slight scale down (0.97-0.98) |
| Disabled    | Functionality unavailable   | Reduced opacity (0.4), no pointer cursor |
| Loading     | Async operation in progress | Spinner or skeleton, disabled interaction |
| Error       | Validation failed           | Error color border, error message visible |
| Success     | Action completed            | Success color indicator, brief feedback  |

### State priority
When multiple states apply simultaneously: Error > Disabled > Focus > Active > Hover > Default.
Focus must remain visible even when combined with hover.

---

## Page-Level States

Design systems must define visual patterns for these page states:

### Empty states
When there is no content to display. Show:
- A clear explanation of why it's empty
- An illustration or icon (contextual, not generic)
- A primary action to create/add content
- Never: a blank page, a generic "no data" message, or a broken-looking layout

### Loading states
While content is being fetched. Use:
- Skeleton screens for known content shapes (preferred over spinners)
- Progress bars for operations with known duration
- Spinners only for indeterminate waits < 5 seconds
- Never: blocking the entire page for a partial data load

### Error states
When something has gone wrong. Show:
- What happened (in plain language, not error codes)
- What the user can do about it (retry, contact support, try a different approach)
- Never: "Something went wrong." with no recovery path
- Never: stack traces, status codes, or technical jargon in user-facing UI

---

## Internationalization and RTL Accessibility

See `responsive_system.md` for RTL CSS implementation and logical property usage. This section covers the accessibility requirements for multilingual and bidirectional interfaces.

### Language and Script Considerations

- **Font stacks must include CJK fallbacks** for East Asian languages: add `"Noto Sans SC"`, `"Noto Sans JP"`, `"Noto Sans KR"` after the primary Latin font.
- **Thai, Khmer, and Lao scripts have no word spacing.** CSS `word-break: break-word` or `overflow-wrap: break-word` is required for these languages. Test layout with Thai text to catch overflow issues.
- **German and Finnish words can be extremely long** (30+ characters). Test labels, button text, and table headers with compound words. Allow text wrapping in buttons when translated strings exceed the English width.
- **Arabic and Hebrew scripts are connected** — letterforms change based on position. Ensure fonts have proper contextual alternates enabled.

### Bidirectional (BiDi) Layout Rules

- **Use CSS logical properties everywhere** (`margin-inline-start` not `margin-left`, `padding-block-end` not `padding-bottom`). This is mandatory, not optional.
- **Never hardcode directional assumptions** in component designs. "Right arrow = next" fails in RTL; use `→` in LTR and `←` in RTL.
- **Icons with directional meaning must flip in RTL.** Chevron-right for "expand" becomes chevron-left. Use `transform: scaleX(-1)` on `[dir="rtl"]`.
- **Navigation order follows reading direction.** Tab order in RTL should move right-to-left through the page.
- **Text alignment defaults** must use `text-align: start` (logical) rather than `text-align: left` (physical).

### Accessibility Testing for i18n

Required testing languages (minimum):

| Language | Script | Direction | Tests |
|----------|--------|-----------|-------|
| English | Latin | LTR | Baseline layout |
| Arabic | Arabic | RTL | BiDi layout, text alignment, icon flipping |
| German | Latin | LTR | Long words, wrapping, button sizing |
| Chinese (Simplified) | CJK | LTR | Character width, line height, font fallback |
| Thai | Thai | LTR | No word spacing, line breaking |

### Common i18n Accessibility Failures

1. **Truncated translated text.** English UI strings are often 30-50% shorter than German, French, or Russian. Buttons, labels, and nav items must accommodate 2x text expansion.
2. **Color-only indicators in cultures with different associations.** Red does not mean "error" in all cultures. Pair color with icon or text.
3. **Date and number format assumptions.** `MM/DD/YYYY` is US-only. Use locale-aware formatting. Currency symbols change position (€1.000,00 in German vs $1,000.00 in English).
4. **Screen reader language declaration.** Every page must declare `lang` on `<html>`. Mixed-language content needs `lang` attributes on individual elements.
5. **Right-to-left form fields without proper `dir` attribute.** A text input in Arabic without `dir="rtl"` renders text left-aligned, which is disorienting.

---

## Responsive Design Principles

See `responsive_system.md` for complete breakpoint definitions and responsive design rules.

### Accessibility-specific responsive rules

These rules supplement the responsive system with accessibility considerations:

- **Touch targets**: enforce 48px at mobile, 44px acceptable at desktop (WCAG 2.5.5 AAA minimum); note WCAG 2.2 adds 2.5.8 "Target Size (Minimum)" at 24px for AA, while 2.5.5 remains the AAA target at 44px
- **Content reflow**: text never extends beyond ~72ch (about 65-75 characters per line) to maintain readability
- **Images**: maintain aspect ratio and never clip important content
- **Tables**: scroll horizontally on mobile or convert to stacked card views
- **Sidebars**: collapse to top-of-page or off-canvas on mobile
