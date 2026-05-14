# Performance Budgets

Thresholds for generated design system output.

## File Size Budgets

| File | Budget | Action if Exceeded |
|------|--------|-------------------|
| `tokens.css` | 15 KB | Trim comments, check for duplicate tokens |
| `design-system.html` | 100 KB | Reduce inline demo content, use fewer swatches |
| Preview HTML (each) | 20 KB | Remove redundant markup |
| UI Kit HTML (each) | 50 KB | Reduce component variants shown |

## Font Loading Budgets

| Metric | Budget |
|--------|--------|
| Total font file size | 300 KB |
| Number of font families | 3 maximum (display + body + mono) |
| Number of font weights | 4 maximum per family |
| First contentful paint impact | < 200ms from font load |

### Optimization Checklist

- [ ] Using variable fonts where available (single file, multiple weights)
- [ ] `font-display: swap` on all `@font-face` declarations
- [ ] Critical fonts preloaded with `<link rel="preload">`
- [ ] No unused font weights loaded
- [ ] Consider system-fonts-only mode for performance-critical contexts

## CSS Performance

| Metric | Budget |
|--------|--------|
| Total custom properties | < 200 |
| CSS selectors | < 100 |
| `@import` statements | 0 in `tokens.css`; use HTML `<link>` or bundled fonts |
| `!important` usage | 0 |

## Runtime Performance

| Metric | Budget |
|--------|--------|
| First paint | < 500ms (local file) |
| Cumulative layout shift | < 0.1 |
| Dark mode toggle response | < 50ms |

## Lighthouse Thresholds

Run against `design-system.html` served locally:

```
Performance: >= 95
Accessibility: >= 90
Best Practices: >= 95
```

## Passing Threshold

- **PASS**: All budgets met
- **PASS WITH WARNINGS**: 1 budget exceeded by < 20%
- **FAIL**: Any budget exceeded by > 20%, or Accessibility < 85
