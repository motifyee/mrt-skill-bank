# Theming Schema

Light, dark, and high-contrast theme switching using CSS custom properties.

---

## Theme Architecture

The three-layer token system keeps theme switching isolated to the semantic layer:

```
Raw tokens          Semantic tokens          Component tokens
(never change)      (remapped per theme)     (never change)

--neutral-50: #FFF  --bg: var(--neutral-50)  --btn-bg: var(--accent)
--neutral-900:#1717 --fg: var(--neutral-900)  --btn-fg: var(--on-accent)
--accent-raw: #0066  --accent: var(--accent-raw)
```

Raw tokens hold palette values. Semantic tokens assign roles. Component tokens
reference semantic tokens. To switch themes, remap semantic tokens only.

---

## Light Theme (Default)

```css
:root {
  /* Surface */
  --bg: var(--neutral-50);
  --bg-alt: var(--neutral-100);
  --bg-inset: var(--neutral-100);

  /* Foreground */
  --fg: var(--neutral-900);
  --fg-muted: var(--neutral-500);
  --fg-subtle: var(--neutral-400);

  /* Accent */
  --accent: var(--accent-500);
  --accent-hover: var(--accent-400);
  --accent-press: var(--accent-300);
  --accent-tint: var(--accent-200);
  --on-accent: var(--neutral-50);

  /* Borders */
  --border: var(--neutral-300);
  --border-strong: var(--neutral-400);

  /* Semantic */
  --success: var(--success-raw);
  --success-tint: var(--success-tint-raw);
  --warning: var(--warning-raw);
  --warning-tint: var(--warning-tint-raw);
  --error: var(--error-raw);
  --error-tint: var(--error-tint-raw);
  --info: var(--info-raw);
  --info-tint: var(--info-tint-raw);

  /* Focus */
  --focus-ring: var(--accent);

  /* Shadows — use --shadow-color variable for easy dark-mode override */
  --shadow-color: rgba(0, 0, 0, 0.05);   /* tune opacity per brand; range 0.04-0.06 */
  --shadow-sm: 0 1px 2px var(--shadow-color);
  --shadow-md: 0 4px 12px var(--shadow-color);
  --shadow-lg: 0 12px 32px var(--shadow-color);
}
```

---

## Dark Theme

> In dark mode, semantic accent tokens remap to lighter raw accent stops for sufficient contrast. The raw palette provides the stops; theme tokens select which stop to use.

Use dark neutrals for backgrounds, never pure black. Light neutrals for
foreground, never pure white. Accent may shift one step lighter for contrast.

```css
[data-theme="dark"] {
  /* Surface */
  --bg: var(--neutral-900);
  --bg-alt: var(--neutral-800);
  --bg-inset: var(--neutral-700);

  /* Foreground */
  --fg: var(--neutral-50);
  --fg-muted: var(--neutral-400);
  --fg-subtle: var(--neutral-500);

  /* Accent — same raw stops; semantic layer handles contrast on dark surfaces */
  --accent: var(--accent-500);
  --accent-hover: var(--accent-400);
  --accent-press: var(--accent-300);
  --accent-tint: color-mix(in srgb, var(--accent-500) 15%, transparent);
  --on-accent: var(--neutral-900);

  /* Borders */
  --border: rgba(255, 255, 255, 0.1);
  --border-strong: rgba(255, 255, 255, 0.2);

  /* Shadows — elevate --shadow-color opacity for dark surfaces (range 0.25-0.50) */
  --shadow-color: rgba(0, 0, 0, 0.35);
  --shadow-sm: 0 1px 4px var(--shadow-color);
  --shadow-md: 0 4px 16px var(--shadow-color);
  --shadow-lg: 0 12px 40px var(--shadow-color);

  /* Semantic */
  --success: var(--success-raw);
  --success-tint: rgba(34, 197, 94, 0.15);
  --warning: var(--warning-raw);
  --warning-tint: rgba(245, 158, 11, 0.15);
  --error: var(--error-raw);
  --error-tint: rgba(239, 68, 68, 0.15);
  --info: var(--info-raw);
  --info-tint: rgba(59, 130, 246, 0.15);

  /* Focus */
  --focus-ring: var(--accent);
}
```

---

## High Contrast Theme

Optimized for WCAG AAA and users who need maximum legibility. Prefer raw token
references so palette maintenance still propagates; use literal values only
when the product intentionally defines separate high-contrast raw tokens.

```css
[data-theme="high-contrast"] {
  /* Surface — maximum contrast raw tokens */
  --bg: var(--neutral-50);
  --bg-alt: var(--neutral-100);
  --bg-inset: var(--neutral-200);

  /* Foreground */
  --fg: var(--neutral-900);
  --fg-muted: var(--neutral-800);
  --fg-subtle: var(--neutral-700);

  /* Accent — use deeper stops for maximum contrast in high-contrast mode */
  --accent: var(--accent-600);
  --accent-hover: var(--accent-600);
  --accent-press: var(--accent-600);
  --accent-tint: var(--accent-200);
  --on-accent: var(--neutral-50);

  /* Borders — thick and visible */
  --border: var(--neutral-900);
  --border-strong: var(--neutral-900);

  /* Semantic — no reliance on color alone, always pair with icon/text */
  --success: #006600;
  --success-tint: #E6FFE6;
  --warning: #664400;
  --warning-tint: #FFF5E6;
  --error: #CC0000;
  --error-tint: #FFE6E6;
  --info: #004488;
  --info-tint: #E6F0FF;

  /* Focus — enhanced */
  --focus-ring: #000000;

  /* Shadows — removed, use borders instead */
  --shadow-color: transparent;
  --shadow-sm: 0 0 0 2px var(--border);
  --shadow-md: 0 0 0 2px var(--border);
  --shadow-lg: 0 0 0 3px var(--border-strong);
}

/* High-contrast needs thicker borders on interactive elements */
[data-theme="high-contrast"] button,
[data-theme="high-contrast"] input,
[data-theme="high-contrast"] select,
[data-theme="high-contrast"] textarea {
  border-width: 2px;
}

/* Focus ring must be unmistakable */
[data-theme="high-contrast"] :focus-visible {
  outline: 3px solid var(--focus-ring);
  outline-offset: 2px;
}
```

---

## Theme Switching Implementation

### Canonical approach: Data attribute

This skill uses `data-theme` attributes exclusively. The `.dark` CSS class approach
is NOT used because it cannot support more than two themes. **Legacy -- prefer `[data-theme="dark"]`. The `.dark` class should not be used in new implementations.**

```html
<html data-theme="dark">
```

```css
:root { /* light tokens */ }
[data-theme="dark"] { /* dark tokens */ }
[data-theme="high-contrast"] { /* high-contrast tokens */ }
```

### System preference fallback

Respects OS-level preference. Use as a fallback when no explicit choice is set.

```css
@media (prefers-color-scheme: dark) {
  :root:not([data-theme]) {
    /* dark tokens */
  }
}
```

### JavaScript toggle with persistence

```js
function getStoredTheme() {
  return localStorage.getItem('theme');
}

function getSystemTheme() {
  return window.matchMedia('(prefers-color-scheme: dark)').matches
    ? 'dark'
    : 'light';
}

function applyTheme(theme) {
  document.documentElement.setAttribute('data-theme', theme);
  localStorage.setItem('theme', theme);
}

function initTheme() {
  const stored = getStoredTheme();
  const theme = stored || getSystemTheme();
  applyTheme(theme);
}

function toggleTheme() {
  const current = document.documentElement.getAttribute('data-theme');
  applyTheme(current === 'dark' ? 'light' : 'dark');
}

// Initialize on load
initTheme();

// React to OS preference changes
window.matchMedia('(prefers-color-scheme: dark)')
  .addEventListener('change', (e) => {
    if (!getStoredTheme()) {
      applyTheme(e.matches ? 'dark' : 'light');
    }
  });
```

---

## Automatic Theme Detection

When no explicit theme is stored, match the operating system preference:

```css
@media (prefers-color-scheme: dark) {
  :root:not([data-theme]) {
    --bg: var(--neutral-900);
    --bg-alt: var(--neutral-800);
    --bg-inset: var(--neutral-700);
    --fg: var(--neutral-50);
    --fg-muted: var(--neutral-400);
    --fg-subtle: var(--neutral-500);
    --accent: var(--accent-500);
    --accent-hover: var(--accent-400);
    --accent-press: var(--accent-300);
    --accent-tint: color-mix(in srgb, var(--accent-500) 15%, transparent);
    --on-accent: var(--neutral-900);
    --border: rgba(255, 255, 255, 0.1);
    --border-strong: rgba(255, 255, 255, 0.2);
    --shadow-color: rgba(0, 0, 0, 0.4);
  }
}
```

The `:not([data-theme])` selector ensures the media query only applies when
the user has not made an explicit choice.

---

## Theme Transitions

Smooth visual transition when switching themes. Always respect reduced motion.

```css
html {
  transition:
    background-color 200ms var(--ease),
    color 200ms var(--ease);
}

@media (prefers-reduced-motion: reduce) {
  html {
    transition: none;
  }
}
```

Limit the transition to `background-color` and `color` on the root element.
Component-level transitions add complexity and can cause flicker.

---

## Generating Themes from Context Packet

The skill generates both themes automatically using this process:

1. **Start with light theme as default.** Map raw palette tokens to semantic
   roles following the conventions in `token_schema.md`.

2. **Apply dark mode transformation rules.** Invert surface/foreground
   relationships. Shift accent one step lighter. Use `rgba` for borders and
   tints instead of opaque values.

3. **Verify contrast ratios** for both themes (see below).

4. **Output the complete CSS** with `:root` (light), `[data-theme="dark"]`
   (dark), and the `prefers-color-scheme` media query fallback, all in a
   single `tokens.css` file.

5. **Include the toggle script** inline or as a separate `theme-toggle.js`.

---

## Contrast Verification

Every foreground/background combination must pass WCAG AA in every theme.

| Element type              | Minimum ratio | Notes                              |
|---------------------------|:-------------:|------------------------------------|
| Normal text (<18px)       | 4.5:1         | Body, labels, captions             |
| Large text (>=18px bold or >=24px) | 3:1  | Headings, display text             |
| UI components             | 3:1           | Buttons, inputs, icons             |
| Focus indicators          | 3:1           | Against adjacent color             |

### Pairs to verify

Check these combinations in both light and dark themes:

- `--fg` on `--bg`
- `--fg-muted` on `--bg`
- `--fg-subtle` on `--bg`
- `--on-accent` on `--accent`
- `--fg` on `--bg-alt`
- `--fg` on `--accent-tint`
- `--on-accent` on `--accent-hover`
- `--on-accent` on `--accent-press`
- `--error` on `--bg` (and same for warning, success, info)
- `--focus-ring` on `--bg`

If any pair fails, adjust the raw token values until compliance is met. Do not
ship a theme with failing contrast.

---

## White-Label / Multi-Brand Theming

For platforms that serve multiple brands from one design system (white-label SaaS, customer theming), add a brand-override layer on top of the semantic layer.

### Pattern: `[data-brand]` override layer

```css
/* Base system — all brands inherit these */
:root { /* raw palette + semantic tokens */ }
[data-theme="dark"] { /* dark remaps */ }

/* Brand A override — only semantic token differences */
[data-brand="brand-a"] {
  --accent-500: #E11D48;   /* rose brand accent */
  --accent-400: #F43F5E;
  --accent-300: #FB7185;
  --accent-200: #FFE4E6;
  --accent-600: #BE123C;
  /* Semantic tokens auto-update because they reference --accent-500 */
}

/* Brand B override */
[data-brand="brand-b"] {
  --accent-500: #059669;
  --accent-400: #10B981;
  --accent-300: #34D399;
  --accent-200: #D1FAE5;
  --accent-600: #047857;
}

/* Combine brand + theme: [data-brand="brand-a"][data-theme="dark"] */
[data-brand="brand-a"][data-theme="dark"] {
  /* Brand A dark overrides — usually just --accent-tint adjustment */
  --accent-tint: color-mix(in srgb, var(--accent-500) 15%, transparent);
}
```

### Rules

1. **Override raw tokens only.** Never override semantic tokens directly in brand layers — the raw-to-semantic chain handles propagation automatically.
2. **Document which tokens each brand overrides.** Store brand token sets in a `brands/` directory alongside `tokens.css`.
3. **Test contrast for every brand override.** Each new accent must be validated against `--bg` and `--on-accent` in both themes.
4. **Keep font overrides separate.** If brands use different fonts, load them in brand-specific CSS files and override only `--font-display` and `--font-body`.

### JavaScript for brand + theme combined

```js
function applyBrand(brand) {
  document.documentElement.setAttribute('data-brand', brand);
  localStorage.setItem('brand', brand);
}

function initBrand() {
  const stored = localStorage.getItem('brand');
  if (stored) applyBrand(stored);
}
initBrand();
```
