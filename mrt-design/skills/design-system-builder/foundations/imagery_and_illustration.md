# Imagery and Illustration

Illustration styles, photo treatment, and art direction per aesthetic.
Cross-reference: aesthetic presets in `aesthetic_directions.md`, color tokens in `visual_system.md`.

## Design Principles

1. **Match the medium to the message:** Use icons for single actions, illustrations for processes and abstract ideas, and photos for real people, places, and products.
2. **Apply aesthetic-specific color grading to every photo:** Never drop raw photos into a layout; apply the filter treatment (warm desaturation, high-contrast B&W, muted luxury, etc.) that matches the active aesthetic preset.
3. **Never place body text on an unmodified photo:** Always use a gradient overlay (40-60% dark for white text) or a contained image with a solid background beside it.
4. **Reduce brightness 10-15% in dark mode:** Full-brightness photos on dark surfaces create glare; dim images and intensify overlays for text readability.

---

## Illustration Style Matrix

| Aesthetic         | Illustration Style                     | Photo Treatment                          | Asset Strategy |
|-------------------|----------------------------------------|------------------------------------------|----------------|
| Warm Editorial    | Hand-drawn ink, editorial spot art     | Warm tone, slight desaturation, film grain | real assets, CSS pattern fallback |
| Neon Dashboard    | Isometric 3D, wireframe, data abstract | High contrast, cyan/magenta tint, dark bg | generated bitmap, icon-only fallback |
| Soft SaaS         | Friendly vector, rounded characters    | Bright, soft lighting, pastel overlays   | real assets, icon-only fallback |
| Brutalist Raw     | Cutout collage, stencil, punk zine     | High contrast B&W, newspaper halftone    | CSS pattern, no imagery fallback |
| Luxury Premium    | Minimal line art, gilded botanical     | Muted, low saturation, deep shadows      | real assets (licensed), icon-only fallback |
| Retro Terminal    | ASCII art, pixel art, CRT phosphor     | Scanline overlay, green tint, dithered   | CSS pattern, icon-only fallback |
| Earth Organic     | Watercolor, textured hand-drawn        | Warm, natural light, earthy saturation   | real assets, CSS pattern fallback |
| Tech Blueprint    | Schematic line art, exploded diagrams  | Blueprint tint, clinical, high detail    | icon-only, generated bitmap for diagrams |
| Candy Pop         | Bold vector, warped shapes, collage    | Saturated, punchy colors, high energy    | generated bitmap, real assets fallback |
| Swiss Precision   | Geometric abstract, systematic shapes  | Clean, high contrast, structured crops   | CSS pattern, icon-only fallback |
| Wabi-Sabi Serene  | Ink wash, asymmetric organic forms     | Muted, soft focus, natural imperfection  | real assets, CSS pattern fallback |
| Rajwada Splendor  | Miniature painting motifs, gold filigree | Warm, rich saturation, jewel-tone grading | real assets (licensed), CSS pattern fallback |
| Islamic Geometry  | Geometric tessellation, arabesque line | Cool tint, structured, high detail       | CSS pattern (primary), icon-only fallback |
| Afrofuture Modern | Bold geometric masks, textile patterns | High contrast, vibrant, warm saturation  | generated bitmap, real assets fallback |

### Asset Strategy Field

Every aesthetic has an `asset_strategy` that determines the primary source of visual
imagery. This prevents placeholder abuse and ensures the system knows what to do
when real assets are unavailable.

| Strategy | When to use | Rules |
|----------|-------------|-------|
| `real assets` | Brand has photography budget, product shots, or team photos | License all stock. Never use watermarked images. Document source URL and license type for each asset. Preferred for marketing and brand-heavy surfaces. |
| `real assets (licensed)` | Brand requires high-quality photography with explicit licensing | Must include license metadata (Creative Commons type, stock license ID, or "original work"). Flag any unlicensed substitution. |
| `generated bitmap` | No real assets available, system needs custom visuals | Use AI-generated images only when the brand has consented. Label as AI-generated in substitution flags. Prefer abstract over representational to avoid deep-uncanny-valley. |
| `CSS pattern` | Technical or minimal aesthetics where decorative imagery is unnecessary | Build visual interest through CSS gradients, repeating patterns (dot grids, diagonal lines), and background-image SVG data URIs. No external image dependencies. |
| `icon-only` | Developer tools, internal apps, or systems where imagery adds no value | Replace all image slots with icon-based compositions (large icons + structured text). Zero image assets to manage. |
| `no imagery` | Brutalist or terminal aesthetics that explicitly reject decorative imagery | No image tags at all. Visual interest comes from typography, borders, and color alone. |

### Per-Surface Imagery Requirements

| Surface | Default Strategy | Notes |
|---------|-----------------|-------|
| Marketing | `real assets` or `generated bitmap` | Hero and testimonials always need imagery. Product shots if applicable. |
| Dashboard | `icon-only` or `CSS pattern` | Charts and data visualizations replace traditional imagery. Avatars may need real photos. |
| Settings / Admin | `no imagery` or `icon-only` | Purely functional. Icons for section labels, no decorative images. |
| Docs | `icon-only` or `CSS pattern` | Diagrams and code blocks replace imagery. Callout icons for admonitions. |

### Licensing and Source Rules

1. **Never ship watermarked images.** If using stock, document the source and license.
2. **AI-generated images must be flagged** in the substitutions section with `reason: "AI-generated placeholder"`.
3. **Unsplash/Pexels** are acceptable for prototypes only; document that production requires licensed alternatives.
4. **CSS patterns and icon-only strategies** have no licensing concerns and are preferred for internal/developer tools.
5. **Document every image asset** in the project README with: source URL, license type, dimensions, and whether it is a placeholder or final asset.

---

## When to Use What

### Icons vs Illustrations vs Photos

```
Need to convey a single concept or action?
  -> ICON (see iconography.md)
     Example: settings gear, trash icon, checkmark

Need to explain a process, feature, or abstract idea?
  -> ILLUSTRATION
     Example: onboarding flow, feature explanation, empty state

Need to show real people, places, or products?
  -> PHOTO
     Example: team section, product shots, location, testimonials with portraits

Need visual richness without specific subject matter?
  -> ABSTRACT ILLUSTRATION or PATTERN
     Example: hero background, section dividers, texture fills
```

### Abstract vs Representational

- **Abstract:** Brands that emphasize innovation, technology, or minimalism. Shapes, gradients, patterns. Works for Neon Dashboard, Tech Blueprint, Swiss Precision.
- **Representational:** Brands that emphasize human connection, craft, or storytelling. Scenes, characters, objects. Works for Warm Editorial, Earth Organic, Soft SaaS.
- **Mixed:** Most brands benefit from both. Use abstract for backgrounds and transitions, representational for feature explanations and empty states.

### Full-Bleed vs Contained

- **Full-bleed images:** Hero sections, immersive backgrounds, emotional impact. Requires overlay for text readability.
- **Contained images:** Cards, feature grids, testimonials. Easier to control composition. Use consistent border-radius matching `--radius-*` tokens.
- **Rule:** Dense layouts use contained images. Sparse layouts can afford full-bleed.

---

## Photo Treatment Rules

Before choosing a treatment, define the `imagery` packet fields:

| Field | Decision |
|---|---|
| `photo_style` | lifestyle, product, editorial, documentary, minimal, or none |
| `illustration_style` | geometric, hand-drawn, diagrammatic, 3D, pattern-based, or none |
| `color_treatment` | full-color, duotone, desaturated, high-contrast, monochrome, or none |
| `crop_rules` | hero ratio, card ratio, focal-point position, safe text overlay area |
| `subject_guidelines` | what to show, what to avoid, cultural/accessibility constraints |

Marketing-heavy systems fail when imagery is only a placeholder. Even when no
real assets exist, define art direction so placeholders teach the future asset
strategy: crop, color grade, subject matter, and overlay rules.

### Color Grading Per Aesthetic

```css
/* Warm Editorial: warm tone, slight desaturation */
.img--warm-editorial {
  filter: saturate(0.85) sepia(0.1) contrast(1.05);
}

/* Neon Dashboard: cool tint, high contrast on dark */
.img--neon-dashboard {
  filter: saturate(1.2) contrast(1.15) brightness(0.9);
}

/* Luxury Premium: muted, low saturation */
.img--luxury {
  filter: saturate(0.6) contrast(1.1) brightness(0.95);
}

/* Earth Organic: warm, natural */
.img--earth {
  filter: saturate(1.1) sepia(0.08) brightness(1.02);
}

/* Brutalist: high contrast B&W */
.img--brutalist {
  filter: grayscale(1) contrast(1.4);
}

/* Candy Pop: saturated, punchy */
.img--candy {
  filter: saturate(1.3) contrast(1.1) brightness(1.05);
}
```

### Aspect Ratio Standards

| Context         | Ratio    | CSS Token                   |
|-----------------|----------|-----------------------------|
| Hero images     | 16:9     | `--ratio-hero: 16/9`       |
| Card thumbnails | 4:3      | `--ratio-card: 4/3`        |
| Square cards    | 1:1      | `--ratio-square: 1/1`      |
| Portraits       | 3:4      | `--ratio-portrait: 3/4`    |
| Avatars         | 1:1      | `--ratio-avatar: 1/1`      |
| Bento wide      | 21:9     | `--ratio-ultrawide: 21/9`  |

```css
.img-frame {
  aspect-ratio: var(--ratio-card);
  overflow: hidden;
  border-radius: var(--radius-md);
}
.img-frame img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
```

### Overlay Patterns

```css
/* Hero text overlay: gradient from dark base */
.hero-overlay {
  background: linear-gradient(
    to top,
    var(--bg) 0%,
    rgba(0, 0, 0, 0.6) 40%,
    rgba(0, 0, 0, 0.2) 100%
  );
}

/* Light overlay for text on light images */
.hero-overlay--light {
  background: linear-gradient(
    to right,
    var(--bg) 40%,
    transparent 100%
  );
}
```

- Dark overlays: 40-60% opacity for white text readability.
- Light overlays: used in split heroes where text is on one side only.
- Never place body text directly on an unmodified photo.

### Object-Fit and Object-Position

```css
/* Default: cover fills frame, crops edges */
.img-frame img { object-fit: cover; }

/* Products on white: contain to show full item */
.img-frame--product img { object-fit: contain; object-position: center; }

/* Portraits: focus on face (top-center bias) */
.img-frame--portrait img { object-position: center 20%; }

/* Landscapes: center bias */
.img-frame--landscape img { object-position: center; }
```

---

## Placeholder Strategy

When generating mockups without real images, use these techniques instead of broken image icons.

### CSS Gradient Placeholders

```css
.placeholder {
  background: linear-gradient(
    135deg,
    var(--bg-alt) 0%,
    var(--border) 50%,
    var(--bg-alt) 100%
  );
  display: grid;
  place-items: center;
  color: var(--fg-muted);
  font-size: var(--fs-label);
}
.placeholder::after {
  content: attr(data-label);
}
```

### Photo Source URLs

Use search-term-based source URLs for realistic placeholders:

```html
<!-- Warm Editorial: lifestyle, warm tone -->
<img src="https://images.unsplash.com/photo-XXXXX?w=800&q=80" alt="Placeholder: warm lifestyle scene">

<!-- Neon Dashboard: technology, dark mood -->
<img src="https://images.unsplash.com/photo-XXXXX?w=800&q=80" alt="Placeholder: technology abstract">

<!-- Earth Organic: nature, food, organic -->
<img src="https://images.unsplash.com/photo-XXXXX?w=800&q=80" alt="Placeholder: organic food scene">
```

### SVG Pattern Backgrounds

```css
.placeholder-pattern {
  background-color: var(--bg-alt);
  background-image: url("data:image/svg+xml,..."); /* dot grid, diagonal lines, etc. */
}
```

**Rule:** Always add a visible substitution notice near placeholder images:
```html
<div class="placeholder-notice">Replace with final image</div>
```

---

## Dark Mode Imagery

Images need adjustment in dark mode to avoid jarring brightness.

```css
[data-theme="dark"] img:not([class*="logo"]) {
  filter: brightness(0.85) saturate(0.9);
}

/* Overlays shift direction */
[data-theme="dark"] .hero-overlay {
  background: linear-gradient(
    to top,
    var(--bg) 0%,
    rgba(0, 0, 0, 0.75) 50%,
    rgba(0, 0, 0, 0.3) 100%
  );
}

/* Illustrations with white backgrounds need treatment */
[data-theme="dark"] .illustration--light-bg {
  filter: invert(1) hue-rotate(180deg);
  /* Only works for monochrome/simple illustrations */
  border-radius: var(--radius-md);
  mix-blend-mode: screen;
}

/* Photos in cards: reduce contrast against dark surfaces */
[data-theme="dark"] .card img {
  opacity: 0.92;
}
```

### Dark Mode Image Rules

1. **Reduce brightness** 10-15%. Full-brightness photos on dark backgrounds create glare.
2. **Increase overlay opacity.** Dark-to-darker gradients need heavier masking for text readability.
3. **Avoid light-background illustrations** in dark mode. Use inverted variants or transparent SVGs.
4. **Gradients reverse.** Dark-to-light overlays become dark-to-darker.
5. **Color grading intensifies slightly.** Warm images go warmer; cool images go cooler.
