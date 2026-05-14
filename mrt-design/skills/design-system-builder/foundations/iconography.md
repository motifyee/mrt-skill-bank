# Iconography Foundations

How to select, size, apply, and govern icon systems in a design system.

## Design Principles

1. **One library per system:** Choose a single icon library and enforce it; mixing stroke styles, weights, or sources creates immediate inconsistency.
2. **Icons inherit currentColor:** Default every icon to `currentColor` so it tracks its parent text; apply accent or muted colors only to active states and decorative roles.
3. **Never rely on icons alone for meaning:** Pair every icon with a visible text label or `aria-label` so meaning survives accessibility tools and unfamiliar contexts.
4. **Respect the 16px floor:** Below 16px icons lose legibility; use text or color indicators instead of shrinking icons further.

---

## Icon System Selection

### Recommended icon libraries

| Library        | Style       | Weight    | Best for                        |
|---------------|-------------|-----------|---------------------------------|
| Lucide        | Stroke      | 1.5-2px   | General purpose, clean, modern  |
| Phosphor      | Stroke/fill | 1.5px     | Flexible (6 weights per icon)   |
| Heroicons     | Stroke/outline | 1.5-2px | Tailwind ecosystem, simple     |
| Tabler Icons  | Stroke      | 2px       | Dense UI, many variants         |
| Bootstrap Icons | Stroke/fill | 1-2px   | Bootstrap ecosystem             |

### Selection criteria
- Choose ONE icon library per design system. Mixing creates inconsistency.
- Stroke-based icons pair with modern, lightweight interfaces.
- Filled icons work for dense interfaces where icons need more visual weight.
- The icon style should match the brand's display font character (geometric = geometric icons, humanist = softer icons).
- Consider the icon count — a library with 1000+ icons covers more edge cases.

### Custom icon sets
If the brand has custom icons:
- Require SVG format only (no icon fonts, no PNG sprites)
- Establish a grid (typically 24x24 viewBox)
- Define consistent stroke width and corner style
- Provide a Figma/Sketch source file for future additions
- Document the naming convention (e.g., `icon-name-variant-size.svg`)

---

## Icon Sizing

### Standard scale

| Size   | Use case                                  |
|--------|-------------------------------------------|
| 16px   | Inline with small text, tight spaces      |
| 20px   | Inline with body text (default)           |
| 24px   | Navigation, buttons, standard UI          |
| 32px   | Feature highlights, empty states          |
| 48px   | Hero illustrations, prominent features    |

Default size is 20px for body-text inline use, 24px for UI elements.

### Sizing rules
- Icons in buttons: 20px, centered vertically with text, 8px gap
- Icons in navigation: 24px, with or without text label
- Icons in feature cards: 24-32px, above the title
- Icons in alerts/toasts: 20px, left-aligned before text
- Icons in tables: 16px inline with cell content

---

## Icon Color

### Rules
- Icons inherit `currentColor` by default — they match their parent text color.
- Accent-colored icons should be used sparingly (active states, important actions).
- Muted icons use the secondary/muted foreground color.
- Never use more than two icon colors in a single view (default + accent).

### Color application
```css
/* Default: inherit text color */
.icon { color: inherit; }

/* Muted: for decorative or secondary icons */
.icon-muted { color: var(--fg-muted); }

/* Accent: for active or emphasized icons */
.icon-accent { color: var(--accent); }
```

---

## Icon + Text Pairing

### Alignment
- Icons before text: vertically centered, 8px gap
- Icons after text: vertically centered, 6px gap (tighter for trailing indicators)
- Icons above text: horizontally centered, 8px gap (feature cards)

### Rules
- Every icon should have a text label or `aria-label` — never rely on icon alone for meaning
- Navigation icons should have visible text labels on desktop (icon-only acceptable on mobile with aria-label)
- Button icons: always paired with text for primary actions; icon-only acceptable for universal actions (close, search, settings) with aria-label

---

## What to Avoid

- **No emoji as icons** — emoji render differently across platforms and lack design control
- **No unicode characters as icons** (arrows, bullets, checkmarks) — use the icon library's SVG versions
- **No mixing icon styles** — if using stroke, don't introduce filled icons without a clear system rule
- **No decorative icons without purpose** — every icon should earn its space by adding meaning
- **No tiny icons** — below 16px, icons lose legibility; use text or color indicators instead

---

## Substitution Flag

When no icon library is specified by the brand, select the closest match and document it:
```
Icon library: [Library Name] — selected as substitute. No custom icon set was provided.
If the brand has a custom icon set, place SVGs in assets/icons/ and update the system.
```

---

## Icon Loading Strategy

### Implementation Patterns

**1. Lucide Icons (CDN for prototypes)**

```html
<!-- In <head> or before closing </body> -->
<script src="https://unpkg.com/lucide@latest"></script>

<!-- Usage: icon element -->
<i data-lucide="settings" width="20" height="20"></i>
<i data-lucide="trash-2" width="16" height="16" class="icon-muted"></i>

<!-- Initialize after DOM is ready -->
<script>lucide.createIcons();</script>
```

**2. Heroicons (inline SVG pattern)**

```html
<!-- No CDN needed — copy SVG directly from heroicons.com -->
<!-- Outline variant (24x24 viewBox, stroke-width 1.5) -->
<svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
     stroke-width="1.5" stroke="currentColor" width="20" height="20"
     class="icon" aria-hidden="true">
  <path stroke-linecap="round" stroke-linejoin="round"
        d="M9.594 3.94c.09-.542.56-.94 1.11-.94h2.593c.55 0 1.02.398 1.11.94l.213 1.281c.063.374.313.686.645.87.074.04.147.083.22.127.325.196.72.257 1.075.124l1.217-.456a1.125 1.125 0 0 1 1.37.49l1.296 2.247a1.125 1.125 0 0 1-.26 1.431l-1.003.827c-.293.241-.438.613-.43.992a7.723 7.723 0 0 1 0 .255c-.008.378.137.75.43.991l1.004.827c.424.35.534.955.26 1.43l-1.298 2.247a1.125 1.125 0 0 1-1.369.491l-1.217-.456c-.355-.133-.75-.072-1.076.124a6.47 6.47 0 0 1-.22.128c-.331.183-.581.495-.644.869l-.213 1.281c-.09.543-.56.94-1.11.94h-2.594c-.55 0-1.019-.398-1.11-.94l-.213-1.281c-.062-.374-.312-.686-.644-.87a6.52 6.52 0 0 1-.22-.127c-.325-.196-.72-.257-1.076-.124l-1.217.456a1.125 1.125 0 0 1-1.369-.49l-1.297-2.247a1.125 1.125 0 0 1 .26-1.431l1.004-.827c.292-.24.437-.613.43-.991a6.932 6.932 0 0 1 0-.255c.007-.38-.138-.751-.43-.992l-1.004-.827a1.125 1.125 0 0 1-.26-1.43l1.297-2.247a1.125 1.125 0 0 1 1.37-.491l1.216.456c.356.133.751.072 1.076-.124.072-.044.146-.086.22-.128.332-.183.582-.495.644-.869l.214-1.28Z" />
  <path stroke-linecap="round" stroke-linejoin="round"
        d="M15 12a3 3 0 1 1-6 0 3 3 0 0 1 6 0Z" />
</svg>
```

**3. Phosphor Icons (CDN)**

```html
<!-- In <head> or before closing </body> -->
<script src="https://unpkg.com/@phosphor-icons/web"></script>

<!-- Usage: regular weight (default), size via font-size or width/height -->
<i class="ph ph-gear" style="font-size: 20px;" aria-hidden="true"></i>
<i class="ph-bold ph-trash" style="font-size: 16px;" aria-hidden="true"></i>
```

**4. Inline SVG Fallback (no CDN, universal)**

```html
<!-- Use when no icon library is available or for production -->
<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20"
     viewBox="0 0 20 20" fill="none" stroke="currentColor"
     stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"
     class="icon" aria-hidden="true">
  <circle cx="10" cy="10" r="3"/>
  <path d="M10 1v2M10 17v2M1 10h2M17 10h2"/>
</svg>
```

### Choosing a Loading Strategy

| Context | Recommended Approach | Reason |
|---------|---------------------|--------|
| Prototypes and design-system previews | CDN (Lucide or Phosphor) | Fastest setup; no build step; one `<script>` tag |
| Production web apps | Inline SVG | No external dependency; tree-shakeable; works offline; best performance |
| React/Vue/Svelte projects | Framework package (e.g., `lucide-react`, `@heroicons/react`) | Tree-shaking; component API; TypeScript support |
| High-security / air-gapped environments | Inline SVG only | No CDN calls; no external network requests |
| No icon library specified | Lucide CDN for prototypes, inline SVG for production | Lucide is the default recommendation; clean, modern, comprehensive |

### Production Guidance

For production deployments:
1. Do not load entire icon libraries via CDN — include only the icons used.
2. Prefer inline SVG or framework-specific packages that support tree-shaking.
3. Set `aria-hidden="true"` on decorative icons; use `aria-label` on interactive icon buttons.
4. Ensure all icons use `currentColor` for consistency with the design system's color tokens.
