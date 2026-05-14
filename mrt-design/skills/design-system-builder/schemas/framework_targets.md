# Framework Targets

Maps design tokens to component implementations for React, Vue, Astro, and
Svelte. Every framework consumes the same `tokens.css` file.

---

## Token-to-CSS Mapping (Universal)

All frameworks import the same CSS file. Tokens are framework-agnostic.

```css
/* tokens.css — load via <link> before any component CSS */
:root {
  /* Raw + semantic tokens from token_schema.md */
  /* Theming overrides from theming_schema.md */
}
```

```html
<!-- Any framework: include once in the document head -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="stylesheet" href="[font_stylesheet_url]">
<link rel="stylesheet" href="tokens.css">
```

```js
// Or import from JS (Vite, Webpack, etc.)
import './tokens.css';
```

---

## React Components

### Button

```tsx
import type { ButtonHTMLAttributes } from 'react';

type ButtonVariant = 'primary' | 'secondary' | 'ghost' | 'destructive';
type ButtonSize = 'sm' | 'md' | 'lg';

interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: ButtonVariant;
  size?: ButtonSize;
}

export function Button({
  variant = 'primary',
  size = 'md',
  className = '',
  children,
  ...rest
}: ButtonProps) {
  return (
    <button
      className={`btn btn-${variant} btn-${size}${className ? ` ${className}` : ''}`}
      {...rest}
    >
      {children}
    </button>
  );
}
```

### File structure

```
src/
  components/
    tokens.css          (import first)
    Button.tsx
    Card.tsx
    Input.tsx
    Badge.tsx
    Modal.tsx
    Nav.tsx
    index.ts            (barrel export)
```

### Barrel export

```ts
// index.ts
export { Button } from './Button';
export { Card } from './Card';
export { Input } from './Input';
export { Badge } from './Badge';
export { Modal } from './Modal';
export { Nav } from './Nav';
```

### CSS for React components

```css
/* components.css — imported after tokens.css */
.btn {
  display: inline-flex;
  align-items: center;
  gap: var(--space-2);
  font-family: var(--font-body);
  font-weight: var(--fw-semibold);
  border-radius: var(--radius-md);
  border: 1px solid transparent;
  cursor: pointer;
  transition:
    background-color var(--dur-fast) var(--ease),
    border-color var(--dur-fast) var(--ease),
    box-shadow var(--dur-fast) var(--ease);
}

.btn-sm  { padding-block: var(--space-1); padding-inline: var(--space-3); font-size: var(--fs-body-sm); }
.btn-md  { padding-block: var(--space-2); padding-inline: var(--space-5); font-size: var(--fs-body); }
.btn-lg  { padding-block: var(--space-3); padding-inline: var(--space-6); font-size: var(--fs-body-lg); }

.btn-primary {
  background-color: var(--accent);
  color: var(--on-accent);
}
.btn-primary:hover { background-color: var(--accent-hover); }
.btn-primary:active { background-color: var(--accent-press); }

.btn-secondary {
  background-color: transparent;
  color: var(--fg);
  border-color: var(--border-strong);
}
.btn-secondary:hover { background-color: var(--bg-alt); }

.btn-ghost {
  background-color: transparent;
  color: var(--fg-muted);
}
.btn-ghost:hover { background-color: var(--bg-alt); color: var(--fg); }

.btn-destructive {
  background-color: var(--error);
  color: white;
}
.btn-destructive:hover { background-color: var(--error); filter: brightness(1.1); }

.btn:focus-visible {
  outline: 3px solid var(--focus-ring);
  outline-offset: 2px;
}

.btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
```

---

## Vue Components

### Button

```vue
<template>
  <button
    :class="['btn', `btn-${variant}`, `btn-${size}`, extraClass]"
    v-bind="$attrs"
  >
    <slot />
  </button>
</template>

<script setup lang="ts">
interface Props {
  variant?: 'primary' | 'secondary' | 'ghost' | 'destructive';
  size?: 'sm' | 'md' | 'lg';
  extraClass?: string;
}

withDefaults(defineProps<Props>(), {
  variant: 'primary',
  size: 'md',
  extraClass: '',
});
</script>
```

### File structure

```
src/
  components/
    tokens.css
    Button.vue
    Card.vue
    Input.vue
    Badge.vue
    Modal.vue
    Nav.vue
    index.ts
```

### Barrel export

```ts
// index.ts
export { default as Button } from './Button.vue';
export { default as Card } from './Card.vue';
export { default as Input } from './Input.vue';
export { default as Badge } from './Badge.vue';
export { default as Modal } from './Modal.vue';
export { default as Nav } from './Nav.vue';
```

The CSS file (`components.css`) is identical to the React version. Vue SFCs
consume the same classes.

---

## Astro Components

### Button

```astro
---
interface Props {
  variant?: 'primary' | 'secondary' | 'ghost' | 'destructive';
  size?: 'sm' | 'md' | 'lg';
}

const {
  variant = 'primary',
  size = 'md',
} = Astro.props;
---

<button class:list={['btn', `btn-${variant}`, `btn-${size}`]}>
  <slot />
</button>
```

### File structure

```
src/
  components/
    tokens.css
    Button.astro
    Card.astro
    Input.astro
    Badge.astro
    Modal.astro
    Nav.astro
    index.ts
```

Astro's `class:list` directive handles conditional classes. Components use the
same CSS token references as React and Vue.

---

## Svelte Components

### Button

```svelte
<script lang="ts">
  interface Props {
    variant?: 'primary' | 'secondary' | 'ghost' | 'destructive';
    size?: 'sm' | 'md' | 'lg';
    class?: string;
  }

  let {
    variant = 'primary',
    size = 'md',
    class: className = '',
  }: Props = $props();
</script>

<button class="btn btn-{variant} btn-{size} {className}">
  {@render children()}
</button>
```

### File structure

```
src/
  components/
    tokens.css
    Button.svelte
    Card.svelte
    Input.svelte
    Badge.svelte
    Modal.svelte
    Nav.svelte
    index.ts
```

Svelte 5 uses `$props()` and `{@render children()}`. The component CSS classes
are identical across all frameworks.

---

## Framework Selection Guide

| Framework        | Best for                                                | Bundle cost |
|------------------|---------------------------------------------------------|:-----------:|
| Plain HTML/CSS   | Single-page prototypes, email templates, static sites   | Zero        |
| Astro            | Content-heavy marketing sites, blogs, documentation     | Minimal     |
| React            | SPAs, dashboards, complex interactive UIs               | Medium      |
| Vue              | Progressive enhancement, admin panels, existing Vue apps | Medium   |
| Svelte           | Performance-critical apps, small bundles, embedded widgets | Low      |

### Decision rules

- If the project is a static site with no interactivity, use plain HTML.
- If content is primary and interactivity is sparse, use Astro.
- If the project is a complex SPA with heavy state management, use React.
- If the team already uses Vue or needs gradual adoption, use Vue.
- If bundle size and runtime performance are critical, use Svelte.

---

## Generation Rules

When generating framework-specific output, follow this order:

1. **Generate `tokens.css` first.** This file is framework-agnostic and
   contains all raw tokens, semantic tokens, and theme overrides. Every
   framework imports it.

2. **Map component patterns to the chosen framework's conventions.** Use
   hooks for React, composition API for Vue, SFC structure for Astro,
   and `$props()` for Svelte 5.

3. **Use native patterns.** React uses `className`, Vue uses `:class`,
   Astro uses `class:list`, Svelte uses `class=`. Do not mix conventions.

4. **Include a `package.json`** when generating a full component library:
   ```json
   {
     "name": "@brand/ui",
     "version": "1.0.0",
     "type": "module",
     "exports": {
       ".": "./index.ts",
       "./tokens.css": "./tokens.css",
       "./components.css": "./components.css"
     }
   }
   ```

5. **Generate TypeScript types for all component props.** Every component
   must have typed props with defaults documented.

---

## CSS-in-JS Alternatives

### Tailwind CSS config

Map design tokens to a Tailwind configuration for teams using the utility
framework.

```js
// tailwind.config.js
import designTokens from './tokens.json'; // exported from tokens.css

export default {
  theme: {
    colors: {
      bg: 'var(--bg)',
      'bg-alt': 'var(--bg-alt)',
      fg: 'var(--fg)',
      'fg-muted': 'var(--fg-muted)',
      accent: 'var(--accent)',
      'accent-hover': 'var(--accent-hover)',
      'on-accent': 'var(--on-accent)',
      border: 'var(--border)',
      success: 'var(--success)',
      warning: 'var(--warning)',
      error: 'var(--error)',
      info: 'var(--info)',
    },
    fontFamily: {
      display: ['var(--font-display)'],
      body: ['var(--font-body)'],
      mono: ['var(--font-mono)'],
    },
    fontSize: {
      display: 'var(--fs-display)',
      h1: 'var(--fs-h1)',
      h2: 'var(--fs-h2)',
      h3: 'var(--fs-h3)',
      h4: 'var(--fs-h4)',
      body: 'var(--fs-body)',
      sm: 'var(--fs-body-sm)',
      label: 'var(--fs-label)',
    },
    spacing: {
      1: 'var(--space-1)',
      2: 'var(--space-2)',
      3: 'var(--space-3)',
      4: 'var(--space-4)',
      5: 'var(--space-5)',
      6: 'var(--space-6)',
      7: 'var(--space-7)',
      8: 'var(--space-8)',
    },
    borderRadius: {
      sm: 'var(--radius-sm)',
      md: 'var(--radius-md)',
      lg: 'var(--radius-lg)',
      xl: 'var(--radius-xl)',
      full: 'var(--radius-full)',
    },
  },
};
```

### Styled-components / Emotion theme object

```ts
// theme.ts
const theme = {
  colors: {
    bg: 'var(--bg)',
    bgAlt: 'var(--bg-alt)',
    fg: 'var(--fg)',
    fgMuted: 'var(--fg-muted)',
    accent: 'var(--accent)',
    accentHover: 'var(--accent-hover)',
    onAccent: 'var(--on-accent)',
    border: 'var(--border)',
    success: 'var(--success)',
    warning: 'var(--warning)',
    error: 'var(--error)',
    info: 'var(--info)',
  },
  fonts: {
    display: 'var(--font-display)',
    body: 'var(--font-body)',
    mono: 'var(--font-mono)',
  },
  space: {
    1: 'var(--space-1)',
    2: 'var(--space-2)',
    3: 'var(--space-3)',
    4: 'var(--space-4)',
    5: 'var(--space-5)',
    6: 'var(--space-6)',
    7: 'var(--space-7)',
    8: 'var(--space-8)',
  },
  radii: {
    sm: 'var(--radius-sm)',
    md: 'var(--radius-md)',
    lg: 'var(--radius-lg)',
    xl: 'var(--radius-xl)',
  },
  easing: 'var(--ease)',
  duration: {
    fast: 'var(--dur-fast)',
    base: 'var(--dur-base)',
    slow: 'var(--dur-slow)',
  },
} as const;

export default theme;
```

### Universal fallback

CSS custom properties work everywhere: static HTML, server-rendered pages,
and any JavaScript framework. When in doubt, output `tokens.css` with
semantic HTML classes. This is the lowest-common-denominator approach that
every framework can consume without build steps.

```css
/* Universal: no framework, no build step, no runtime */
/* In HTML: <link rel="stylesheet" href="tokens.css"> */

.card {
  background: var(--bg-alt);
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  padding-inline: var(--space-5);
  padding-block: var(--space-5);
}

.card-title {
  font-family: var(--font-display);
  font-size: var(--fs-h3);
  font-weight: var(--fw-semibold);
  color: var(--fg);
}
```
