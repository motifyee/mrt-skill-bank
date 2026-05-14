# Visual Regression Testing

Approach for detecting unintended visual changes across design system iterations.

## Approach

Since design systems are static HTML/CSS, visual regression compares screenshots between versions.

### Quick Method (Manual)

1. Open the previous version of `design-system.html` in a browser
2. Open the new version in an adjacent tab
3. Compare at 1440px and 375px viewport widths
4. Check these sections specifically:
   - Color palette swatches (hue, lightness, saturation)
   - Typography scale (rendered sizes, weights, line heights)
   - Language/script-specific typography behavior (line-height, tracking, fallback stack,
     and visual size across primary/secondary languages)
   - Spacing demonstrations (gap consistency)
   - Component previews (buttons, cards, inputs)

### Automated Method (Playwright)

```js
import { test, expect } from '@playwright/test';

test('design system visual regression', async ({ page }) => {
  await page.goto('./design-system.html');

  // Full page comparison
  await expect(page).toHaveScreenshot('design-system.png', {
    maxDiffPixelRatio: 0.01,  // Allow 1% pixel difference
    threshold: 0.2,
  });

  // Component-level comparisons
  const buttons = page.locator('#buttons-section');
  await expect(buttons).toHaveScreenshot('buttons.png');
});
```

### What to Flag

| Change Type | Expected? | Action |
|-------------|-----------|--------|
| Token value change | If you edited tokens | Verify intentionality |
| Layout shift | Never | Investigate CSS regression |
| Font rendering change | If you changed fonts | Verify fallback stack |
| Script rendering change | If language/script strategy changed | Verify line-height, tracking, glyph coverage, and page-part roles |
| Color shift | If you changed palette | Check contrast still passes |
| Dark mode difference | If you changed dark tokens | Re-run contrast checks |

## Baseline Management

- Store baseline screenshots in `preview/baselines/`
- Name format: `{section}-{viewport}.png`
- Update baselines only after intentional changes are verified
- Never update baselines to match a regression

## Passing Threshold

- **PASS**: Zero unexpected visual changes
- **PASS WITH WARNINGS**: 1-2 minor spacing differences (< 2px)
- **FAIL**: Any unintended color, font, script-rendering, or layout change
