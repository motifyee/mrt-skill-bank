## Design Principles

- **Cards are self-contained content units.** Every card should make sense in isolation, with a clear visual hierarchy from image/icon to title to description to action.
- **Hover elevation signals interactivity.** A card that lifts on hover tells the user it is clickable; a static card tells them it is informational only.
- **Border and shadow are mutually exclusive elevation strategies.** Use a subtle border for flat contexts and shadow for elevated/overlaid contexts, never both at once.
- **Internal alignment uses flex-column with bottom-anchored CTAs.** When cards sit in a grid, the call-to-action should align across cards regardless of content length.

## Brand Expression

Before applying defaults, check `creative_brief`, `character_rules.cards`, and
`components.component_style_contract.cards`.

- `safe`: standard border or shadow language from the style contract.
- `elevated`: one distinctive treatment, such as accent top border, inset corner,
  or branded hover lift.
- `bold`: asymmetric padding, offset media, clipped corners, or layered card stacks
  are allowed if readable.
- `experimental`: CSS masks, blend modes, or kinetic hover effects are allowed only
  with reduced-motion and fallback states.

Cards are the main vehicle for signature DNA. At minimum, feature cards,
pricing cards, or stat cards should carry one systemic effect such as a focus
ring, active border, divider, grid line, or hover behavior from
`signature_moment.systemic_effects`.

### Component Tokens

```css
.card {
  --card-padding: var(--space-5);
  --card-radius: var(--radius-md);
  --card-border-color: var(--border);
  --card-bg: var(--bg);
  --card-shadow: var(--shadow-sm);
  --card-shadow-hover: var(--shadow-md);
  --card-title-font: var(--font-display);
  --card-title-size: var(--fs-h4);
  --card-title-weight: var(--fw-semibold);
  --card-body-color: var(--fg-muted);
  --card-body-font: var(--font-body);
  --card-body-size: var(--fs-body-sm);
  --card-media-aspect: 16 / 9;
}
```

### CSS

```css
.card {
  background: var(--card-bg);
  border-radius: var(--card-radius);
  padding: var(--card-padding);
  border: 1px solid var(--card-border-color);
  display: flex;
  flex-direction: column;
  transition: box-shadow var(--dur-slow) var(--ease),
              transform var(--dur-slow) var(--ease);
}

/* Hover state (interactive cards only) */
.card--interactive:hover {
  box-shadow: var(--card-shadow-hover);
  transform: translateY(-2px);
}

/* Focus state */
.card--interactive:focus-visible {
  outline: 2px solid var(--focus-ring);
  outline-offset: 2px;
}

/* Active / pressed state */
.card--interactive:active {
  transform: translateY(0);
  box-shadow: var(--card-shadow);
}

/* Selected state */
.card--selected {
  border-color: var(--accent);
  box-shadow: inset 0 0 0 1px var(--accent);
}

/* Disabled state */
.card--disabled {
  opacity: 0.5;
  pointer-events: none;
}
```

### HTML Pattern

```html
<!-- Interactive Feature Card -->
<a href="/feature" class="card card--interactive" tabindex="0">
  <div class="card__media">
    <img src="feature.jpg" alt="Feature description" loading="lazy" />
  </div>
  <div class="card__body">
    <h3 class="card__title">Feature Name</h3>
    <p class="card__description">
      Description text that explains the feature or offering.
    </p>
  </div>
  <div class="card__footer">
    <span class="card__cta" aria-hidden="true">Learn more</span>
  </div>
</a>

<!-- Static Stat Card -->
<div class="card card--stat">
  <span class="card__stat-label">Monthly Revenue</span>
  <span class="card__stat-value">$12,400</span>
  <span class="card__stat-trend card__stat-trend--up">
    <svg aria-hidden="true" width="16" height="16" viewBox="0 0 16 16">
      <path d="M8 3l5 5H3z" fill="currentColor"/>
    </svg>
    +12.5%
  </span>
</div>

<!-- Selectable Card (radio-like) -->
<label class="card card--selectable">
  <input type="radio" name="plan" class="card__input" aria-label="Pro plan" />
  <span class="card__title">Pro Plan</span>
  <span class="card__description">$29/month</span>
</label>
```

### Card Structure CSS

```css
.card__media {
  margin: calc(var(--card-padding) * -1);
  margin-block-end: var(--card-padding);
  border-radius: var(--card-radius) var(--card-radius) 0 0;
  overflow: hidden;
  aspect-ratio: var(--card-media-aspect);
}
.card__media img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.card__body {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: var(--space-2);
}

.card__title {
  font-family: var(--card-title-font);
  font-size: var(--card-title-size);
  font-weight: var(--card-title-weight);
  color: var(--fg);
  margin: 0;
}

.card__description {
  font-family: var(--card-body-font);
  font-size: var(--card-body-size);
  color: var(--card-body-color);
  line-height: 1.5;
  margin: 0;
}

.card__footer {
  margin-block-start: auto;
  padding-block-start: var(--space-4);
  display: flex;
  align-items: center;
  gap: var(--space-3);
}

/* Stat card specifics */
.card--stat {
  gap: var(--space-1);
}
.card__stat-label {
  font-family: var(--font-display);
  font-size: var(--fs-label);
  font-weight: var(--fw-medium);
  color: var(--fg-muted);
  text-transform: uppercase;
  letter-spacing: 0.04em;
}
.card__stat-value {
  font-family: var(--font-display);
  font-size: var(--fs-h2);
  font-weight: var(--fw-bold);
  color: var(--fg);
  font-variant-numeric: tabular-nums;
}
.card__stat-trend {
  display: inline-flex;
  align-items: center;
  gap: var(--space-1);
  font-size: var(--fs-body-sm);
  font-weight: var(--fw-medium);
}
.card__stat-trend--up { color: var(--success); }
.card__stat-trend--down { color: var(--error); }

/* Selectable card */
.card__input {
  position: absolute;
  opacity: 0;
  width: 0;
  height: 0;
}
.card__input:checked + .card__title {
  color: var(--accent);
}
.card--selectable:has(.card__input:checked) {
  border-color: var(--accent);
  box-shadow: inset 0 0 0 1px var(--accent);
}
.card--selectable:has(.card__input:focus-visible) {
  outline: 2px solid var(--focus-ring);
  outline-offset: 2px;
}
```

### Card Variants

- **Standard card**: Border + no shadow. For informational content grids.
- **Elevated card**: Shadow instead of border. For menus, popovers, featured content.
- **Interactive card**: Full clickable surface. Wrap in `<a>` or add `tabindex="0"` + keyboard handler. Hover lifts card.
- **Feature card**: Icon/illustration at top, title, description, optional CTA at bottom.
- **Pricing card**: Highlighted variant for "recommended" tier. Use accent `border-block-start` or accent background.
- **Stat card**: Large metric, label, trend, and optional sparkline. Use tabular numerals.
- **Media card**: Image/video with fixed aspect ratio and accessible alt/caption treatment.
- **Selectable card**: Radio/checkbox input hidden inside the card. Selected state shows accent border.

### Pricing Card Accent

```css
.card--pricing-highlight {
  border-color: var(--accent);
  position: relative;
}
.card--pricing-highlight::before {
  content: 'Recommended';
  position: absolute;
  inset-block-start: 0;
  inset-inline-end: var(--space-4);
  padding: var(--space-1) var(--space-3);
  background: var(--accent);
  color: var(--on-accent);
  font-family: var(--font-display);
  font-size: var(--fs-label);
  font-weight: var(--fw-semibold);
  border-radius: 0 0 var(--radius-sm) var(--radius-sm);
  transform: translateY(-100%);
}
```

### States

- **Default**: neutral surface, clear hierarchy.
- **Hover**: apply style-contract hover exactly; do not invent per file.
- **Focus**: visible focus ring on interactive/selectable cards.
- **Active / pressed**: slight depress (remove translateY).
- **Selected**: use signature DNA or accent border treatment.
- **Disabled**: reduce opacity and remove pointer affordance without hiding content.

### Dark Mode

- Cards in dark mode use `var(--card-bg)` which maps to a slightly elevated surface token, not pure black.
- Border color remaps to `var(--border)` which is lighter in dark mode.
- Shadow tokens should use lower opacity in dark mode; prefer subtle border-based elevation instead.
- Media card images should retain their brightness; avoid applying overlays that reduce contrast.

```css
[data-theme="dark"] .card {
  --card-bg: var(--bg-alt);
  --card-border-color: var(--border);
  --card-shadow: 0 1px 2px rgba(0, 0, 0, 0.3);
  --card-shadow-hover: 0 4px 12px rgba(0, 0, 0, 0.4);
}
```

### Responsive Behavior

- Cards in a grid should use `grid-template-columns: repeat(auto-fill, minmax(280px, 1fr))` for natural wrapping.
- On narrow screens (< 480px), switch to single-column layout.
- Card media aspect ratio may adjust to `4 / 3` on mobile for better use of vertical space.
- Preserve card reading order and avoid equal-height forcing when content becomes cramped.
- Bottom-anchored CTAs use `margin-block-start: auto` in a flex column so they align across cards of different heights.

### Card Internal Layout

```
+----------------------------------+
|  [Icon/Image]                    |  <- Optional visual
|                                  |
|  Title (H3)                      |  <- font-display, weight 600
|  Description text that explains  |  <- font-body, color muted
|  the feature or offering.        |
|                                  |
|  [CTA Button] or [Link ->]       |  <- Aligned bottom if flex column
+----------------------------------+
```

### Accessibility

- Interactive cards wrapped in `<a>` are naturally focusable; add `tabindex="0"` only for `<div>` based cards.
- Card title should be the primary content for screen readers; description and footer are supplementary.
- Media card images must have descriptive `alt` text; decorative images use `alt=""` with `role="presentation"`.
- Stat card values should use `aria-label` for context (e.g., `aria-label="Monthly revenue: $12,400, up 12.5 percent"`).
- Selectable cards: the hidden input must have `aria-label`; the visual selection state must be announced via `aria-checked`.
- Focus ring must be visible against card backgrounds in both light and dark mode.
