## Design Principles

- **Badges are compact status signals, not interactive elements.** They should never be clickable by themselves; pair with a tooltip or link wrapper if action is needed.
- **Color communicates category, text confirms meaning.** A green badge says "positive" via color and "Active" via text -- both channels reinforce the message.
- **Badge contrast must pass WCAG AA on any background.** The tinted background + saturated text pattern ensures readability without relying on the badge border.
- **Keep badge text under three words.** Badges that wrap to multiple lines have failed their purpose as inline indicators.

## Brand Expression

Badges are small, so brand expression must be precise. Apply
`creative_brief.risk_dial` and `character_rules.cards`/`sections` only through
shape, border, typography, and icon treatment:

- `safe`: rounded status pill with text confirmation.
- `elevated`: signature border, small icon, or uppercase label treatment.
- `bold`: angled corner, outlined label, or category-specific marker if still legible.
- `experimental`: avoid for critical status badges; reserve for marketing tags.

Never let badges become the only carrier of color meaning.

### Component Tokens

```css
.badge {
  --badge-padding-inline: var(--space-1) var(--space-3);
  --badge-padding-block: 2px;
  --badge-radius: var(--radius-full);
  --badge-font-size: var(--fs-label);
  --badge-font-weight: var(--fw-semibold);
  --badge-letter-spacing: 0.03em;
  --badge-icon-size: 14px;
  --badge-min-height: 22px;
}
```

### CSS

```css
.badge {
  display: inline-flex;
  align-items: center;
  gap: var(--space-1);
  padding: var(--badge-padding-block) var(--badge-padding-inline);
  font-family: var(--font-display);
  font-size: var(--badge-font-size);
  font-weight: var(--badge-font-weight);
  letter-spacing: var(--badge-letter-spacing);
  border-radius: var(--badge-radius);
  line-height: 1;
  white-space: nowrap;
  min-height: var(--badge-min-height);
  transition: background-color var(--dur-fast) var(--ease),
              color var(--dur-fast) var(--ease);
}

/* Semantic variants */
.badge--accent {
  background: var(--accent-tint);
  color: var(--accent);
}
.badge--success {
  background: color-mix(in srgb, var(--success) 12%, transparent);
  color: var(--success);
}
.badge--warning {
  background: color-mix(in srgb, var(--warning, #f59e0b) 12%, transparent);
  color: var(--warning, #f59e0b);
}
.badge--error {
  background: color-mix(in srgb, var(--error) 12%, transparent);
  color: var(--error);
}
.badge--info {
  background: color-mix(in srgb, var(--accent) 12%, transparent);
  color: var(--accent);
}
.badge--neutral {
  background: var(--bg-alt);
  color: var(--fg-muted);
}
```

### Size Variants

```css
/* Compact — used inside dense lists or table cells */
.badge--sm {
  --badge-padding-inline: var(--space-1) var(--space-2);
  --badge-font-size: var(--fs-caption, 0.6875rem);
  --badge-min-height: 18px;
}

/* Default — standard inline usage */
/* Uses base .badge tokens */

/* Large — standalone labels or hero sections */
.badge--lg {
  --badge-padding-inline: var(--space-2) var(--space-4);
  --badge-padding-block: var(--space-1);
  --badge-font-size: var(--fs-body-xs, 0.75rem);
  --badge-min-height: 28px;
}
```

### Shape Variants

```css
/* Rounded pill (default) */
.badge--pill {
  --badge-radius: var(--radius-full);
}

/* Subtle rounded rectangle */
.badge--rounded {
  --badge-radius: var(--radius-sm);
}

/* Outlined style — transparent background with colored border */
.badge--outlined {
  background: transparent;
  border: 1.5px solid currentColor;
}
```

### Badge with Icon

```css
.badge__icon {
  width: var(--badge-icon-size);
  height: var(--badge-icon-size);
  flex-shrink: 0;
}
.badge--sm .badge__icon {
  --badge-icon-size: 12px;
}
.badge--lg .badge__icon {
  --badge-icon-size: 16px;
}
```

### Dot Indicator

A small colored circle preceding the text, used for status signaling:

```css
.badge__dot {
  width: 8px;
  height: 8px;
  border-radius: var(--radius-full);
  flex-shrink: 0;
  background: currentColor;
}
.badge--sm .badge__dot {
  width: 6px;
  height: 6px;
}
```

### States

```css
/* Default: visible and static */
/* Hover: badges are non-interactive, no hover state by design */
/* Focus: if wrapped in a focusable element (link/button), show inherited focus ring */
/* Disabled: badges cannot be disabled; they represent state, not action */
```

### HTML Pattern

```html
<!-- Text-only badge -->
<span class="badge badge--success">Active</span>

<!-- Badge with dot indicator -->
<span class="badge badge--warning">
  <span class="badge__dot" aria-hidden="true"></span>
  Pending Review
</span>

<!-- Badge with icon -->
<span class="badge badge--error">
  <svg class="badge__icon" viewBox="0 0 16 16" aria-hidden="true">
    <circle cx="8" cy="8" r="8" fill="currentColor" opacity="0.2"/>
    <path d="M5 5l6 6M11 5l-6 6" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
  </svg>
  Failed
</span>

<!-- Large outlined badge -->
<span class="badge badge--lg badge--outlined badge--accent">New Feature</span>
```

### Dark Mode

- Badge background tints should use lower opacity in dark mode (8% instead of 12%) to avoid appearing washed out.
- Text colors remain the same semantic token; ensure the combination still passes AA.
- Outlined badges gain a slightly more visible border width in dark mode (1.5px -> 2px) for contrast.

```css
@media (prefers-color-scheme: dark) {
  .badge--success {
    background: color-mix(in srgb, var(--success) 8%, transparent);
  }
  .badge--error {
    background: color-mix(in srgb, var(--error) 8%, transparent);
  }
  .badge--outlined {
    border-width: 2px;
  }
}
```

### Responsive Behavior

- Badges are inline elements and reflow naturally with surrounding text.
- On very narrow containers, avoid using large badges inside table cells; switch to the `badge--sm` size variant.
- Dot-only badges (no text) may be used on small screens to save space, but must include `aria-label` for screen readers.

### Accessibility

- Badges use `<span>` by default; they are not interactive.
- When a badge conveys status, ensure the surrounding context (table column header, list item label) provides the semantic meaning.
- Dot indicators are decorative (`aria-hidden="true"`); the text carries the accessible meaning.
- If a badge is the sole indicator of state in a table, add `aria-label` to the cell or use `visually-hidden` text alongside the badge.
- Color contrast: badge text against badge background must pass WCAG AA (4.5:1 for normal text, 3:1 for large text).
- Badge text against the page background behind it must also pass AA when the badge uses an outlined variant with no fill.

### Required Variants

- Accent/promotional
- Success
- Warning
- Error
- Info
- Neutral
- With dot indicator
- With icon
- Outlined
- Size: sm / default / lg
