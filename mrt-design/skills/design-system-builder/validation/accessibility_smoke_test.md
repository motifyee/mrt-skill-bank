# Accessibility Smoke Test

Quick WCAG 2.1 AA checks for generated design system output. Run these in under 5 minutes.

## Automated Checks (axe-core)

Add to any page using the design system:

```js
// In browser console after loading design-system.html or any preview
const results = await axe.run();
console.log(`Violations: ${results.violations.length}`);
results.violations.forEach(v => console.log(v.id, v.description));
```

### Required Pass Criteria

| Check | Standard | Severity |
|-------|----------|----------|
| Color contrast (text) | 4.5:1 minimum for normal text | Critical |
| Color contrast (large text) | 3:1 minimum for 18px+ bold or 24px+ | Critical |
| Color contrast (UI components) | 3:1 minimum against adjacent colors | Major |
| Image alt text | All `<img>` have meaningful alt | Major |
| Form labels | All inputs have associated labels | Major |
| Heading hierarchy | No skipped levels (h1 -> h3) | Minor |
| Focus visible | All interactive elements show focus ring | Critical |
| Keyboard navigation | All interactive elements reachable via Tab | Critical |

## Manual Spot Checks

1. **Tab through the dashboard** — every button, link, and control must be reachable
2. **Zoom to 200%** — nothing should overflow or become unusable
3. **Check dark mode toggle** — contrast must remain valid in both modes
4. **Screen reader announcement** — headings, buttons, and status changes must announce

## Generated Output Checklist

For the generated `design-system.html`:

- [ ] All color swatches show hex value text with sufficient contrast against the swatch background
- [ ] Font size samples include the actual size in the label
- [ ] Spacing scale is presented in a table (not just colored boxes)
- [ ] Component previews have visible focus indicators
- [ ] Dark mode preview maintains all contrast ratios

## Passing Threshold

- **PASS**: Zero Critical violations, max 2 Minor
- **PASS WITH WARNINGS**: Zero Critical, 3-5 Minor
- **FAIL**: Any Critical violation
