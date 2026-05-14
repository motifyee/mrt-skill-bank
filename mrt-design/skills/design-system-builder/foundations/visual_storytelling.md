# Visual Storytelling

Visual narrative flow, section rhythm, and creating signature moments.
Cross-reference: layout patterns in `layout_compositions.md`, imagery in `imagery_and_illustration.md`.

## Design Principles

1. **Match rhythm to surface type:** Marketing pages can use a five-act narrative. Dashboards, docs, settings, and admin tools need task-oriented rhythms instead.
2. **Alternate rhythm dimensions between sections:** Never place two adjacent sections on the same side of density, content type, background, and appeal; at minimum, alternate background and density.
3. **One signature moment per page:** Choose exactly one distinctive visual element (oversized type, full-bleed photo, bento grid, etc.) and restrain all other sections so that moment is memorable.
4. **Three tiers of visual weight only:** Establish dominant (signature), standard (content), and quiet (supporting) tiers; cap display-size type and full-bleed imagery to the dominant section.

---

## Visual Narrative Arc

Use the five-act arc for marketing, launch, portfolio, and brand storytelling
surfaces. Do not force it onto product-control surfaces.

### 1. Hook (Hero)

Grab attention. State the value in under 5 seconds.

- One headline. One subheadline. One CTA. No exceptions.
- The hero must answer: "What is this, and why should I care?"
- Visual treatment: full-bleed image, oversized type, or bold illustration (see `imagery_and_illustration.md`).
- Token usage: `--fs-display` for headline, `--accent` for CTA, high contrast.

### 2. Build (Features / Details)

Deepen understanding. Show how the product works.

- 3-6 feature cards or a feature bento grid (see `layout_compositions.md`).
- Each feature: icon, heading (1 line), description (2-3 lines).
- Progressive disclosure: lead with the most impactful feature, end with the most technical.
- Visual treatment: illustration or screenshot per feature, not generic stock.

### 3. Proof (Testimonials / Metrics / Case Studies)

Establish credibility. Third-party validation is more persuasive than self-claims.

- Metrics band with specific numbers (see `layout_compositions.md`, pattern 10).
- Testimonial row with real names, titles, and portraits.
- Logo wall for social proof (especially enterprise/B2B).
- Case study links or video testimonials for depth.

### 4. Climax (Pricing / CTA)

The conversion moment. This is where the visitor decides.

- Pricing columns (see `layout_compositions.md`, pattern 12) for SaaS.
- Single CTA banner (pattern 13) for simpler products.
- Reduce decision friction: maximum 3 options, clear recommendation on the featured tier.
- Visual treatment: accent background, elevated prominence, no competing elements nearby.

### 5. Resolution (Final CTA / Footer)

Last impression. Often overlooked but critical for visitors who scroll to the bottom.

- Repeat the primary CTA in a different visual treatment.
- Footer with navigation, legal links, and brand reinforcement.
- Optional: a final tagline or micro-testimonial.

---

## Section Rhythm

Alternate between contrasting section characteristics to prevent visual fatigue.

### Rhythm Dimensions

| Dimension       | Pole A            | Pole B              |
|-----------------|-------------------|----------------------|
| Density         | Dense (cards, data)| Sparse (one big visual) |
| Content type    | Visual-heavy      | Text-heavy           |
| Background      | Light (--bg)      | Dark (--bg-alt)      |
| Appeal          | Logical (data)    | Emotional (story)    |

**Rule:** Never place two adjacent sections on the same side of all four dimensions.
At minimum, alternate background and density.

### Marketing Page Rhythm Template

A standard SaaS marketing page with optimal section sequence:

```
1. HERO                    — Sparse, visual, light bg, emotional (Hook)
2. LOGO WALL               — Dense, visual, light bg, logical (Proof warmup)
3. FEATURE GRID            — Dense, balanced, light bg, logical (Build)
4. BIG FEATURE (split)     — Sparse, visual, alt bg, emotional (Build)
5. METRICS BAND            — Dense, visual, alt bg, logical (Proof)
6. TESTIMONIALS            — Dense, text, light bg, emotional (Proof)
7. PRICING                 — Dense, balanced, light bg, logical (Climax)
8. FAQ                     — Dense, text, alt bg, logical (Objection handling)
9. FINAL CTA BANNER        — Sparse, visual, accent bg, emotional (Resolution)
10. FOOTER                 — Dense, text, dark bg, logical (Close)
```

Key transitions: 1->2 (sparse to dense), 3->4 (light to alt bg), 5->6 (alt to light), 8->9 (logical to emotional).

### Surface-Specific Rhythm

| Surface | Rhythm Model | What To Avoid |
|---|---|---|
| Marketing | Hook -> Build -> Proof -> Climax -> Resolution | Endless equal card grids |
| Dashboard | Scan -> Filter -> Inspect -> Act -> Confirm | Dramatic hero sections or narrative scroll |
| Documentation | Orient -> Navigate -> Explain -> Reference -> Next step | Marketing-style hype and hidden navigation |
| Settings | Group -> Explain consequence -> Edit -> Save -> Confirm | Decorative signature moments near risky controls |
| Admin/Internal | Prioritize -> Triage -> Bulk act -> Audit trail | Sparse brand drama that slows operators |

Signature moments on task surfaces should improve orientation or confidence:
selected-row treatment, data-as-ornament, active filter rails, or calm status
animation. They should not compete with the user's work.

---

## Visual Weight Distribution

### F-Pattern and Z-Pattern

**F-Pattern** for text-heavy pages (blogs, documentation, news):
- Eye scans left-to-right across the top, then drops down and scans a shorter horizontal line.
- Place key information on the left: headings, lead sentences, CTA alignment.
- Use for: documentation, long-form articles, pricing detail pages.

**Z-Pattern** for visual pages (landing pages, hero sections):
- Eye follows a Z: top-left to top-right, diagonal to bottom-left, across to bottom-right.
- Place primary CTA top-right. Place secondary CTA bottom-right.
- Use for: hero sections, simple landing pages, app splash screens.

### Signature Hierarchy: Anchor vs DNA

The signature system has two layers that work together:

**Signature Anchor** — appears exactly once per page/surface. This is the hero
moment, the single visual idea that makes the page memorable.
- The thing a visitor describes when telling someone about the page
- Distinctive enough to screenshot and share
- Occupies exactly one section (typically the hero or a proof section)
- Never competes with other sections for attention

**Signature DNA** — propagates into ordinary components across the entire system.
This is how the anchor's visual idea reaches buttons, cards, nav, inputs, and
other daily-use elements.
- Must appear in at least 3 distinct component categories
- Expressed as specific tokens or CSS states, not conceptual language
- Examples: "focus rings use the same glow as the hero grid", "active nav
  carries the same accent underline as the signature divider", "card hover
  inherits the same border treatment as the featured proof section"

This split resolves the conflict between "one signature moment" (for page-level
drama) and "systemic propagation" (for brand consistency). The anchor creates
the memory; the DNA ensures the system feels cohesive.

All other sections should be restrained. If everything is bold, nothing is.

### Encoding in the Context Packet

In `schemas/packet_schema.md`:
- `signature_anchor`: a single phrase naming the visual idea
- `signature_dna`: 3+ propagation rules, each naming a specific component
  and how it carries the mark
- `signature_moment`: legacy field kept for backward compatibility

### Signature DNA for Mundane Components

Signature DNA must propagate beyond hero sections and marketing surfaces into
the components users interact with daily. Below are concrete patterns for how
DNA manifests in mundane UI elements.

**Toast Notifications**
- Carry the signature border or accent treatment in the notification's left
  stripe or icon area. Example: if the signature is "calibrated cyan glow,"
  toasts use a thin cyan left border instead of a generic colored bar.
- The entrance animation should echo the system's motion signature (easing
  curve and duration), not a default slide-in.
- Dismiss button uses the same focus ring treatment as all interactive elements
  per `signature_dna` propagation rules.

**Settings Forms**
- Section dividers in settings pages inherit the signature divider treatment
  (or lack thereof) from `components.component_style_contract.sections.divider`.
- Toggle switches use the signature accent for the active state, matching the
  accent used in hero and navigation -- not a generic system blue.
- Save/confirm buttons follow the exact `button_primary` contract values,
  ensuring the signature hover and transition behavior is consistent.

**Tables**
- Row hover states echo the signature card hover treatment from
  `component_style_contract.card_default.hover_effect`. If cards get an accent
  top border on hover, table rows get a subtle accent left border or background
  tint using the same token.
- Selected/active rows use the same visual language as selected nav items
  (per `signature_dna` propagation rules).
- Sort indicators and filter chips use the signature accent, not a default
  UI color.

**Error States**
- Error borders and icons use `error` semantic tokens, not the signature accent,
  to preserve semantic clarity. The signature DNA must not override error/warning
  colors.
- Error message typography follows the `voice` profile (tone, casing, length),
  which is part of the system's identity.
- Focus rings on error-state inputs still use the signature focus ring treatment
  (from `signature_dna`), maintaining visual coherence even in failure states.

**Rule:** If a `signature_dna` entry names a component category, every instance
of that category across all surfaces (marketing, dashboard, settings, docs) must
carry the mark. There should be no surface where DNA-propagated components look
generic or system-default.

### Hierarchy Without Competition

- Establish three tiers of visual weight: dominant (signature moment), standard (content sections), and quiet (supporting elements).
- Only one section per page is dominant.
- Use `--fs-display` only in the signature section. Other sections cap at `--fs-h2`.
- Full-bleed imagery appears in maximum two sections. Everything else is contained.
- Accent color used in maximum 2-3 elements per viewport. Not in every heading, badge, and icon.

---

## Signature Anchor Types

Catalog of distinctive visual techniques. Choose one per page as the anchor.
Each type should also define how its DNA propagates into ordinary components.

### Signature Moment Synthesis

Invent before selecting. Use this formula:

1. Brand attribute: what should the interface feel like?
2. Product truth: what is structurally true about the product or data?
3. Visual technique: what material, motion, type, layout, or image treatment expresses it?
4. Systemic effect: how does that idea influence normal components?
5. Constraint: where would the effect become distracting or inaccessible?

Example: "Deployment confidence" + "pipelines move through stages" + "calibrated
grid glow" becomes a hero diagnostic grid, selected pipeline focus rings, and
chart active states. Constraint: no glow on body text or dense table rows.

### Oversized Typography

Headline at 80-120px that dominates the viewport.

- **When:** Strong value proposition, minimal copy, brand confidence.
- **Implementation:** `font-size: clamp(3rem, 8vw, 8rem); line-height: 0.95;`
- **Accessibility:** Ensure contrast ratio passes WCAG AA. Do not use thin weights below 400.

### Asymmetric Layouts

Off-center content, irregular grids, intentional imbalance.

- **When:** Creative brands, portfolios, editorial. Avoid for data-heavy or form-heavy pages.
- **Implementation:** `grid-template-columns: 2fr 1fr;` with content offset using `margin-block-start: var(--space-20);`
- **Accessibility:** Maintain logical DOM order. Visual asymmetry should not break keyboard/reading order.

### Full-Bleed Photography

Edge-to-edge image spanning the viewport with text overlay.

- **When:** Emotional impact, lifestyle brands, product showcases.
- **Implementation:** `background-size: cover; background-position: center;` with gradient overlay (see `imagery_and_illustration.md`).
- **Accessibility:** Text over images requires overlay. Contrast ratio must be verified with the actual image behind it.

### Kinetic / Animated Elements

Motion that draws attention through movement: parallax, scroll-triggered reveals, hover animations.

- **When:** Tech products, creative portfolios. Avoid for content-heavy or accessibility-critical pages.
- **Implementation:** `animation` with `prefers-reduced-motion` fallback. `transition` for hover states.
- **Accessibility:** Respect `prefers-reduced-motion: reduce`. All animations must have a still fallback.

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

### Bento Grids with Mixed Content

Asymmetric card grid mixing text, images, stats, and illustrations (Apple-style).

- **When:** Product feature overviews, platform showcases, app marketing.
- **Implementation:** See Feature Bento pattern in `layout_compositions.md` (pattern 8).
- **Accessibility:** Each bento card is a distinct landmark. Use semantic HTML (`<article>`, `<section>`).

### 3D / Depth Effects

Parallax layers, perspective transforms, glassmorphism, depth shadows.

- **When:** Premium product pages, interactive showcases. Performance-sensitive.
- **Implementation:** `transform: perspective(1000px) rotateY(...)` with `will-change: transform`.
- **Accessibility:** Provide a flat/2D fallback. Avoid for essential content comprehension.

### Grain / Texture Overlays

Film grain, paper texture, noise that adds tactile quality.

- **When:** Warm Editorial, Earth Organic, Luxury Premium aesthetics.
- **Implementation:** SVG noise filter at low opacity overlaid on sections.
- **Accessibility:** Purely decorative. Ensure it does not reduce text contrast.

```css
.texture-grain::after {
  content: "";
  position: absolute;
  inset: 0;
  background: url("data:image/svg+xml,...") repeat;
  opacity: 0.03;
  pointer-events: none;
}
```

### Custom Illustration Features

A bespoke illustration or animated illustration sequence as the page centerpiece.

- **When:** Brands with established illustration systems. Product onboarding, feature narratives.
- **Implementation:** SVG with CSS animations. Inline SVG for interactive control.
- **Accessibility:** Provide `role="img"` and `aria-label` on decorative SVGs. Descriptive alt text for informative illustrations.

### Split-Screen Compositions

Two-panel layout with contrasting content: image left / text right, dark left / light right.

- **When:** Product comparisons, before/after, dual-audience messaging.
- **Implementation:** See Split View in `layout_compositions.md` (pattern 4).
- **Accessibility:** Each panel is independently readable. Logical reading order is maintained.

### Parallax / Scroll-Driven Reveals

Content that appears or transforms as the user scrolls.

- **When:** Story-driven pages, timelines, product tours. Avoid for task-focused pages.
- **Implementation:** Intersection Observer API for reveal animations. CSS `scroll-driven-animations` where supported.
- **Accessibility:** Content must be accessible without scrolling. Reveal animations are progressive enhancement.

---

## Page Flow Examples

### 1. SaaS Landing Page (Developer Tool)

Aesthetic: Neon Dashboard or Tech Blueprint. Audience: technical buyers.

```
HERO (split)          Dark bg, terminal-style headline, code snippet visual
                        -> "Ship faster with [tool]"
LOGO WALL             "Trusted by engineering teams at"
FEATURES (grid)       6 cards: Performance, Security, API, SDK, Scale, Monitor
                        -> each with code snippet or terminal visual
METRICS BAND          99.9% uptime / 50ms latency / 10M+ requests / 500+ teams
SIGNATURE: Bento      One large bento grid showing the product dashboard
                        -> this is the memorable visual
INTEGRATION LOGOS     "Works with your stack" — tech logos (AWS, Vercel, etc.)
TESTIMONIALS          3 quotes from engineering leads
PRICING               3 tiers: Free / Pro / Enterprise (Pro featured)
DOCS CTA              "Read the docs" + "Get started free"
FOOTER                Dark, technical
```

Rhythm: dark-light-dark alternation. Sparse hero, dense features, sparse bento, dense pricing.

### 2. Premium Product Page (Luxury Aesthetic)

Aesthetic: Luxury Premium. Audience: high-income consumers.

```
HERO (centered)       Near-black bg, single product image, minimal type
                        -> Oversized serif headline, generous whitespace
SIGNATURE: Full-bleed  Edge-to-edge product photography, scroll-reveal details
                        -> This is the shareable moment
CRAFTSMANSHIP (split)  Left: macro detail photo. Right: story text about materials
FEATURES              3 minimal cards: Material / Design / Sustainability
                        -> muted, restrained, ample space
QUOTE                 Single customer quote, large type
PRICING / CTA         Price displayed with minimal styling, "Add to bag" button
                        -> Gold accent (#C9A96E) on CTA only
DETAILS               Accordion: shipping, returns, care instructions
FINAL CTA             "Experience [brand]" with subtle background texture
FOOTER                Minimal, dark, elegant
```

Rhythm: sparse throughout with controlled density only in features and details. Emotional tone dominates.

### 3. Agency Portfolio (Creative Aesthetic)

Aesthetic: Candy Pop or Brutalist Raw. Audience: potential clients.

```
HERO (centered)       Bold, playful headline with animated text
                        -> "We make brands impossible to ignore"
SIGNATURE: Asymmetric  Overlapping portfolio thumbnails in irregular grid
                        -> Hover reveals project details
SELECTED WORK (bento)  4-5 case study cards, mixed sizes
                        -> Each card is a project thumbnail + client name
METRICS               "12 industries / 200+ projects / 98% retention"
TESTIMONIALS          3 client quotes with portraits
PROCESS (timeline)    4 steps: Discover / Design / Build / Launch
                        -> horizontal on desktop, vertical on mobile
CTA BANNER            Gradient bg matching brand accent
                        -> "Start a project" form or modal link
FOOTER                Contact info, social links, playful sign-off
```

Rhythm: high energy throughout. Dense bento alternates with sparse testimonials. Visual-first approach.
