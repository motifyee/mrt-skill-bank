# CSS Validation

Property-level checks for generated `tokens.css` output.
All token names below match the canonical dictionary in `schemas/token_schema.md`.

## Required Properties

Verify these custom property categories exist in the generated CSS:

### Color Tokens
```
--neutral-900 through --neutral-50 (or equivalent evocative raw names)
--accent-500, --accent-400, --accent-300, --accent-200, --accent-600
--success-raw, --warning-raw, --error-raw, --info-raw (semantic raw)
--bg, --bg-alt, --bg-inset, --bg-inverse
--fg, --fg-muted, --fg-subtle
--accent, --accent-hover, --accent-press, --accent-tint, --on-accent
--border, --border-strong, --focus-ring
--success, --success-tint, --warning, --warning-tint, --error, --error-tint, --info, --info-tint
```

### Typography Tokens
```
--font-display, --font-body, --font-mono
--fs-display, --fs-h1, --fs-h2, --fs-h3, --fs-h4
--fs-body-lg, --fs-body, --fs-body-sm, --fs-label, --fs-mono
--lh-display, --lh-heading, --lh-body, --lh-body-sm, --lh-label
--tracking-display, --tracking-heading, --tracking-body, --tracking-label
--fw-regular, --fw-medium, --fw-semibold, --fw-bold
```

### Spacing Tokens
```
--space-1 through --space-10
--container-max
--nav-h, --nav-h-sm
```

### Shape Tokens
```
--radius-sm, --radius-md, --radius-lg, --radius-xl, --radius-full
--shadow-sm, --shadow-md, --shadow-lg, --shadow-accent
```

### Motion Tokens
```
--ease
--dur-micro, --dur-fast, --dur-base, --dur-slow
```

### Density Tokens
```
--density-scale, --touch-target
```

### Z-Index Tokens (required when modals/dropdowns/toasts are generated)
```
--z-base, --z-dropdown, --z-sticky, --z-modal, --z-toast, --z-tooltip
```

## Regex Validation

Run against the generated CSS:

```bash
# Check custom properties are defined (not just referenced)
grep -cP '^\s*--' tokens.css  # Should be 60+

# Check no empty values
grep -P '^\s*--[\w-]+:\s*;' tokens.css  # Should return 0 lines

# Check no hardcoded colors in component layer (only in raw layer)
# Component tokens should reference other vars, not hex values
grep -P '^\s*--(btn|card|input|badge|nav|footer).*:.*#[0-9a-fA-F]' tokens.css
# Should return 0 lines — component tokens must use var() references

# Check no !important
grep -c '!important' tokens.css  # Should be 0

# Check phantom token names are absent
grep -P '--fg-heading|--semantic-error|--semantic-success|--fg-on-accent|--fg-disabled|--brand-' tokens.css
# Should return 0 lines

# Check canonical motion token names only
grep -P '--dur-(?!micro|fast|base|slow)' tokens.css
# Should return 0 lines (no non-canonical durations)

# Check all 10 spacing steps are defined
for i in $(seq 1 10); do grep -q -- "--space-$i:" tokens.css || echo "MISSING: --space-$i"; done
```

## Three-Layer Structure Validation

The generated CSS must have three distinct comment sections:

1. `/* ---------- Color — raw palette ---------- */` — hex values only
2. `/* ---------- Color — semantic ---------- */` — var() references to raw tokens
3. Component tokens (in separate component CSS or inline) — var() references to semantic tokens

Each layer must only reference the layer below it (raw ← semantic ← component).

## Dark Mode Selector Validation

Only these two selectors are canonical:
```css
[data-theme="dark"] { ... }

@media (prefers-color-scheme: dark) {
  :root:not([data-theme]) { ... }
}
```

```bash
# Check for non-canonical dark mode selectors
grep -P '\.dark\b|body\.dark|\.dark-mode|data-theme="dark-mode"' tokens.css
# Should return 0 lines
```

## Passing Threshold

- **PASS**: All required properties present, zero empty values, three-layer structure intact, no phantom tokens
- **PASS WITH WARNINGS**: Missing 1-2 non-critical tokens (e.g., z-index when no overlays generated)
- **FAIL**: Missing semantic layer, empty values, layer boundary violations, phantom token names present
