# Layout Compositions

Page-level layout patterns and section templates. These bridge the spacing system
(see `visual_system.md`) and individual components (see `component_patterns.md`).

## Design Principles

1. **Max-width constrains content; sections span full-width:** Use `max-width` on inner containers for readability, while outer sections fill the viewport to enable background variation.
2. **Pick one shell, then compose sections within it:** Choose the page-level shell (SaaS, marketing, documentation, split) first, then stack section layouts (hero, features, testimonials) inside it.
3. **Use `auto-fit` with `minmax()` for intrinsic grids:** Let CSS Grid determine column count from available space rather than hard-coding breakpoints for every card layout.
4. **Alternate density and background between sections:** Prevent visual fatigue by alternating sparse/dense and light/alt-background treatments as the user scrolls.

---

## Base Section Class

```css
.section {
  padding-block: var(--space-16);
  padding-inline: var(--space-6);
  max-width: var(--max-width, 1280px);
  margin-inline: auto;
}
.section--compact  { padding-block: var(--space-10); }
.section--spacious { padding-block: var(--space-20); }
```

Set `--max-width` per project. Common values: 1152px, 1280px, 1440px.

---

## Page-Level Shells

### 1. SaaS Shell
Sidebar + top bar + scrollable content area.
```
+--------+--------------------------------+
| SIDEBAR| TOPBAR                         |
| 240px  +--------------------------------+
|        | CONTENT (scroll)               |
+--------+--------------------------------+
```
```css
.shell {
  display: grid;
  grid-template-columns: 240px 1fr;
  grid-template-rows: 56px 1fr;
  grid-template-areas: "sidebar topbar" "sidebar content";
  height: 100dvh;
}
```
**When:** Dashboards, admin panels, IDEs, management tools.
**Responsive:** Icon rail at `md`, hamburger off-canvas at `sm`.

### 2. Marketing Flow
Full-width sections stacked vertically, alternating backgrounds.
```css
.flow > section:nth-child(even) { background: var(--bg-alt); }
.flow > section:nth-child(odd)  { background: var(--bg); }
```
**When:** Landing pages, product pages, campaign pages.
**Responsive:** Sections stay full-width. Internal grids collapse per section pattern.

### 3. Documentation
Fixed sidebar nav + main content + optional right TOC.
```css
.docs {
  display: grid;
  grid-template-columns: 240px minmax(0, 1fr) 200px;
  gap: var(--space-6);
  max-width: 1440px;
  margin-inline: auto;
}
.docs__toc { position: sticky; top: var(--space-8); align-self: start; }
```
**When:** API docs, knowledge bases, handbooks.
**Responsive:** Sidebars become drawers at `md`. TOC drops below at `sm`.

### 4. Split View
50/50 or biased two-panel layout.
```css
.split { display: grid; grid-template-columns: 1fr 1fr; gap: var(--space-8); }
.split--biased  { grid-template-columns: 2fr 3fr; }
.split--sidebar { grid-template-columns: 340px 1fr; }
```
**When:** Settings, comparisons, master-detail, before/after.
**Responsive:** Stacks at `md`. Tab switching at `sm`.

---

## Section Layouts

### 5. Hero Centered
Centered text + CTA, full-width background.
```css
.hero-centered {
  text-align: center;
  display: flex; flex-direction: column; align-items: center;
  gap: var(--space-6);
  padding-block: var(--space-20) var(--space-16);
}
.hero-centered h1 { max-width: 15ch; }
.hero-centered p  { max-width: 48ch; color: var(--fg-muted); }
```
**When:** Primary landing hero, product announcements, simple value props.

### 6. Hero Split
Text left, visual right (or reversed via `--reverse`).
```css
.hero-split {
  display: grid; grid-template-columns: 1fr 1fr;
  gap: var(--space-10); align-items: center;
}
.hero-split--reverse > :first-child { order: 2; }
.hero-split--reverse > :last-child  { order: 1; }
```
**When:** Product pages, app showcases, features with screenshots.

### 7. Feature Grid
3-column auto-fit cards. Intrinsic responsiveness via `minmax()`.
```css
.feature-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: var(--space-6);
}
```
**When:** Feature lists, service offerings, team grids, resource cards.

### 8. Feature Bento
Asymmetric grid. Apple-style with mixed card sizes (2x2 + 1x1).
```css
.bento {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: var(--space-4);
}
.bento > :nth-child(1) { grid-column: span 2; grid-row: span 2; }
.bento > :nth-child(4) { grid-column: span 1; }
.bento > :nth-child(5) { grid-column: span 3; }
```
**When:** Product showcases, platform overviews, app highlights.
**Responsive:** Collapse to single column at `sm`.

### 9. Logo Wall
Trust bar, grayscale logos in flex-wrap row.
```css
.logo-wall { display: flex; flex-wrap: wrap; justify-content: center; align-items: center; gap: var(--space-8); }
.logo-wall img {
  height: 32px; opacity: 0.4; filter: grayscale(1);
  transition: all var(--dur-base) var(--ease);
}
.logo-wall img:hover { opacity: 1; filter: grayscale(0); }
```
**When:** Social proof, partner logos, press mentions, trusted-by sections.

### 10. Metrics Band
4-column stats with dividers.
```css
.metrics { display: grid; grid-template-columns: repeat(4, 1fr); text-align: center; }
.metrics > * + * { border-inline-start: 1px solid var(--border); }
.metrics strong { font-size: var(--fs-display-sm); color: var(--accent); }
```
**When:** SaaS credibility, impact numbers, performance claims.
**Responsive:** 2 columns at `md`, 1 column at `sm`. Remove dividers on stack.

### 11. Testimonial Row
3 cards in a row or carousel.
```css
.testimonials {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: var(--space-6);
}
```
**When:** Social proof, customer stories, case study previews.

### 12. Pricing Columns
3 tiers, center highlighted.
```css
.pricing { display: grid; grid-template-columns: repeat(3, 1fr); gap: var(--space-4); align-items: start; }
.pricing__card--featured {
  border: 2px solid var(--accent);
  transform: scale(1.04); z-index: 1;
  box-shadow: var(--shadow-lg);
}
```
**When:** SaaS pricing, plan comparisons, tier selection.
**Responsive:** Stack at `md`. Featured card uses border only (no scale).

### 13. CTA Banner
Full-width with gradient or accent background.
```css
.cta-banner {
  background: var(--accent); color: var(--on-accent);
  text-align: center;
  padding-block: var(--space-12); padding-inline: var(--space-6);
}
.cta-banner--gradient { background: linear-gradient(135deg, var(--accent), var(--accent-alt)); }
```
**When:** Final conversion moment, section transitions, newsletter signup.

### 14. FAQ Accordion
Stacked expandable items.
```css
.faq-list { display: flex; flex-direction: column; max-width: 72ch; margin-inline: auto; }
.faq-item + .faq-item { border-top: 1px solid var(--border); }
.faq-item__answer {
  display: grid; grid-template-rows: 0fr;
  transition: grid-template-rows var(--dur-base) var(--ease);
}
.faq-item[open] .faq-item__answer { grid-template-rows: 1fr; }
.faq-item__answer > div { overflow: hidden; }
```
**When:** Pricing FAQs, product questions, support sections.

### 15. Timeline / Steps
Horizontal (desktop) or vertical (mobile) step indicators.
```css
.steps { display: flex; justify-content: space-between; position: relative; }
.steps::before {
  content: ""; position: absolute; top: 50%; inset-inline: 0;
  height: 2px; background: var(--border);
}
.step { display: flex; flex-direction: column; align-items: center; gap: var(--space-2); z-index: 1; }
.step__marker {
  width: 40px; height: 40px; border-radius: var(--radius-full);
  background: var(--bg); border: 2px solid var(--accent);
  display: grid; place-items: center;
}
```
**When:** Onboarding flows, how-it-works, process explanations, roadmaps.

---

## Responsive Behavior Summary

See `responsive_system.md` for breakpoint definitions and the complete grid behavior per breakpoint table.

Layout-specific stacking notes (unique to each composition):

| Layout          | Key Responsive Behavior                                       |
|-----------------|---------------------------------------------------------------|
| SaaS Shell      | Hamburger at `sm`, icon rail at `md`, full sidebar at `lg+`  |
| Marketing Flow  | Always full-width stack; internal grids collapse per section  |
| Documentation   | 1-col+drawer at `sm`, 2-col at `md`, 3-col at `lg`           |
| Split View      | Tab switch at `sm`, stacked at `md`, side-by-side at `lg`    |
| Hero Split      | Stacked at `sm`, side-by-side at `md+`                        |
| Feature Grid    | Auto-fit via `minmax(280px, 1fr)` — columns adapt naturally   |
| Bento           | 1-col at `sm`, 2-col at `md`, 4-col at `lg`                  |
| Pricing         | Stack at `sm`, 3-col compact at `md`, 3-col full at `lg`     |
| Timeline        | Vertical at `sm`, horizontal compact at `md`, full at `lg`   |

---

## Container Queries Alternative

See `responsive_system.md` for the complete container query guidelines (when to use, when not to use, and CSS patterns).

```css
.card-container { container-type: inline-size; container-name: card; }
@container card (min-width: 400px) { .card { flex-direction: row; } }
@container card (min-width: 600px) { .card__details { display: grid; grid-template-columns: 1fr 1fr; } }
```

---

## Grid Pattern Reference

```css
--grid-cards:   repeat(auto-fit, minmax(280px, 1fr)); /* auto columns */
--grid-sidebar: 260px minmax(0, 1fr);                /* sidebar + content */
--grid-split:   1fr 1fr;                             /* equal split */
--grid-content: 2fr 1fr;                             /* biased split */
--grid-metrics: repeat(4, 1fr);                      /* 4-col stats */
--grid-bento:   repeat(4, 1fr);                      /* bento base */
```
