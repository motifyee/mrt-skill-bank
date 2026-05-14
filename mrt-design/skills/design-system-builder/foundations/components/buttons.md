## Design Principles

- **Primary action is always the most visually prominent.** The primary button uses the accent color background; secondary actions recede via outline or ghost styling.
- **Disabled states must not rely on color alone.** Use opacity reduction, cursor changes, and pointer-events to communicate inoperability across all modalities.
- **Touch targets meet minimum 44x44px.** Padding compensates for compact visual sizing so buttons remain tappable on mobile.
- **Semantic element choice is non-negotiable.** Use `<button>` for in-page actions and `<a>` for navigation -- never interchange them.
- **State feedback is immediate and perceptible.** Hover, active, and focus-visible states each produce a distinct visual change (color shift, micro-translate, outline ring).

## Brand Expression

Before applying defaults, check `character_rules.buttons` from the context packet. This field defines the brand-specific rule that overrides generic button styling. Examples:
- `"pill shape, uppercase label"` → apply `border-radius: var(--radius-full); text-transform: uppercase; letter-spacing: 0.08em;`
- `"sharp corners, no fill on secondary"` → apply `border-radius: 0; background: transparent;`
- `"thick border, offset shadow"` → apply `border: 2px solid var(--accent); box-shadow: 3px 3px 0 var(--accent);`

Always apply `character_rules.buttons` BEFORE generic defaults. The character rule wins.

### Structure
Every button system needs 3-4 variants: Primary (accent bg), Secondary (outlined), Ghost (text-only), and optionally Destructive (error color).

```css
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-2);
  padding: var(--space-2) var(--space-5);
  font-family: var(--font-display);
  font-size: var(--fs-body-sm);
  font-weight: var(--fw-semibold);
  letter-spacing: 0.02em;
  border-radius: var(--radius-sm);
  border: none;
  cursor: pointer;
  transition:
    background-color var(--dur-fast) var(--ease),
    border-color var(--dur-fast) var(--ease),
    box-shadow var(--dur-fast) var(--ease),
    color var(--dur-fast) var(--ease);
  text-decoration: none;
  line-height: 1.4;
}
.btn:focus-visible {
  outline: 2px solid var(--focus-ring);
  outline-offset: 2px;
}
.btn:disabled {
  opacity: 0.4;
  cursor: not-allowed;
  pointer-events: none;
}

/* Primary */
.btn-primary {
  background: var(--accent);
  color: var(--on-accent);
}
.btn-primary:hover { background: var(--accent-hover); transform: translateY(-1px); }
.btn-primary:active { background: var(--accent-press); transform: translateY(0); }

/* Secondary */
.btn-secondary {
  background: transparent;
  color: var(--fg);
  border: 1.5px solid var(--border-strong);
}
.btn-secondary:hover {
  background: var(--bg-alt);
}

/* Ghost */
.btn-ghost {
  background: transparent;
  color: var(--accent);
  padding-inline-start: var(--space-3);
  padding-inline-end: var(--space-3);
}
.btn-ghost:hover { background: var(--accent-tint); color: var(--accent); }

/* Destructive */
.btn-destructive {
  background: var(--error);
  color: var(--on-accent);
}
.btn-destructive:hover { opacity: 0.88; }
```

### Sizes

Use token-based padding. Never use arbitrary px values for button sizing.

```css
/* sm */
.btn-sm {
  padding: var(--space-1) var(--space-3);
  font-size: var(--fs-label);
}

/* md (default) */
.btn-md {
  padding: var(--space-2) var(--space-5);
  font-size: var(--fs-body-sm);
}

/* lg */
.btn-lg {
  padding: var(--space-3) var(--space-6);
  font-size: var(--fs-body);
}
```

### Icon Buttons

When a button contains only an icon, use `padding-inline-start` and `padding-inline-end` equally and add `aria-label`:

```css
.btn-icon {
  padding-inline-start: var(--space-2);
  padding-inline-end: var(--space-2);
  aspect-ratio: 1;
}
```

### Accessibility
- Always use `<button>` for actions, `<a>` for navigation
- Include `aria-label` when text is icon-only
- Focus-visible ring must be clearly visible against all backgrounds
- Minimum touch target: 44x44px
- Never use `transition: all` — explicit property transitions prevent unexpected animation of layout-affecting properties like `width` or `height`
- Use `padding-inline-start` / `padding-inline-end` instead of `padding-left` / `padding-right` for RTL support
