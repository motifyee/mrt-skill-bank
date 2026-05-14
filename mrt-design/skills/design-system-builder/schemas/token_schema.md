# Token Schema

CSS variable architecture, naming conventions, and DTCG interoperability.
Every design system output must follow this schema.

---

## Canonical Token Dictionary

> **Authoritative reference.** This section is the single source of truth for every token name, value, and alias in the design system. All agent-generated files must use the names and values defined here. This section overrides any conflicting values in the Full CSS Variable Template below, in foundation files (e.g. `motion_choreography.md`), or elsewhere. When in doubt, this table wins.

### Color Tokens — Raw Palette

| CSS Variable | Category | Purpose | Allowed Aliases |
|---|---|---|---|
| `--neutral-900` | neutral | Darkest surface / primary text | `--ink` |
| `--neutral-800` | neutral | Dark surface variant / secondary dark | `--ink-2` |
| `--neutral-700` | neutral | Medium-dark surface / muted dark text | — |
| `--neutral-600` | neutral | Dark muted / border-dark | — |
| `--neutral-500` | neutral | Mid gray / disabled text | — |
| `--neutral-400` | neutral | Light muted text | — |
| `--neutral-300` | neutral | Subtle borders / dividers | `--line-color` |
| `--neutral-200` | neutral | Light background alt | — |
| `--neutral-100` | neutral | Light background / surface tint | `--paper-2` |
| `--neutral-50` | neutral | Lightest surface / page background | `--paper` |
| `--accent-500` | accent | Primary brand / action color | `--flame` |
| `--accent-400` | accent | Hover state | `--flame-hover` |
| `--accent-300` | accent | Pressed state | `--flame-press` |
| `--accent-200` | accent | Tinted / transparent accent | `--flame-tint` |
| `--accent-600` | accent | Deep / darker accent | `--flame-deep` |
| `--success-raw` | semantic | Success state color | — |
| `--success-tint-raw` | semantic | Success background tint | — |
| `--warning-raw` | semantic | Warning state color | — |
| `--warning-tint-raw` | semantic | Warning background tint | — |
| `--error-raw` | semantic | Error state color | — |
| `--error-tint-raw` | semantic | Error background tint | — |
| `--info-raw` | semantic | Info state color | — |
| `--info-tint-raw` | semantic | Info background tint | — |

### Color Tokens — Semantic Mapping

| CSS Variable | Purpose | Default Resolution (Light) |
|---|---|---|
| `--bg` | Page background | `var(--neutral-50)` |
| `--bg-alt` | Alternate surface | `var(--neutral-100)` |
| `--bg-inset` | Inset / nested surface | `var(--neutral-100)` |
| `--bg-inverse` | Inverted surface (dark) | `var(--neutral-900)` |
| `--bg-inverse-alt` | Inverted surface variant | `var(--neutral-800)` |
| `--fg` | Primary text | `var(--neutral-900)` |
| `--fg-muted` | Secondary text | Project-specific |
| `--fg-subtle` | Tertiary / placeholder text | Project-specific |
| `--accent` | Primary action color | `var(--accent-500)` |
| `--accent-hover` | Action hover | `var(--accent-400)` |
| `--accent-press` | Action pressed | `var(--accent-300)` |
| `--accent-tint` | Accent background tint | `var(--accent-200)` |
| `--on-accent` | Text on accent background | `#FFFFFF` |
| `--border` | Default border | `var(--neutral-300)` |
| `--border-strong` | Emphasized border | Project-specific |
| `--focus-ring` | Keyboard focus indicator | `var(--accent)` |
| `--success` | Success semantic | `var(--success-raw)` |
| `--success-tint` | Success tint semantic | `var(--success-tint-raw)` |
| `--warning` | Warning semantic | `var(--warning-raw)` |
| `--warning-tint` | Warning tint semantic | `var(--warning-tint-raw)` |
| `--error` | Error semantic | `var(--error-raw)` |
| `--error-tint` | Error tint semantic | `var(--error-tint-raw)` |
| `--info` | Info semantic | `var(--info-raw)` |
| `--info-tint` | Info tint semantic | `var(--info-tint-raw)` |

### Typography Tokens

| CSS Variable | Category | Purpose | Typical Range |
|---|---|---|---|
| `--font-display` | family | Headline / display font stack | Custom font, fallbacks |
| `--font-body` | family | Body text font stack | Custom font, fallbacks |
| `--font-mono` | family | Code / monospace font stack | Monospace, fallbacks |
| `--fs-display` | scale | Display / hero size | 72-96px |
| `--fs-h1` | scale | Heading 1 | 48-64px |
| `--fs-h2` | scale | Heading 2 | 32-48px |
| `--fs-h3` | scale | Heading 3 | 24-32px |
| `--fs-h4` | scale | Heading 4 | 18-24px |
| `--fs-body-lg` | scale | Large body text | 18px |
| `--fs-body` | scale | Default body | 16px |
| `--fs-body-sm` | scale | Small body / caption | 14px |
| `--fs-label` | scale | Label / overline | 12px |
| `--fs-mono` | scale | Monospace / code | 14px |
| `--lh-display` | metrics | Display line-height | 1.05 |
| `--lh-heading` | metrics | Heading line-height | 1.15 |
| `--lh-body` | metrics | Body line-height | 1.6 |
| `--lh-body-sm` | metrics | Small body line-height | 1.55 |
| `--lh-label` | metrics | Label line-height | 1.3 |
| `--tracking-display` | metrics | Display letter-spacing | -0.02em |
| `--tracking-heading` | metrics | Heading letter-spacing | -0.01em |
| `--tracking-body` | metrics | Body letter-spacing | 0 |
| `--tracking-label` | metrics | Label letter-spacing | 0.08em |
| `--fw-regular` | weight | Regular weight | 400 |
| `--fw-medium` | weight | Medium weight | 500 |
| `--fw-semibold` | weight | Semibold weight | 600 |
| `--fw-bold` | weight | Bold weight | 700 |

### Spacing Tokens

| CSS Variable | Purpose | Typical Base Value |
|---|---|---|
| `--space-1` | Micro gap (icon padding) | 4-8px |
| `--space-2` | Tight gap (inline spacing) | 8-16px |
| `--space-3` | Small gap (component internal) | 12-24px |
| `--space-4` | Base gap (default padding) | 16px |
| `--space-5` | Medium gap (card padding) | 24px |
| `--space-6` | Large gap (section internal) | 32px |
| `--space-7` | XL gap (section spacing) | 48px |
| `--space-8` | 2XL gap (major sections) | 64px |
| `--space-9` | 3XL gap (page sections) | 96px |
| `--space-10` | 4XL gap (hero spacing) | 128px |

### Shadow Tokens

| CSS Variable | Mode | Purpose | Opacity Range |
|---|---|---|---|
| `--shadow-sm` | light | Subtle lift (cards, inputs) | 0.04-0.06 |
| `--shadow-md` | light | Medium elevation (dropdowns, popovers) | 0.08-0.12 |
| `--shadow-lg` | light | High elevation (modals, panels) | 0.12-0.16 |
| `--shadow-sm` | dark | Subtle lift on dark surfaces | 0.20-0.30 |
| `--shadow-md` | dark | Medium elevation on dark surfaces | 0.30-0.40 |
| `--shadow-lg` | dark | High elevation on dark surfaces | 0.40-0.50 |
| `--shadow-accent` | both | Accent-colored glow for CTAs | Uses `color-mix` |

> Shadow tokens use `var(--shadow-color)` for the color component where supported, falling back to `rgba(0,0,0,...)` with the opacity ranges above.

### Motion / Duration Tokens

| CSS Variable | Canonical Value | Purpose |
|---|---|---|
| `--dur-micro` | `100ms` | Tooltip fade, micro-state change, ripple |
| `--dur-fast` | `200ms` | Hover, focus feedback, small reveal |
| `--dur-base` | `300ms` | Standard transitions, element entrances |
| `--dur-slow` | `450ms` | Section transitions, complex animations |
| `--ease` | `cubic-bezier(0.2, 0.8, 0.2, 1)` | Standard easing for all transitions |

> **Canonical defaults.** These are the default duration values. If a foundation file specifies a different duration (e.g. 120ms, 320ms), this table overrides it. **Exception:** aesthetic presets may override specific motion tokens when documented in the context packet under `motion.overrides`. Playful aesthetics may use 60-80ms snaps for micro-feedback; brutalist aesthetics may use 0ms instant changes; luxury aesthetics may use 600ms+ slow transitions for deliberate, premium-feeling motion.

### Radius Tokens

| CSS Variable | Canonical Value | Purpose |
|---|---|---|
| `--radius-sm` | `4px` | Small elements (tags, chips, badges) |
| `--radius-md` | `8px` | Buttons, inputs, cards |
| `--radius-lg` | `12px` | Large cards, modals, panels |
| `--radius-xl` | `20px` | Feature cards, hero sections |
| `--radius-full` | `999px` | Pills, avatars, circular elements |

### Density Tokens

| CSS Variable | Values | Purpose |
|---|---|---|
| `--density-scale` | `1` (comfortable), `0.75` (compact), `1.25` (spacious) | Multiplier for all spacing-derived values |
| `--touch-target` | `44px` (comfortable), `36px` (compact), `52px` (spacious) | Minimum interactive area size |

> **Clarification:** `density_system.spacing_multiplier` in the context packet IS the value assigned to `--density-scale` in CSS. They are the same multiplier -- not two separate values. In generated CSS, assign the packet value directly to `--density-scale`, then derive component padding, row height, and section rhythm from that token. Do not multiply spacing by both values.

### Z-Index Tokens

Required when the generated system includes any overlay components (dropdowns, modals, toasts, tooltips, sticky headers).

| CSS Variable | Canonical Value | Purpose |
|---|---|---|
| `--z-base` | `0` | Default stacking context (normal flow) |
| `--z-dropdown` | `100` | Dropdowns, popovers, select menus |
| `--z-sticky` | `200` | Sticky headers, side panels |
| `--z-modal` | `300` | Modals, dialogs, drawers |
| `--z-toast` | `400` | Toast notifications, snackbars |
| `--z-tooltip` | `500` | Tooltips (must always be on top) |

> When a generated component needs a z-index, it MUST reference one of these tokens rather than a hardcoded integer. This prevents stacking conflicts across UI kit surfaces.

### Secondary Accent Tokens (Optional)

For systems with two distinct accent colors (e.g., primary brand action + a secondary category color, or dual-brand platforms). Use only when the context packet documents a second brand color or the aesthetic origin requires it.

| CSS Variable | Category | Purpose |
|---|---|---|
| `--accent-alt-500` | accent-alt | Secondary brand action color | 
| `--accent-alt-400` | accent-alt | Secondary hover state |
| `--accent-alt-300` | accent-alt | Secondary pressed state |
| `--accent-alt-200` | accent-alt | Secondary tinted background |
| `--accent-alt-600` | accent-alt | Secondary deep / text-on-light |
| `--on-accent-alt` | accent-alt | Text on secondary accent background |

Secondary accent semantic tokens (`--accent-alt`, `--accent-alt-hover`, `--accent-alt-press`, `--accent-alt-tint`) follow the same three-layer pattern as the primary accent. Generate these only when `colors.accent_secondary` is populated in the context packet.

### Chart / Data Visualization Tokens (Optional)

For systems with charts, graphs, or data-dense surfaces. Generate these when the context packet includes a dashboard or analytics surface. Values are derived from the accent palette or set explicitly in `foundations/data_visualization.md`.

| CSS Variable | Category | Purpose |
|---|---|---|
| `--seq-1` through `--seq-7` | sequential | Sequential palette for ordered data (gradient steps) |
| `--cat-1` through `--cat-8` | categorical | Distinct hues for category comparison |
| `--sem-positive` | semantic | Positive trend (typically green family) |
| `--sem-negative` | semantic | Negative trend (typically red family) |
| `--sem-neutral` | semantic | Flat / no-change (typically neutral family) |
| `--sem-warning` | semantic | Attention / caution (typically amber family) |

These tokens are component-specific to data visualization. They are exempt from the canonical semantic token layer (they live in the raw palette layer directly) but must be defined in `tokens.css` alongside other raw tokens when any chart component is generated.

### Container Width Tokens

| CSS Variable | Canonical Value | Purpose |
|---|---|---|
| `--container-sm` | `640px` | Narrow reading columns, forms, dialogs |
| `--container-md` | `960px` | Medium content areas, documentation |
| `--container-lg` | `1280px` | Standard page max-width |
| `--container-max` | `1440px` | Full-width marketing pages |
| `--nav-h` | `64–72px` | Standard navigation height |
| `--nav-h-sm` | `52–60px` | Compact navigation (mobile / compact density) |

> Use `--container-max` for the outermost wrapper. Use `--container-lg` for content sections inside. Use `--container-md` for documentation reading columns and settings panels. Use `--container-sm` for forms, confirmation dialogs, and narrow utility panels.

---

## CSS Variable Architecture

### Three-layer system

```
Raw palette               Semantic mapping         Component binding
--neutral-900: #141414    --bg: var(--neutral-50)  --btn-bg: var(--accent)
--neutral-50: #FFFFFF     --fg: var(--neutral-900) --btn-fg: var(--on-accent)
--accent-500: #FF6B00     --accent: var(--accent)  --card-bg: var(--bg-alt)
```

### Naming convention

> **Canonical authority:** All agent-generated files must use the names and values in the Canonical Token Dictionary (top of this file). When in doubt, this file wins over foundation files.

**Raw tokens** default to systematic names, with optional evocative aliases:

```css
/* Default: systematic */
--neutral-900: #141414;
--neutral-800: #1F1F1F;
--neutral-50: #FFFFFF;
--neutral-100: #F5F2EE;
--accent-500: #FF6B00;
--accent-400: #FF8124;

/* Optional: evocative aliases for brand personality */
--ink: var(--neutral-900);
--paper: var(--neutral-50);
--flame: var(--accent-500);
```

**Tradeoff:** Systematic names are more maintainable and self-documenting (`--neutral-900`
tells you the color's role and weight). Evocative names add brand personality but can
confuse new team members. Default to systematic; add evocative aliases only when the
team explicitly wants them.

**Semantic tokens** use role-based names:
```css
--bg: var(--paper);
--bg-alt: var(--paper-2);
--bg-inverse: var(--ink);
--fg: var(--ink);
--fg-muted: #6B6B6B;
--fg-subtle: #9A9A9A;
--accent: var(--flame);
--accent-hover: var(--flame-hover);
--accent-press: var(--flame-press);
--accent-tint: var(--flame-tint);
--on-accent: #FFFFFF;
--border: var(--line);
--focus-ring: var(--flame);
```

**Component tokens** (optional, for complex systems):
```css
--btn-primary-bg: var(--accent);
--btn-primary-fg: var(--on-accent);
--input-border: var(--border);
--input-focus-ring: var(--accent);
```

---

## Density Tiers

Design systems may define three density tiers that adjust spacing and sizing globally:

```css
/* Density tier — applied as data attribute: <html data-density="compact"> */
/* Default is "comfortable" — no data attribute needed */

:root {
  /* Base density (comfortable) */
  --density-scale: 1;
  --touch-target: 44px;
}

[data-density="compact"] {
  --density-scale: 0.75;
  --touch-target: 36px;
}

[data-density="spacious"] {
  --density-scale: 1.25;
  --touch-target: 52px;
}
```

Compact: data-heavy tools, dashboards, dev tools. Comfortable: SaaS, marketing, docs. Spacious: editorial, luxury, brand sites.

Include `density_system` in the context packet when the product's content density
is known. Agents use it to adjust spacing multiplier, component padding, row
height, section rhythm, and touch targets.

`density_system.spacing_multiplier` is the source value for CSS `--density-scale`.
Do not multiply spacing by both values. In generated CSS, assign the packet value
directly to `--density-scale`, then derive component padding from that token.

---

## RTL-Aware Tokens

For bidirectional layouts, provide logical-direction tokens that resolve differently in LTR vs RTL:

```css
:root {
  --space-inline-start: var(--space-4);
  --space-inline-end: var(--space-4);
  --space-block-start: var(--space-2);
  --space-block-end: var(--space-2);
  --border-inline-start: 1px solid var(--border);
  --border-inline-end: none;
  --radius-inline-start: var(--radius-md);
  --radius-inline-end: var(--radius-md);
}

[dir="rtl"] {
  --space-inline-start: var(--space-4); /* same values, but CSS logical props handle the flip */
  --radius-inline-start: var(--radius-md);
  --radius-inline-end: var(--radius-md);
}
```

Use CSS logical properties (`margin-inline-start`, `padding-block-end`, `border-inline-start`)
in component CSS rather than physical directions (`margin-left`, `padding-top`).

Include `reading_direction` in the context packet (`"ltr"` or `"rtl"`) when the product
serves RTL locales. Agents add the `[dir="rtl"]` override block and use logical properties.

---

## Full CSS Variable Template

Font loading is handled by HTML `<link>` tags, bundled `@font-face`, or
system-fonts-only mode according to project constraints. Do not use CSS
`@import` in `tokens.css`; it creates a render-blocking waterfall and conflicts
with performance guidance.

```css
/* ==========================================================================
   [Brand] Design System — tokens.css
   Base tokens + semantic tokens. Import this before any component CSS.
   ========================================================================== */

:root {
  /* ---------- Color — raw palette ---------- */
  --neutral-900:           #[value];   /* darkest neutral / primary text */
  --neutral-800:           #[value];   /* dark surface variant */
  --neutral-700:           #[value];   /* medium-dark surface */
  --neutral-600:           #[value];   /* dark muted / border-dark */
  --neutral-500:           #[value];   /* mid gray / disabled text */
  --neutral-400:           #[value];   /* light muted text */
  --neutral-300:           #[value];   /* subtle borders / dividers */
  --neutral-200:           #[value];   /* light background alt */
  --neutral-100:           #[value];   /* light surface tint */
  --neutral-50:            #[value];   /* lightest surface / page bg */
  --accent-500:            #[value];   /* primary brand / action color */
  --accent-400:            #[value];   /* hover state */
  --accent-300:            #[value];   /* pressed state */
  --accent-200:            #[value];   /* tinted / transparent accent */
  --accent-600:            #[value];   /* deep / darker accent */
  --success-raw:           #16A34A;
  --success-tint-raw:      #DCFCE7;
  --warning-raw:           #F59E0B;
  --warning-tint-raw:      #FEF3C7;
  --error-raw:             #DC2626;
  --error-tint-raw:        #FEE2E2;
  --info-raw:              #0EA5E9;
  --info-tint-raw:         #F0F9FF;

  /* ---------- Color — semantic ---------- */
  --bg:                    var(--neutral-50);
  --bg-alt:                var(--neutral-100);
  --bg-inset:              var(--neutral-100);
  --bg-inverse:            var(--neutral-900);
  --bg-inverse-alt:        var(--neutral-800);
  --fg:                    var(--neutral-900);
  --fg-muted:              #[value];
  --fg-subtle:             #[value];
  --on-accent:             #FFFFFF;
  --accent:                var(--accent-500);
  --accent-hover:          var(--accent-400);
  --accent-press:          var(--accent-300);
  --accent-tint:           var(--accent-200);
  --border:                var(--neutral-300);
  --border-strong:         #[value];
  --focus-ring:            var(--accent);
  --success:               var(--success-raw);
  --success-tint:          var(--success-tint-raw);
  --warning:               var(--warning-raw);
  --warning-tint:          var(--warning-tint-raw);
  --error:                 var(--error-raw);
  --error-tint:            var(--error-tint-raw);
  --info:                  var(--info-raw);
  --info-tint:             var(--info-tint-raw);

  /* ---------- Type — families ---------- */
  --font-display: '[Display Font]', fallbacks;
  --font-body:    '[Body Font]', fallbacks;
  --font-mono:    '[Mono Font]', fallbacks;

  /* ---------- Type — scale ---------- */
  --fs-display: [72-96]px;
  --fs-h1:      [48-64]px;
  --fs-h2:      [32-48]px;
  --fs-h3:      [24-32]px;
  --fs-h4:      [18-24]px;
  --fs-body-lg: 18px;
  --fs-body:    16px;
  --fs-body-sm: 14px;
  --fs-label:   12px;
  --fs-mono:    14px;

  /* ---------- Type — metrics ---------- */
  --lh-display:    1.05;
  --lh-heading:    1.15;
  --lh-body:       1.6;
  --lh-body-sm:    1.55;
  --lh-label:      1.3;
  --tracking-display: -0.02em;
  --tracking-heading: -0.01em;
  --tracking-body:     0;
  --tracking-label:    0.08em;
  --fw-regular:  400;
  --fw-medium:   500;
  --fw-semibold: 600;
  --fw-bold:     700;

  /* ---------- Spacing ([4-8]px base) ---------- */
  --space-1:   [4-8]px;
  --space-2:   [8-16]px;
  --space-3:   [12-24]px;
  --space-4:   16px;
  --space-5:   24px;
  --space-6:   32px;
  --space-7:   48px;
  --space-8:   64px;
  --space-9:   96px;
  --space-10:  128px;

  /* ---------- Radii ---------- */
  --radius-sm:   4px;
  --radius-md:   8px;
  --radius-lg:   12px;
  --radius-xl:   20px;
  --radius-full: 999px;

  /* ---------- Shadows ---------- */
  --shadow-color: rgba(0,0,0,0.05);                          /* tune per brand */
  --shadow-sm:  0 1px 2px var(--shadow-color);               /* opacity 0.04-0.06 */
  --shadow-md:  0 4px 12px var(--shadow-color);              /* opacity 0.08-0.12 */
  --shadow-lg:  0 12px 32px var(--shadow-color);             /* opacity 0.12-0.16 */
  --shadow-accent: 0 8px 24px color-mix(in srgb, var(--accent) 24%, transparent);

  /* ---------- Motion ---------- */
  --ease: cubic-bezier(0.2, 0.8, 0.2, 1);
  --dur-micro: 100ms;
  --dur-fast: 200ms;
  --dur-base: 300ms;
  --dur-slow: 450ms;

  /* ---------- Layout ---------- */
  --container-sm:  640px;
  --container-md:  960px;
  --container-lg:  1280px;
  --container-max: 1440px;    /* override per brand; range 1200-1440px */
  --nav-h:   72px;
  --nav-h-sm: 60px;

  /* ---------- Z-Index ---------- */
  /* Required when system includes dropdowns, modals, toasts, or tooltips */
  --z-base:     0;
  --z-dropdown: 100;
  --z-sticky:   200;
  --z-modal:    300;
  --z-toast:    400;
  --z-tooltip:  500;
}

[data-theme="dark"] {
  /* Remap every semantic color token for dark surfaces. Raw tokens do not change. */
  --bg:                    var(--neutral-900);
  --bg-alt:                var(--neutral-800);
  --bg-inset:              var(--neutral-700);
  --bg-inverse:            var(--neutral-50);
  --fg:                    var(--neutral-50);
  --fg-muted:              var(--neutral-400);
  --accent:                var(--accent-500);
  --accent-hover:          var(--accent-400);
  --accent-press:          var(--accent-300);
  --accent-tint:           color-mix(in srgb, var(--accent) 16%, transparent);
  --on-accent:             var(--neutral-900);
  --border:                rgba(255,255,255,0.10);
  --border-strong:         rgba(255,255,255,0.20);

  /* Shadows — elevated opacity for dark surfaces */
  --shadow-color: rgba(0,0,0,0.35);                           /* opacity 0.30-0.40 midpoint */
  --shadow-sm:             0 1px 4px var(--shadow-color);      /* opacity 0.20-0.30 */
  --shadow-md:             0 4px 16px var(--shadow-color);     /* opacity 0.30-0.40 */
  --shadow-lg:             0 12px 40px var(--shadow-color);    /* opacity 0.40-0.50 */
}

/* ---- Dark Mode Fallback ----
   ONLY accepted pattern: [data-theme="dark"] is primary selector.
   @media (prefers-color-scheme: dark) with :root:not([data-theme]) is the
   automatic fallback for users who have not explicitly toggled a theme.
   No other dark-mode selector pattern is permitted. */
@media (prefers-color-scheme: dark) {
  :root:not([data-theme]) {
    /* Duplicate the [data-theme="dark"] semantic remap here. */
  }
}
```

---

## Canonical Token Values — Quick Reference

> The complete Canonical Token Dictionary with all tokens, aliases, and purposes is at the top of this file.
> The tables below provide the canonical motion and shadow values for quick look-up.

### Motion & Duration

| Token Name | Canonical Value | Purpose |
|---|---|---|
| `--ease` | `cubic-bezier(0.2, 0.8, 0.2, 1)` | Standard easing for all transitions |
| `--dur-micro` | `100ms` | Tooltip fade, micro-state change, ripple |
| `--dur-fast` | `200ms` | Hover, focus feedback, small reveal |
| `--dur-base` | `300ms` | Standard transitions, element entrances |
| `--dur-slow` | `450ms` | Section transitions, complex animations |

### Shadows — Canonical Opacity Ranges

| Token | Light Mode Opacity | Dark Mode Opacity |
|---|---|---|
| `--shadow-sm` | 0.04-0.06 | 0.20-0.30 |
| `--shadow-md` | 0.08-0.12 | 0.30-0.40 |
| `--shadow-lg` | 0.12-0.16 | 0.40-0.50 |
| `--shadow-accent` | `color-mix(in srgb, var(--accent) 24%, transparent)` | Same (mode-independent) |

### Dark Mode Selector — Only Accepted Pattern

```css
/* Primary: explicit theme toggle */
[data-theme="dark"] { /* ... semantic remaps ... */ }

/* Fallback: OS preference when no explicit toggle is set */
@media (prefers-color-scheme: dark) {
  :root:not([data-theme]) { /* ... duplicate remaps ... */ }
}
```

> No other dark-mode selector pattern is canonical. Do not use `body.dark`, `.dark-mode`, or any other class-based or attribute-based variant.

---

## Documentation Format (Current + Legacy)

Current workflow documentation outputs are:
- `README.md` (system documentation)
- `DECISIONS.md` (decision rationale traceability)
- `SKILL.md` (project-level AI router)

The legacy `DESIGN.md` format is retained for backward compatibility with older outputs.

### Legacy DESIGN.md Format

The DESIGN.md format combines YAML front matter (machine-readable tokens) with
Markdown body (human-readable rationale).

### YAML front matter
```yaml
---
name: Brand Name
version: "1.0"
description: One-line summary of the brand/style

colors:
  primary: "#1A1C1E"
  secondary: "#6C7278"
  accent: "#B8422E"
  surface: "#F7F5F2"
  surface-alt: "#EDEADE"
  on-surface: "#1A1C1E"
  on-accent: "#FFFFFF"
  error: "#D32F2F"
  success: "#2E7D32"

typography:
  display:
    fontFamily: Display Font
    fontSize: 4.5rem
    fontWeight: 700
    lineHeight: 1.05
    letterSpacing: -0.03em
  h1:
    fontFamily: Display Font
    fontSize: 3rem
    fontWeight: 600
    lineHeight: 1.1
    letterSpacing: -0.02em
  body:
    fontFamily: Body Font
    fontSize: 1rem
    fontWeight: 400
    lineHeight: 1.6

rounded:
  sm: 4px
  md: 8px
  lg: 12px
  xl: 20px
  full: 999px

spacing:
  xs: 4px
  sm: 8px
  md: 16px
  lg: 24px
  xl: 48px

components:
  button-primary:
    backgroundColor: "{colors.accent}"
    textColor: "{colors.on-accent}"
    rounded: "{rounded.sm}"
    padding: 12px 24px
  card:
    backgroundColor: "{colors.surface}"
    rounded: "{rounded.md}"
    padding: "{spacing.lg}"
---
```

### Markdown sections (in order)
1. **Overview** — Brand personality, target audience, emotional goals
2. **Colors** — Palette with semantic roles and usage rules
3. **Typography** — Font choices, scale, and hierarchy
4. **Components** — Button, card, input specs with states
5. **Layout** — Grid, spacing, container widths
6. **Elevation** — Shadow or flat elevation system
7. **Do's and Don'ts** — Design guardrails
8. **Responsive** — Breakpoints and mobile behavior
9. **Agent Prompt Guide** — Quick reference for AI use

---

## W3C Design Tokens Community Group (DTCG) Format

The CSS variable architecture above is the primary output format. For interoperability
with tools like Figma Variables, Style Dictionary, or Tokens Studio, also provide a
DTCG-compatible JSON file. The DTCG format uses `$value` and `$type` instead of
CSS custom property syntax.

### DTCG JSON Structure

```json
{
  "color": {
    "neutral": {
      "dark":  { "$value": "#141414", "$type": "color" },
      "light": { "$value": "#FFFFFF", "$type": "color" }
    },
    "accent": {
      "base":  { "$value": "#FF6B00", "$type": "color" },
      "hover": { "$value": "#FF8124", "$type": "color" },
      "press": { "$value": "#CC5500", "$type": "color" }
    },
    "semantic": {
      "bg":       { "$value": "{color.neutral.light}", "$type": "color" },
      "fg":       { "$value": "{color.neutral.dark}", "$type": "color" },
      "accent":   { "$value": "{color.accent.base}", "$type": "color" },
      "error":    { "$value": "#DC2626", "$type": "color" },
      "success":  { "$value": "#16A34A", "$type": "color" },
      "warning":  { "$value": "#F59E0B", "$type": "color" }
    }
  },
  "dimension": {
    "spacing": {
      "1":  { "$value": "4px", "$type": "dimension" },
      "2":  { "$value": "8px", "$type": "dimension" },
      "4":  { "$value": "16px", "$type": "dimension" },
      "6":  { "$value": "32px", "$type": "dimension" },
      "8":  { "$value": "64px", "$type": "dimension" }
    },
    "radius": {
      "sm": { "$value": "4px", "$type": "dimension" },
      "md": { "$value": "8px", "$type": "dimension" },
      "lg": { "$value": "12px", "$type": "dimension" }
    }
  },
  "fontFamily": {
    "display": { "$value": ["Space Grotesk", "system-ui", "sans-serif"], "$type": "fontFamily" },
    "body":    { "$value": ["IBM Plex Sans", "system-ui", "sans-serif"], "$type": "fontFamily" },
    "mono":    { "$value": ["Fira Code", "ui-monospace", "monospace"], "$type": "fontFamily" }
  },
  "fontWeight": {
    "regular":  { "$value": 400, "$type": "fontWeight" },
    "medium":   { "$value": 500, "$type": "fontWeight" },
    "semibold": { "$value": 600, "$type": "fontWeight" },
    "bold":     { "$value": 700, "$type": "fontWeight" }
  }
}
```

### Mapping DTCG to CSS Variables

| DTCG Path | CSS Variable |
|-----------|-------------|
| `color.semantic.bg` | `--bg` |
| `color.accent.base` | `--flame` |
| `dimension.spacing.4` | `--space-4` |
| `dimension.radius.md` | `--radius-md` |
| `fontFamily.display` | `--font-display` |

### Generation Note

When the `constraints` section of the context packet specifies DTCG output,
Agent A generates both `tokens.css` AND `tokens.json` in DTCG format.
Use Style Dictionary or a similar tool to transform the DTCG JSON into
Tailwind config, SCSS variables, or other formats.
