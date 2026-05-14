# Data Visualization

Chart colors, dashboard grid patterns, KPI card design, and data presentation
principles. All patterns use the design token system for consistency.

## Design Principles

1. **Bind chart colors to CSS tokens:** Map every series color, semantic hue, and gradient stop to a CSS variable so theme switching (light/dark/brand) propagates to all charts from one place.
2. **Avoid red/green as the only differentiator:** Use an 8-hue categorical palette tuned for color-blind safety and pair every color-coded data point with a label or pattern.
3. **Match skeleton shape to chart type:** Render skeleton placeholders that echo the chart's form (bars at varying heights for bar charts, subtle waves for line charts) rather than a generic gray rectangle.
4. **KPI cards answer one question each:** Show a single metric, its label, and a directional change indicator; never crowd a KPI card with secondary charts or footnotes.

---

## Chart Color Palettes

### Sequential Palette (Single-metric Gradients)

For heatmaps, choropleth maps, and single-variable intensity displays. Bind
to CSS variables so themes can remap values.

```css
:root {
  --seq-1: #f0f4ff;
  --seq-2: #c7d7fe;
  --seq-3: #8da4f8;
  --seq-4: #5b7cf5;
  --seq-5: #3b5bdb;
  --seq-6: #2b4ac7;
  --seq-7: #1e3a8a;
}
```

### Categorical Palette (Multi-series Charts)

Eight distinct hues tuned for color-blind safety. Avoid relying solely on
red/green differentiation.

```css
:root {
  --cat-1: #3b82f6;  /* Blue */
  --cat-2: #f59e0b;  /* Amber */
  --cat-3: #10b981;  /* Emerald */
  --cat-4: #8b5cf6;  /* Violet */
  --cat-5: #ef4444;  /* Red */
  --cat-6: #06b6d4;  /* Cyan */
  --cat-7: #f97316;  /* Orange */
  --cat-8: #ec4899;  /* Pink */
}
```

### Semantic Palette (Positive/Negative/Neutral)

```css
:root {
  --sem-positive: #10b981;  /* Green - growth, success */
  --sem-negative: #ef4444;  /* Red - decline, error */
  --sem-neutral:  #6b7280;  /* Gray - no change */
  --sem-warning:  #f59e0b;  /* Amber - caution */
}
```

Bind chart colors to tokens so themes override once:

```css
.chart-series-1 { fill: var(--cat-1); }
.chart-series-2 { fill: var(--cat-2); }
.chart-series-3 { fill: var(--cat-3); }
```

---

## Dashboard Grid System

```
+--------------------------------------------------+
|  [Sidebar]  |  [Header Bar]                       |
|             |-------------------------------------|
|             |  [KPI] [KPI] [KPI] [KPI]            |
|             |-------------------------------------|
|             |  [Main Chart - 2/3 width]  [Side]   |
|             |                            [Panel]  |
|             |-------------------------------------|
|             |  [Table - full width]                |
+--------------------------------------------------+
```

```css
.dashboard {
  display: grid;
  grid-template-columns: 240px 1fr;
  grid-template-rows: 56px 1fr;
  grid-template-areas:
    "sidebar header"
    "sidebar content";
  height: 100vh;
}

.dashboard__header {
  grid-area: header;
  border-bottom: 1px solid var(--border);
  padding: 0 var(--space-6);
  display: flex;
  align-items: center;
  gap: var(--space-4);
}

.dashboard__sidebar {
  grid-area: sidebar;
  border-right: 1px solid var(--border);
  background: var(--bg-alt);
  overflow-y: auto;
}

.dashboard__content {
  grid-area: content;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: var(--space-5);
  padding: var(--space-6);
  overflow-y: auto;
  align-content: start;
}
```

Content area regions using named spans:

```css
.dashboard__main-chart {
  grid-column: 1 / 3;
}
.dashboard__side-panel {
  grid-column: 3 / 5;
}
.dashboard__table {
  grid-column: 1 / -1;
}
```

Collapse sidebar on screens below 1024px:

```css
/* Base: single-column layout, sidebar hidden */
.dashboard {
  grid-template-columns: 1fr;
  grid-template-rows: 56px 1fr;
  grid-template-areas: "header" "content";
}
.dashboard__sidebar { display: none; }
.dashboard__content { grid-template-columns: 1fr; }

@media (min-width: 641px) {
  .dashboard__content { grid-template-columns: repeat(2, 1fr); }
}

@media (min-width: 1025px) {
  .dashboard {
    grid-template-columns: 240px 1fr;
    grid-template-rows: 56px 1fr;
    grid-template-areas:
      "sidebar header"
      "sidebar content";
  }
  .dashboard__sidebar { display: block; }
}
```

---

## KPI Cards

```css
.kpi-card {
  padding: var(--space-5);
  background: var(--bg);
  border: 1px solid var(--border);
  border-radius: var(--radius-md);
  display: flex;
  flex-direction: column;
  gap: var(--space-2);
}
.kpi-value {
  font-family: var(--font-display);
  font-size: var(--fs-h3);
  font-weight: var(--fw-bold);
  color: var(--fg);
  line-height: 1.2;
}
.kpi-label {
  font-size: var(--fs-label);
  color: var(--fg-muted);
  text-transform: uppercase;
  letter-spacing: 0.05em;
}
.kpi-change {
  font-size: var(--fs-body-sm);
  display: inline-flex;
  align-items: center;
  gap: var(--space-1);
}
.kpi-change.positive { color: var(--sem-positive); }
.kpi-change.negative { color: var(--sem-negative); }
.kpi-change.neutral  { color: var(--sem-neutral); }
```

```html
<div class="kpi-card">
  <span class="kpi-label">Monthly Revenue</span>
  <span class="kpi-value">$48,200</span>
  <span class="kpi-change positive">+12.5% vs last month</span>
</div>
```

---

## Data Table Patterns

### Sortable Headers

```css
.table th[aria-sort] {
  cursor: pointer;
  user-select: none;
  padding-right: var(--space-6);
  position: relative;
}
.table th[aria-sort]::after {
  content: '';
  position: absolute;
  right: var(--space-2);
  top: 50%;
  transform: translateY(-50%);
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
}
.table th[aria-sort="ascending"]::after {
  border-bottom: 6px solid var(--fg-muted);
}
.table th[aria-sort="descending"]::after {
  border-top: 6px solid var(--fg-muted);
}
```

### Row Selection

```css
.table-row--selected {
  background: color-mix(in srgb, var(--accent) 8%, var(--bg));
}
.table input[type="checkbox"] {
  accent-color: var(--accent);
  width: 16px;
  height: 16px;
}
```

### Pagination Bar

```css
.pagination {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--space-3) var(--space-4);
  border-top: 1px solid var(--border);
  font-size: var(--fs-body-sm);
  color: var(--fg-muted);
}
.pagination__pages {
  display: flex;
  gap: var(--space-1);
}
.pagination__btn {
  min-width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: var(--radius-sm);
  border: 1px solid var(--border);
  background: var(--bg);
  color: var(--fg);
  cursor: pointer;
}
.pagination__btn--active {
  background: var(--accent);
  color: var(--on-accent);
  border-color: var(--accent);
}
```

### Responsive: Card View on Mobile

Below 640px, flatten table rows into stacked cards.

```css
/* Base: card view for mobile */
.table thead { display: none; }
.table-row {
  display: block;
  padding: var(--space-4);
  border-bottom: 1px solid var(--border);
}
.table-cell {
  display: flex;
  justify-content: space-between;
  padding: var(--space-1) 0;
}
.table-cell::before {
  content: attr(data-label);
  font-weight: var(--fw-semibold);
  color: var(--fg-muted);
  font-size: var(--fs-label);
}

@media (min-width: 641px) {
  .table thead { display: table-header-group; }
  .table-row {
    display: table-row;
    padding: 0;
  }
  .table-cell {
    display: table-cell;
    padding: var(--space-3) var(--space-4);
  }
  .table-cell::before {
    content: none;
  }
}
```

---

## Chart Typography

Consistent text styling across all chart types.

| Element        | Token              | Color           |
|----------------|--------------------|-----------------|
| Axis labels    | `var(--fs-label)`  | `var(--fg-muted)` |
| Data labels    | `var(--fs-body-sm)`| `var(--fg)`     |
| Chart titles   | `var(--fs-h4)`     | `var(--fg)`     |
| Tooltips       | `var(--fs-body-sm)`| `var(--bg)` on dark bg |
| Legends        | `var(--fs-body-sm)`| `var(--fg-muted)` |

Chart tooltip styling:

```css
.chart-tooltip {
  padding: var(--space-2) var(--space-3);
  background: var(--fg);
  color: var(--bg);
  font-size: var(--fs-body-sm);
  border-radius: var(--radius-sm);
  box-shadow: var(--shadow-md);
  pointer-events: none;
  white-space: nowrap;
}
```

---

## Empty and Error States for Data

### No Data

Show the chart area with a centered message instead of an empty canvas.

```css
.chart-empty {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 200px;
  color: var(--fg-muted);
  font-size: var(--fs-body);
  gap: var(--space-2);
}
```

### Loading (Skeleton Chart)

Match the chart shape with a skeleton placeholder. For a bar chart, render
gray rectangles at varying heights. For a line chart, render a subtle wave shape.

### Error

Display the chart area with an error message and a retry CTA.

```html
<div class="chart-empty">
  <p>Unable to load chart data.</p>
  <button class="btn btn-secondary">Retry</button>
</div>
```

### Partial Data

Show available data points with dashed lines or lighter opacity for missing
segments. Use `var(--fg-muted)` at reduced opacity for gaps and full `var(--fg)`
for available data.
