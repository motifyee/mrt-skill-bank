# Visual System Foundations

How to build color palettes, typography systems, spacing scales, visual hierarchy,
motion vocabularies, and elevation systems — from professional design-system practice.

## Design Principles

1. **Three-layer color architecture:** Structure every palette as raw values, semantic mappings, and component bindings so theme switching changes only the semantic layer.
2. **Dark mode is remapping, not inversion:** Reassign semantic tokens to dark surfaces with elevated lighter shades and reduced image brightness rather than flipping every value.
3. **Accent through restraint:** Use the primary accent color on at most 2-3 elements per viewport; its power comes from scarcity, not repetition.
4. **Constrain type to a modular scale:** Generate every font size from a single ratio (e.g., 1.25) and limit to 2-3 weights per family to prevent visual noise.
5. **Performance is a hard constraint:** Keep total CSS under 50 KB unminified, limit font families to 3, and animate only GPU-composited properties (transform, opacity).

---

## Color Systems

### Architecture: Raw -> Semantic -> Component

Every color system has three layers:

**Raw palette** — the actual hex values with evocative or systematic names.
```css
--blue-600: #2563EB;
--slate-900: #0F172A;
```

**Semantic mapping** — what each color *means* in the UI.
```css
--bg: var(--slate-900);
--fg: var(--white);
--accent: var(--blue-600);
```

**Component binding** — how tokens apply to specific components.
```css
--btn-primary-bg: var(--accent);
--btn-primary-fg: var(--on-accent);
```

This three-layer architecture enables theme switching (light/dark/brand variants)
by changing only the semantic layer while raw values and component bindings stay stable.

### Palette Construction

A complete palette needs:

| Role              | Count  | Purpose                                      |
|-------------------|--------|----------------------------------------------|
| Neutral scale     | 6-10   | bg, surfaces, borders, muted text, deep bg   |
| Accent (primary)  | 3-5    | CTA, links, active states, focus ring         |
| Accent (secondary)| 0-3    | Optional: secondary actions, categories       |
| Semantic          | 3-4    | Success, warning, error, info                 |
| On-[color]        | Per bg | Text/icon color that's readable on each bg    |

**Neutral scale construction:**
- Start from the darkest (near-black) and lightest (near-white) values
- Fill intermediate steps with consistent perceptual spacing
- Warm/cool/neutral are not enough for distinctive systems. Choose a named
  neutral family before generating the scale:
  - Sandstone: mineral beige, terracotta dust, warm editorial surfaces
  - Concrete: industrial grey, blue-green undertone, architectural restraint
  - Ink: blue-black/violet-black, dark-first, premium or technical depth
  - Moss: green-brown neutrals, organic and grounded without beige defaulting
  - Smoke: cool desaturated greys, clinical or technical precision
  - Brass: muted gold-brown neutrals, institutional warmth or luxury
- Warm neutrals: use a family like Sandstone, Brass, or Volcanic Earth instead
  of merely adding yellow to grey.
- Cool neutrals: use a family like Concrete, Smoke, Ink, or Alpine Winter instead
  of merely adding blue to grey.
- Neutral temperature is a brand decision — derive from interview, don't assume

**OKLCH interpolation for reproducible neutral scales:**

Interpolate neutrals in OKLCH color space for perceptually even spacing. The L (lightness) channel controls the step, while C (chroma) and H (hue) encode temperature:

```
// Warm neutral scale (10 steps, L from 98 to 12, hue ~80):
oklch(0.98 0.005 80)  → lightest
oklch(0.12 0.015 80)  → darkest

// Cool neutral scale (10 steps, L from 98 to 12, hue ~250):
oklch(0.98 0.005 250) → lightest
oklch(0.12 0.015 250) → darkest
```

Process: define lightest + darkest in OKLCH, linear-interpolate L in equal steps,
then convert to sRGB hex. This produces scales where each step feels equidistant
to the human eye — unlike HSL which clusters mid-tones.

**Contrast algorithm:**
- Current: WCAG 2.x relative luminance (4.5:1 normal text, 3:1 large text)
- Future path: APCA (Accessible Perceptual Contrast Algorithm) provides perceptually
  accurate contrast scores that handle the known weaknesses of WCAG 2.x (false passes
  on dark-on-dark, false fails on light-on-dark). When APCA reaches W3C Recommendation,
  switch contrast validation from WCAG 2.x ratios to APCA Lc values.

**Accent color rules:**
- An accent needs at least: base, hover (lighter or brighter), press (darker)
- An accent tint (base at 10-15% opacity on the light surface) is useful for
  subtle backgrounds, selected states, and badges
- Each accent must have a documented `on-accent` color (usually white or the
  darkest neutral) that passes contrast ratio requirements

**Semantic colors:**
- Success: green family. Pair with a text label — never rely on color alone.
- Warning: amber/yellow family. Higher contrast requirement since yellow is hard to see.
- Error: red family. Most important semantic color; deserves the most attention.
- Info: blue family. Optional — often the primary accent doubles as info.

### Dark Mode

Dark mode is not "invert everything." It's a separate semantic mapping:

- Backgrounds become dark (but NOT pure black — use #0A0A0A to #18181B)
- Foreground text becomes light (but NOT pure white — use #E4E4E7 to #FAFAFA)
- Surfaces gain *slight* elevation through lighter shades (not shadow)
- Accent colors may need to shift lighter to maintain contrast on dark backgrounds
- Borders become lighter at low opacity: `rgba(255,255,255, 0.08-0.16)`
- Shadows become darker but less visible — dark-on-dark shadows add depth subtly

Implement via `[data-theme="dark"]` attribute that remaps semantic tokens. The
`prefers-color-scheme` media query may be used as a fallback only when no explicit
theme is set. See `schemas/theming_schema.md` for the canonical theme-switching
approach. Do NOT use a `.dark` CSS class.

---

## Typography Systems

### Font Selection Principles

Typography choices must start with language, script, brand values, and page role.
Read `references/typography_selection.md` before finalizing fonts for non-English,
multilingual, culturally specific, luxury, playful, editorial, or typography-led
systems. Do not force Latin type assumptions onto Arabic, CJK, Devanagari, Bengali,
Tamil, Hebrew, Thai, Cyrillic, or other scripts.

**Display / heading fonts** should carry the brand's spirit. A luxurious system may
need high-contrast serif or calligraphic display forms; a comic system may need
controlled irregularity and lively shapes; a friendly system may need open counters
and humanist rhythm; a technical system may need engineered forms and precise
numerals. The font should express a named brand value, not merely "look modern."

**Body fonts** must be readable at 14-18px across devices. Prioritize large x-height,
open counters, and good hinting. The body font should complement the display font
through contrast (serif vs sans, geometric vs humanist) or coherence (same family,
different weights).

**Role-specific fonts** assign expression carefully:
- Hero and brand text may be expressive if legible.
- Section headings reinforce identity without competing with the hero.
- Body, docs, forms, settings, and dashboard text prioritize reading comfort.
- Labels, tables, and code need precise metrics, clear numerals, and stable widths.

**Monospace fonts** for code: JetBrains Mono, Fira Code, IBM Plex Mono, Source Code
Pro. Choose ligature support if the product shows code.

Load fonts using the project strategy in the context packet: Google Fonts `<link>`,
bundled `@font-face`, licensed/local files, or system-fonts-only mode. Never assume
local font availability, and never use CSS `@import` inside `tokens.css`.

### Type Scale Construction

Use a modular scale with a consistent ratio, then tune metrics per script. Latin
tracking and line-height rules do not automatically work for Arabic, CJK, Devanagari,
Thai, or Hebrew. Script legibility overrides aesthetic drama.

| Ratio   | Name             | Feel                    |
|---------|------------------|-------------------------|
| 1.125   | Major second     | Compact, dense          |
| 1.200   | Minor third      | Comfortable, readable   |
| 1.250   | Major third      | Spacious, editorial     |
| 1.333   | Perfect fourth   | Dramatic, expressive    |
| 1.500   | Perfect fifth    | Very dramatic           |

A typical 10-level scale at ratio 1.25 from base 16px:
display (72px), h1 (56px), h2 (40px), h3 (28px), h4 (20px),
body-lg (18px), body (16px), body-sm (14px), label (12px), caption (11px).

Each level needs explicit: font-family, font-size, font-weight, line-height,
letter-spacing, and page-part usage. Never leave these implicit.

### Script-Sensitive Metrics

| Script / use | Metric guidance |
|---|---|
| Latin | Body 16px, display can use tight line-height and slight negative tracking |
| Arabic | Body often 17-18px equivalent, line-height 1.7-1.9, avoid negative tracking |
| CJK | Body 15-16px, line-height 1.6-1.8, avoid extreme heading jumps |
| Devanagari | Body 16-18px, line-height 1.6-1.8, preserve vertical room for matras |
| Thai | Body 16-18px, line-height 1.7+, avoid tiny labels |
| Hebrew | Body 16px+, line-height 1.5-1.7, avoid Latin-style tracking |
| Data-heavy UI | Use smaller ratios (1.125-1.2) while preserving script legibility |
| Brand-forward hero | Use 1.333+ only when the script remains readable and culturally fitting |

### Line Height Rules
- Display/large headings: 1.0 - 1.15 (tight)
- Subheadings: 1.15 - 1.3 (moderate)
- Body text: 1.5 - 1.7 (comfortable reading)
- Labels/captions: 1.2 - 1.4 (compact)

### Letter Spacing Rules
- Large type (>32px): tighten with negative tracking (-0.01 to -0.03em)
- Body text: leave at 0
- Uppercase labels: widen with positive tracking (+0.05 to +0.12em)
- Small text (<12px): slight widening (+0.01em) improves readability

### Font Weight Strategy
Limit to 2-3 weights per family to avoid visual noise:
- Regular (400): body text, descriptions
- Medium (500): labels, navigation, subtle emphasis
- Semibold/Bold (600-700): headings, button labels, strong emphasis

---

## Spacing & Layout

### The Base Unit

Choose 4px or 8px as the base unit. 4px offers finer control; 8px is more
opinionated but harder to misuse.

A 4px-based scale: `4 / 8 / 12 / 16 / 24 / 32 / 48 / 64 / 96 / 128`
An 8px-based scale: `8 / 16 / 24 / 32 / 48 / 64 / 96 / 128`

Name tokens numerically (--space-1 through --space-10) or semantically
(--space-xs through --space-4xl). Numeric is more flexible; semantic
is more readable.

### Application Guide

| Context              | Typical token range  |
|----------------------|----------------------|
| Inside buttons       | space-2 to space-4 (8-16px) |
| Between form fields  | space-4 to space-5 (16-24px) |
| Card internal padding| space-5 to space-6 (24-32px) |
| Between sections     | space-8 to space-9 (64-96px) |
| Page-level gutters   | space-5 mobile, space-7 desktop (24-48px) |

### Border Radius

Design tokens for corner roundness. Choice of radius scale is one of the strongest signals of a system's personality.

**Token Scale:**

| Token | Value | Use Case |
|-------|-------|----------|
| --radius-none | 0 | Tags, badges, flat buttons (brutalist/minimal) |
| --radius-sm | 4px | Badges, tags, small indicators, chips |
| --radius-md | 8px | Buttons, inputs, cards, dropdowns |
| --radius-lg | 12px | Modals, larger cards, feature sections |
| --radius-xl | 20px | Hero sections, prominent cards, promotional blocks |
| --radius-full | 999px | Pills, avatars, circular elements, toggle thumbs |

**Aesthetic Mapping:**

| Aesthetic | Base Radius | Range | Rationale |
|-----------|-------------|-------|-----------|
| Warm Editorial | --radius-sm | 2-6px | Subtle warmth, not sharp; small radius avoids appearing clinical |
| Neon Dashboard | --radius-sm | 4-8px | Technical precision; slight rounding softens data density |
| Soft SaaS | --radius-md | 8-14px | Friendly and approachable; wider range allows warmth |
| Brutalist Raw | --radius-none | 0px | Intentionally unrounded; sharp corners are the point |
| Luxury Premium | --radius-sm | 2-4px | Understated refinement; restraint signals quality |
| Retro Terminal | --radius-none | 0px | CRT-era authenticity; pixel-sharp edges |
| Earth Organic | --radius-lg | 12-20px | Natural, flowing shapes; higher end for prominent elements |
| Tech Blueprint | --radius-sm | 4-6px | Engineering precision; just enough to avoid visual noise |
| Candy Pop | --radius-xl | 16-24px | Playful and bubbly; generous rounding for energy |
| Swiss Precision | --radius-sm | 2-4px | Systematic exactness; minimal rounding for order |

> These are recommended ranges. If the brand warrants deviation, document the
> reason in the context packet. Agents should stay within the range but pick
> a specific value that serves the brand, not default to the midpoint.

**Application Rules:**
- A single system should use at most 3 radius tokens (e.g., sm + md + full)
- Interactive elements (buttons, inputs) use the same radius for visual consistency
- Modal/dialog radius should be 1-2 steps above component radius for visual hierarchy
- Nested rounded elements: inner radius = outer radius - padding (the "rounded nesting" principle)
- Full-radius (pill) is reserved for: toggles, avatars, notification badges, filter chips

**CSS Implementation:**
```css
:root {
  --radius-none: 0;
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;
  --radius-xl: 20px;
  --radius-full: 999px;
}
```

### Grid System
- 12-column grid is standard for marketing sites (divisible by 2, 3, 4, 6)
- Component-level layout: use CSS Grid and Flexbox, not the page grid
- Max content width: 1200-1440px depending on content density
- Readable text: constrain to ~72ch (about 600-700px)

---

## Visual Hierarchy

### The Five Tools of Hierarchy

You have exactly five tools to signal importance. Use them in combination:

1. **Size** — larger elements draw attention first
2. **Weight** — bolder type feels more important
3. **Color** — high contrast / accent color = important; low contrast / muted = secondary
4. **Space** — more space around an element elevates its perceived importance
5. **Position** — top-left (in LTR languages) and center get seen first

### The Squint Test
Blur your eyes (or blur the design at 10px). The visual hierarchy should still be
obvious: primary headline, primary CTA, key content blocks should be identifiable
even without reading any text.

### Attention Budget
Each viewport has one primary focus and at most two secondary focuses.
If everything is bold, nothing is bold.

- **One primary CTA per viewport** — not two, not three
- **One display-size headline per page** — the hero gets it, nothing else
- **Accent color used sparingly** — the power comes from restraint

---

## Elevation & Depth

### Shadow-Based Elevation (default)

| Level | Use Case            | Shadow Value                                   |
|-------|---------------------|------------------------------------------------|
| 0     | Flush with surface  | none                                           |
| 1     | Cards, slight raise | `0 1px 3px rgba(0,0,0,0.06), 0 1px 2px rgba(0,0,0,0.04)` |
| 2     | Dropdowns, hover    | `0 4px 12px rgba(0,0,0,0.08), 0 2px 4px rgba(0,0,0,0.04)` |
| 3     | Modals, drawers     | `0 12px 32px rgba(0,0,0,0.12), 0 4px 8px rgba(0,0,0,0.06)` |
| 4     | Toasts, popovers    | `0 16px 48px rgba(0,0,0,0.16)` |

### Flat Elevation (alternative)
Some brands prefer borders over shadows. In flat systems:
- Elevation 0: no border
- Elevation 1: 1px border in the line/border color
- Elevation 2: stronger border (1.5-2px or darker color)
- Elevation 3: background color shift (slightly lighter/darker than parent)

Choose shadow-based or flat elevation based on brand direction, then commit
everywhere. Don't mix randomly.

---

## Motion & Interaction

### Purpose-Driven Motion

Motion should serve exactly one of these purposes:
1. **Feedback** — confirming an action was registered (button press, toggle switch)
2. **Orientation** — showing where something came from or went to (slide, expand)
3. **Focus** — drawing attention to something new (fade in, pulse)
4. **Delight** — a moment of personality (but only if it doesn't slow the user down)

### Easing and Duration

**Easing:** Use a confident ease-out curve as the default: `cubic-bezier(0.2, 0.8, 0.2, 1)`.
This accelerates quickly and decelerates smoothly — it feels decisive, not floaty.

**Duration by interaction type:**

| Interaction       | Duration Token  | Canonical Value | Why                                      |
|-------------------|-----------------|-----------------|------------------------------------------|
| Micro-feedback    | --dur-micro     | 100ms           | Touch response, press confirmation       |
| Hover/focus       | --dur-fast      | 200ms           | Must feel instant — user expects no delay |
| Element entrance  | --dur-base      | 300ms           | Needs to be seen but not waited for       |
| Modal appear      | --dur-base      | 300ms           | Slight drama is appropriate                |
| Page transition   | --dur-slow      | 450ms           | Longest acceptable deliberate motion       |

### What to Animate (and What Not To)

**Animate:** background-color, opacity, transform (translate, scale), box-shadow.
These are GPU-accelerated and perform well.

**Avoid animating:** width, height, top, left, margin, padding. These trigger layout
recalculation and cause jank. Use transform: scale/translate instead.

**Never:** auto-playing video backgrounds (performance), scroll-jacking (usability),
bouncing/wiggling elements (distraction), animations longer than 500ms for routine
interactions (patience).

### Hover / Press / Focus Reference

Combine these state treatments:

| Action | Visual Response |
|--------|----------------|
| Hover | BG color shift (lighter or darker by one shade), subtle lift (translateY -1 to -2px), underline grow for links |
| Press | Deeper color, slight scale down (0.97-0.98), shadow reduction |
| Focus | Visible ring (2px accent color, 2px offset), high contrast against bg |
| Disabled | Opacity 0.4, cursor: not-allowed, no hover/press effects |

---

## Performance Budgets

Generated design systems must be fast. These are hard limits, not aspirations.

**CSS Budgets:**
| Metric | Limit | Rationale |
|--------|-------|-----------|
| Total CSS (unminified) | 50KB | Beyond this, parse time harms FCP |
| Total CSS (gzipped) | 10KB | Mobile network constraint |
| Token file (tokens.css) | 8KB | Must parse before any rendering |
| Custom properties | 200 max | Browser performance ceiling |
| Font files (total) | 300KB | 2-3 fonts at 2-3 weights each |

**Font Loading Strategy:**
```css
/* Critical: block rendering for display + body fonts only */
@font-face {
  font-family: 'DisplayFont';
  src: url('/fonts/display-var.woff2') format('woff2');
  font-weight: 100 900;
  font-display: swap; /* show fallback immediately, swap when loaded */
}

/* Non-critical: icons and mono can load asynchronously */
```
- Use `font-display: swap` for all fonts
- Preload critical fonts: `<link rel="preload" href="/fonts/body-var.woff2" as="font" type="font/woff2" crossorigin>`
- Limit to 2 font families (display + body) + 1 mono = 3 total
- Use variable fonts where available (single file, multiple weights)

### Intentional Fallback Stacks

Every font family declaration must include an intentional fallback stack, not just `sans-serif`. Choose fallbacks that approximate the primary font's visual character:

```css
/* Geometric sans (e.g., Poppins, Outfit) */
font-family: 'Poppins', 'Segoe UI', system-ui, -apple-system, sans-serif;

/* Humanist sans (e.g., Nunito, Source Sans) */
font-family: 'Nunito', 'Calibri', 'Gill Sans', system-ui, sans-serif;

/* Grotesque sans (e.g., Space Grotesk, IBM Plex Sans) */
font-family: 'Space Grotesk', 'Segoe UI', 'Roboto', system-ui, sans-serif;

/* Serif (e.g., Playfair, Cormorant) */
font-family: 'Playfair Display', 'Georgia', 'Cambria', 'Times New Roman', serif;

/* Monospace */
font-family: 'JetBrains Mono', 'Cascadia Code', 'Fira Code', 'Consolas', monospace;

/* CJK (e.g., Noto Sans JP) */
font-family: 'Noto Sans JP', 'Hiragino Sans', 'Yu Gothic', 'Meiryo', sans-serif;

/* Arabic (e.g., Noto Naskh Arabic) */
font-family: 'Noto Naskh Arabic', 'Segoe UI', 'Tahoma', sans-serif;

/* Devanagari (e.g., Mukta, Poppins) */
font-family: 'Mukta', 'Poppins', 'Noto Sans Devanagari', sans-serif;
```

### System-Fonts-Only Mode

For performance-critical or firewalled environments where web fonts cannot be loaded, generate a system-fonts-only variant:

1. Replace all Google Fonts `@import` and `<link>` tags with nothing
2. Use system font stacks that approximate the design's character:
   - Display: `system-ui, -apple-system, 'Segoe UI', sans-serif`
   - Body: `system-ui, -apple-system, 'Segoe UI', sans-serif`
   - Mono: `'Cascadia Code', 'Fira Code', 'Consolas', 'Courier New', monospace`
3. Add a comment at the top of `tokens.css`: `/* SYSTEM FONTS MODE — no web fonts loaded */`
4. Adjust typography scale if system fonts render significantly differently at the same sizes

Apply this mode when: the target environment blocks Google Fonts (China, corporate firewalls), the performance budget is under 100KB total, or the user explicitly requests it.

**Rendering Budgets:**
| Metric | Target | How to Achieve |
|--------|--------|---------------|
| First Contentful Paint | < 1.5s | Inline critical CSS, defer token file if large |
| Largest Contentful Paint | < 2.5s | Preload hero images, no layout shift from fonts |
| Cumulative Layout Shift | < 0.1 | Size images, reserve font size space, avoid late CSS |
| Total Blocking Time | < 200ms | Minimize JS, defer non-critical scripts |

**Image Guidelines:**
- Use `srcset` with WebP/AVIF for all raster images
- SVG for icons and illustrations (not raster)
- Lazy-load below-fold images: `loading="lazy"`
- Hero images: preload with `<link rel="preload">`
- Max image width: 2x the largest display size (retina)

**CSS Optimization Checklist:**
- [ ] No `@import` (use `<link>` or build-tool bundling)
- [ ] Custom properties defined in `:root` only (no per-component variables)
- [ ] No deep specificity nesting (> 3 levels)
- [ ] No `!important` overrides, except in the print stylesheet reset where
      overriding screen-only backgrounds, colors, and interactive UI is intentional
- [ ] Animations use only transform and opacity (GPU-accelerated)
- [ ] Scroll-driven animations preferred over JS-driven where supported
- [ ] `content-visibility: auto` on below-fold sections

---

## Print Stylesheet

Basic print CSS for generated documentation and marketing pages. Apply via `@media print`.

```css
@media print {
  /* Reset */
  *, *::before, *::after {
    background: transparent !important;
    color: #000 !important;
    box-shadow: none !important;
    text-shadow: none !important;
  }

  /* Typography */
  body { font-size: 12pt; line-height: 1.5; }
  h1 { font-size: 24pt; }
  h2 { font-size: 18pt; page-break-after: avoid; }
  h3 { font-size: 14pt; page-break-after: avoid; }

  /* Layout */
  @page { margin: 2cm; }
  article { width: 100%; margin: 0; }
  img { max-width: 100% !important; page-break-inside: avoid; }
  table { page-break-inside: avoid; }

  /* Hide non-printable elements */
  nav, .no-print, video, audio, iframe,
  button, .toast, .modal-overlay, .tooltip { display: none !important; }

  /* Links: show URL */
  a[href]::after { content: " (" attr(href) ")"; font-size: 0.8em; }
  a[href^="#"]::after { content: ""; } /* skip internal links */

  /* Orphans and widows */
  p { orphans: 3; widows: 3; }
}
```

**Print Rules:**
- Never print interactive elements (nav, buttons, modals, tooltips, toasts)
- Show link URLs inline for reference
- Force black text on white background
- Avoid page breaks inside images, tables, and code blocks
