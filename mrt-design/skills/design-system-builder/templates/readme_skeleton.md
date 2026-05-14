# [BRAND] Design System

Version [VERSION] | Generated [DATE]

## Quick Start

```html
<link rel="stylesheet" href="tokens.css">
```

## Tokens

### Colors

| Token | Value | Preview |
|-------|-------|---------|
| `--neutral-900` | [HEX] | [swatch] |
| `--neutral-50` | [HEX] | [swatch] |
| `--accent-500` | [HEX] | [swatch] |

### Typography

Primary language/script: [LANGUAGE] / [SCRIPT]

Typography spirit: [luxury | comic/playful | friendly | beauty/editorial | technical | authority | calm | craft | custom]

| Role | Font | Expresses | Use For | Avoid For |
|------|------|-----------|---------|-----------|
| Display | [FONT] | [VALUES] | Hero, headings | [CONSTRAINTS] |
| Body | [FONT] | [VALUES] | Body, UI text | [CONSTRAINTS] |
| Mono | [FONT] | [VALUES] | Code, data | [CONSTRAINTS] |

| Level | Size | Weight | Line Height |
|-------|------|--------|-------------|
| Display | [VALUE] | [VALUE] | [VALUE] |
| H1 | [VALUE] | [VALUE] | [VALUE] |
| Body | [VALUE] | [VALUE] | [VALUE] |

### Spacing

| Token | Value |
|-------|-------|
| `--space-1` | [VALUE] |
| `--space-4` | [VALUE] |
| `--space-8` | [VALUE] |

## Usage

### Framework Integration

See `framework_targets.md` for React, Vue, Svelte, Astro setup.

### Dark Mode

Automatic via `prefers-color-scheme: dark`. Manual toggle with `data-theme="dark"`.

## Files

| File | Purpose |
|------|---------|
| `tokens.css` | All design tokens (raw + semantic + component) |
| `design-system.html` | Visual dashboard of all tokens |
| `preview/` | Individual token category previews |
| `ui_kits/` | Framework-specific component implementations |

## Changelog

### [VERSION] — [DATE]

- Initial generation
