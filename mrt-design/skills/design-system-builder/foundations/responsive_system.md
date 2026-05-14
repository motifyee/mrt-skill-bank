# Responsive System

Breakpoint strategy, mobile-first rules, and viewport adaptation patterns.
Cross-reference: layout compositions in `layout_compositions.md`, tokens in `visual_system.md`.

## Design Principles

1. **Write base styles for the smallest screen first:** Add complexity with `min-width` queries; never use `max-width` or remove styling at larger breakpoints.
2. **Touch targets are non-negotiable:** Every interactive element must meet the 44x44px minimum (WCAG 2.5.5) regardless of its visual size.
3. **Use `clamp()` for fluid typography:** Scale font sizes smoothly between breakpoints rather than jumping at discrete media queries.
4. **Container queries for components, media queries for pages:** Respond to parent width for reusable cards and widgets; respond to viewport width for navigation and page-level shells.
5. **Plan for text expansion in translation:** Use `min-width` instead of fixed `width`, allow wrapping on labels, and test with German (1.3x) and Arabic (1.4x) expansion factors.

---

## Breakpoint Tokens

Standard breakpoints as design tokens. Use with `min-width` queries only (mobile-first).

```css
:root {
  --bp-xs: 0;       /* Base: mobile-first target */
  --bp-sm: 640px;   /* Large phones / small tablets */
  --bp-md: 768px;   /* Tablets portrait */
  --bp-lg: 1024px;  /* Tablets landscape / small laptops */
  --bp-xl: 1280px;  /* Laptops / desktops */
  --bp-2xl: 1440px; /* Large desktops */
}
```

Media query usage:
```css
/* Mobile-first: base styles need no query */
.element { /* mobile styles */ }

@media (min-width: 640px)  { /* sm */ }
@media (min-width: 768px)  { /* md */ }
@media (min-width: 1024px) { /* lg */ }
@media (min-width: 1280px) { /* xl */ }
@media (min-width: 1440px) { /* 2xl */ }
```

---

## Grid Behavior Per Breakpoint

| Layout            | Mobile (<640)         | Tablet (640-1023)     | Desktop (1024+)        |
|-------------------|------------------------|------------------------|-------------------------|
| SaaS Shell        | Hamburger + stack      | Icon sidebar (56px)   | Full sidebar (240px)   |
| Hero Centered     | Full-width, compact    | Full-width            | Full-width, spacious   |
| Hero Split        | Stacked (text then img)| Side-by-side          | Side-by-side           |
| Feature Grid      | 1 column               | 2 columns             | 3 columns              |
| Bento Grid        | 1 column               | 2 columns             | 4 columns              |
| Logo Wall         | 3 per row, small       | 4-5 per row           | 6+ per row             |
| Metrics Band      | 2x2 grid               | 2x2 grid              | 1x4 row                |
| Testimonials      | 1 column / scroll      | 2 columns             | 3 columns              |
| Pricing           | Stacked, featured first| 3 columns, compact    | 3 columns, full        |
| Documentation     | Nav drawer + content   | Nav + content         | Nav + content + TOC    |
| Split View        | Tab toggle             | Stacked               | Side-by-side           |
| Timeline          | Vertical               | Horizontal, compact   | Horizontal, full       |

See `layout_compositions.md` for layout-specific column behavior and responsive stacking patterns per composition.

---

## Page Grid System

A 12-column CSS Grid template for page-level layout. Use this as the structural foundation that compositions (`layout_compositions.md`) sit on top of.

### Grid Template Tokens

```css
:root {
  --grid-columns: 12;
  --grid-gutter: clamp(16px, 2vw, 32px);
  --grid-margin: clamp(16px, 5vw, 80px);
  --grid-max-width: 1280px;
}
```

### Base Grid Setup

```css
.page-grid {
  display: grid;
  grid-template-columns: repeat(var(--grid-columns), 1fr);
  gap: var(--grid-gutter);
  max-width: var(--grid-max-width);
  margin-inline: auto;
  padding-inline: var(--grid-margin);
}

.page-grid--full-bleed {
  max-width: none;
  padding-inline: 0;
}

.page-grid--narrow {
  --grid-max-width: 960px;
}

.page-grid--wide {
  --grid-max-width: 1440px;
}
```

### Named Grid Areas

For pages with consistent structural zones (header, sidebar, main, footer):

```css
/* Base: single-column layout for mobile */
.page-grid--app {
  grid-template-areas:
    "header"
    "main"
    "footer";
  grid-template-columns: 1fr;
}
@media (min-width: 1024px) {
  .page-grid--app {
    grid-template-areas:
      "header  header  header"
      "sidebar main    main"
      "footer  footer  footer";
    grid-template-columns: 240px 1fr 1fr;
    grid-template-rows: auto 1fr auto;
    min-height: 100dvh;
  }
}
```

### Column Span Conventions

Standard span values for content blocks within the 12-column grid:

| Span | Columns | Use Case |
|------|---------|----------|
| Full | 12 | Heroes, full-width banners, footers |
| 2/3 | 8 | Primary content (next to sidebar) |
| 1/3 | 4 | Sidebar, TOC, filters |
| 1/2 | 6 | Two-column feature sections, split heroes |
| 1/4 | 3 | Four-column feature grids, logo walls |
| 1/6 | 2 | Icon columns, narrow metric cards |
| 5/12 | 5 | Asymmetric content (wider side) |
| 7/12 | 7 | Asymmetric content (narrow side) |

```css
/* Base: all columns span full width on mobile */
.col-full,
.col-2-3, .col-1-3, .col-1-2,
.col-1-4, .col-1-6, .col-5-12, .col-7-12 {
  grid-column: span 12;
}

@media (min-width: 640px) {
  .col-1-4 { grid-column: span 6; }
  .col-1-6 { grid-column: span 4; }
  .col-2-3 { grid-column: span 8; }
  .col-1-3 { grid-column: span 4; }
  .col-1-2 { grid-column: span 6; }
  .col-5-12 { grid-column: span 5; }
  .col-7-12 { grid-column: span 7; }
}

@media (min-width: 1024px) {
  .col-1-4 { grid-column: span 3; }
  .col-1-6 { grid-column: span 2; }
}
```

### Nested Sub-Grids

Content blocks that need their own internal grid can use CSS subgrid (where supported) or a nested grid:

```css
.feature-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: var(--grid-gutter);
  grid-column: span 12;
}

@media (min-width: 1024px) {
  .feature-grid {
    grid-column: span 8;
  }
}
```

---

## Mobile-First Rules

### Core Principles

1. **Write base styles for the smallest screen.** Add complexity at breakpoints, never remove it.
2. **Never use `max-width` queries.** Mobile-first uses `min-width` ascending. `max-width` creates specificity conflicts and overrides that scale poorly.
3. **Progressive enhancement, not graceful degradation.** Start with what works everywhere, then layer in advanced layout.

### Touch Targets

```css
/* Minimum tap target: 44x44px (WCAG 2.5.5) */
.touch-target {
  min-height: 44px;
  min-width: 44px;
}

/* Spacing between interactive elements */
.interactive-group {
  gap: var(--space-3); /* 12px minimum between tappable elements */
}
```

- Buttons: minimum 44px height. Padding compensates for small text.
- Links in body text: no extra requirement, but inline links in lists need 8px vertical spacing.
- Icon buttons: render at least 44px tap area even if visual icon is 20px.

### Navigation Patterns

**Mobile:** Hamburger menu with slide-in or overlay panel.
```css
.nav-mobile {
  position: fixed;
  inset: 0;
  background: var(--bg);
  z-index: 100;
  transform: translateX(100%);
  transition: transform var(--dur-base) var(--ease);
}
.nav-mobile[open] { transform: translateX(0); }
```

**App-style:** Bottom navigation bar for tab-based mobile apps.
```css
.nav-bottom {
  position: fixed;
  bottom: 0;
  inset-inline: 0;
  display: flex;
  justify-content: space-around;
  background: var(--bg);
  border-top: 1px solid var(--border);
  padding: var(--space-2) 0;
  z-index: 50;
}
```

**Desktop:** Horizontal top nav or sidebar (see SaaS Shell in `layout_compositions.md`).

### Content Width

```css
/* Single column on mobile, progressive width at breakpoints */
.content {
  max-width: 72ch; /* Optimal reading width */
  margin-inline: auto;
  padding-inline: var(--space-4);
}
@media (min-width: 768px) {
  .content { padding-inline: var(--space-6); }
}
@media (min-width: 1280px) {
  .content { padding-inline: var(--space-8); }
}
```

---

## Fluid Typography

Use `clamp()` for type that scales smoothly between breakpoints without media queries.

**Formula:**
```
clamp(min, preferred, max)
preferred = min + (max - min) * ((100vw - bp-min) / (bp-max - bp-min))
```

```css
:root {
  /* Fluid heading: 28px at 320px viewport, 56px at 1280px+ */
  --fs-h1: clamp(1.75rem, 1.3rem + 1.8vw, 3.5rem);
  --fs-h2: clamp(1.5rem, 1.1rem + 1.4vw, 2.5rem);
  --fs-h3: clamp(1.25rem, 1rem + 0.9vw, 1.75rem);
  --fs-body: clamp(0.938rem, 0.85rem + 0.3vw, 1.063rem);
  --fs-body-sm: clamp(0.813rem, 0.75rem + 0.2vw, 0.875rem);
}
```

### Spacing Reduction

Reduce section padding proportionally on smaller viewports:

```css
.section {
  padding-block: var(--space-10);
}
@media (min-width: 768px) {
  .section { padding-block: calc(var(--space-10) * 1.3); }
}
@media (min-width: 1280px) {
  .section { padding-block: calc(var(--space-10) * 1.5); }
}
```

Or use a fluid spacer:
```css
:root {
  --section-py: clamp(3rem, 2rem + 4vw, 6rem);
}
```

---

## Container Queries

For component-level responsive behavior that depends on parent width, not viewport.

**When to use:**
- Reusable components placed in variable-width containers
- Card components that appear in sidebars and main content
- Widget systems and embeddable components
- CMS-driven layouts where the author controls container width

**When NOT to use:**
- Page-level layouts (stick with media queries)
- Navigation state (hamburger menu is viewport-driven)
- Any time you need to respond to the viewport itself

```css
/* Define the container */
.sidebar { container-type: inline-size; }

/* Query the container */
@container (min-width: 300px) {
  .user-card { flex-direction: row; }
}
@container (min-width: 500px) {
  .user-card__meta { display: grid; grid-template-columns: 1fr 1fr; }
}
```

---

## Responsive Images

### srcset and sizes

```html
<img
  src="photo-800.jpg"
  srcset="photo-400.jpg 400w, photo-800.jpg 800w, photo-1200.jpg 1200w"
  sizes="(max-width: 640px) 100vw, (max-width: 1280px) 50vw, 600px"
  alt="Descriptive alt text"
  loading="lazy"
>
```

### Art Direction with `<picture>`

```html
<picture>
  <source media="(min-width: 1024px)" srcset="hero-wide.webp">
  <source media="(min-width: 640px)" srcset="hero-medium.webp">
  <img src="hero-narrow.webp" alt="Product dashboard on laptop" loading="eager">
</picture>
```

### Base Rule

```css
img, video, svg {
  max-width: 100%;
  height: auto;
  display: block;
}
```

---

## Testing Checklist

Test every layout at these viewport widths:

| Width   | Device                  | Check                                          |
|---------|-------------------------|-------------------------------------------------|
| 375px   | iPhone SE               | Touch targets, text wrapping, nav menu         |
| 390px   | iPhone 14 / 15          | Standard mobile baseline                        |
| 428px   | iPhone 14 Plus / Pro Max| Large phone                                     |
| 768px   | iPad portrait           | 2-column layouts, sidebar visibility            |
| 1024px  | iPad landscape          | Transition point for desktop layouts             |
| 1280px  | Laptop                  | Full layout, max content width                  |
| 1440px  | Large desktop           | Content centering, whitespace balance           |
| 1920px  | Wide monitor            | Max-width constraints working, no over-stretch  |

**Quick test:** Browser DevTools responsive mode with device emulation. Check both orientations for tablets.

---

## Quick Reference: Media Query Pattern

```css
/* Base: mobile (0-639px) */
.component { /* mobile styles */ }

/* sm: 640px+ — large phones, small tablets */
@media (min-width: 640px) {
  .component { /* layout starts adapting */ }
}

/* md: 768px+ — tablets */
@media (min-width: 768px) {
  .component { /* two-column, expanded nav */ }
}

/* lg: 1024px+ — small laptops, tablet landscape */
@media (min-width: 1024px) {
  .component { /* full layout active */ }
}

/* xl: 1280px+ — desktop */
@media (min-width: 1280px) {
  .component { /* max-width, spacious */ }
}
```

---

## Internationalization and RTL

### RTL Layout Support

For right-to-left languages (Arabic, Hebrew, Urdu, Farsi), the design system must support bidirectional layout.

**CSS Logical Properties (mandatory for RTL support):**

Replace physical properties with logical equivalents:

| Physical | Logical | Purpose |
|----------|---------|---------|
| margin-left | margin-inline-start | Inline start margin |
| margin-right | margin-inline-end | Inline end margin |
| padding-left | padding-inline-start | Inline start padding |
| padding-right | padding-inline-end | Inline end padding |
| border-left | border-inline-start | Inline start border |
| border-right | border-inline-end | Inline end border |
| left | inset-inline-start | Inline start position |
| right | inset-inline-end | Inline end position |
| width | inline-size | Inline dimension |
| text-align: left | text-align: start | Inline text alignment |
| float: left | float: inline-start | Inline float |

**RTL CSS Implementation:**
```css
/* Add to root when RTL is active */
[dir="rtl"] {
  --direction-factor: -1;
}

/* Use logical properties in all component CSS */
.button {
  padding-inline-start: var(--space-3);
  padding-inline-end: var(--space-4);
  text-align: start;
}

.nav-icon {
  margin-inline-end: var(--space-2);
  /* Icon position flips automatically in RTL */
}
```

**Layout Flipping Rules:**
- Sidebar navigation: `left` → `inline-start` (moves to right in RTL)
- Text alignment: use `start` not `left`
- Icons with directional meaning (arrows, chevrons): flip with `transform: scaleX(-1)` in RTL
- Grid layouts: use `direction: rtl` on the grid container to reverse column order
- Flexbox: use `flex-direction: row` (respects `dir` attribute automatically)

**Component Considerations for RTL:**
| Component | RTL Adjustment |
|-----------|---------------|
| Breadcrumbs | Separator flips (› becomes ‹) or use "/" (universal) |
| Navigation | Menu items reverse order |
| Forms | Labels flip to right, inputs to left |
| Modals | Close button moves to top-left |
| Tooltips | Arrow position mirrors |
| Pagination | "Next" arrow flips direction |
| Tables | Columns reverse order (if not data tables) |
| Progress bars | Fill direction reverses |

### Text Expansion in Translation

Translated text expands or contracts relative to English. Agents must account for this in layout:

| Language | Expansion Factor | Common Languages |
|----------|-----------------|------------------|
| English (baseline) | 1.0x | en |
| Romance languages | 1.15-1.25x | fr, es, it, pt |
| Germanic languages | 1.2-1.35x | de, nl |
| Nordic languages | 1.1-1.2x | sv, no, da |
| Slavic languages | 1.15-1.3x | ru, pl, cs |
| CJK languages | 0.8-0.9x | zh, ja, ko |
| Arabic/Hebrew | 1.2-1.4x | ar, he |

**Layout rules for translation-ready design:**
- Button widths: use `min-width` not fixed `width`; allow text to expand
- Navigation items: use `flex-wrap: wrap` or hamburger overflow for translated menus
- Card titles: use `line-clamp` with generous line count (3-4 lines minimum)
- Form labels: allow wrapping; don't use fixed widths
- Modal/dialog: use `max-width` with percentage, not fixed pixel width
- Never truncate text with `text-overflow: ellipsis` on translated content unless explicitly requested

### Character Set and Font Considerations

- CJK languages need larger line-height (1.7-2.0 vs 1.5 for Latin)
- Arabic requires larger font-size (16px minimum body text vs 14px for Latin)
- Ensure fallback font stack includes CJK and Arabic ranges:
```css
--font-body: 'Inter', 'Noto Sans', 'Noto Sans Arabic', 'Noto Sans CJK', system-ui, sans-serif;
```
- Test all components with: English, German (long words), Arabic (RTL), Chinese (CJK), Thai (no word spacing)
