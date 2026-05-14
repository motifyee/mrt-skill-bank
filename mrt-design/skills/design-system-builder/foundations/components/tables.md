## Design Principles

- **Tables are for structured data comparison, not layout grids.** If the content is not tabular data with shared columns, use cards or a list instead.
- **Header rows are visually distinct from data rows.** Uppercase labels, muted color, heavier bottom border, and a different font weight separate headers from content at a glance.
- **Row hover highlights the entire record.** A subtle background change on `tr:hover` helps users track which cells belong to the same row across wide tables.
- **Responsive tables degrade gracefully.** On narrow viewports, provide horizontal scroll with sticky first column, or refactor into a card-list view rather than shrinking fonts.
- **Column alignment follows data type.** Text aligns to `start`, numbers align to `end`, and status/badge columns center-align for easy scanning.

## Brand Expression

Tables are utility-first, but they still express the system through density,
selection, focus, and dividers. Apply `creative_brief` through restrained details:

- `safe`: standard row borders, clear hover, no decorative effects.
- `elevated`: branded selected-row rail, accent focus ring, or signature divider.
- `bold`: sticky columns, grouped row bands, or data-as-ornament backgrounds if scanability remains high.
- `experimental`: only for non-critical analytical views; never hide values behind motion.

Use `character_rules.inputs` and `components.component_style_contract.inputs`
for filters/search inside table toolbars. Use `signature_dna` for selected rows,
active sort headers, or chart/table focus states.

### Component Tokens

```css
.table {
  --table-header-bg: transparent;
  --table-header-color: var(--fg-muted);
  --table-header-border: 2px solid var(--border);
  --table-row-border: 1px solid var(--border);
  --table-row-hover-bg: var(--bg-alt);
  --table-row-selected-bg: var(--accent-tint);
  --table-row-selected-border: var(--accent);
  --table-cell-padding: var(--space-3) var(--space-4);
  --table-cell-font: var(--fs-body-sm);
  --table-sort-icon-color: var(--fg-muted);
  --table-sort-icon-active-color: var(--accent);
  --table-sticky-z: 2;
}
```

### CSS

```css
.table-wrapper {
  width: 100%;
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
}

.table {
  width: 100%;
  border-collapse: collapse;
  font-family: var(--font-body);
  font-size: var(--table-cell-font);
}

.table th {
  text-align: start;
  padding: var(--table-cell-padding);
  font-family: var(--font-display);
  font-weight: var(--fw-semibold);
  font-size: var(--fs-label);
  text-transform: uppercase;
  letter-spacing: 0.06em;
  color: var(--table-header-color);
  border-bottom: var(--table-header-border);
  background: var(--table-header-bg);
  white-space: nowrap;
  position: sticky;
  top: 0;
  z-index: var(--table-sticky-z);
}

.table td {
  padding: var(--table-cell-padding);
  border-bottom: var(--table-row-border);
  color: var(--fg);
}

/* Row hover */
.table tbody tr:hover td {
  background: var(--table-row-hover-bg);
}

/* Numeric columns */
.table td[data-type="number"],
.table th[data-type="number"] {
  text-align: end;
  font-variant-numeric: tabular-nums;
}

/* Status / badge columns */
.table td[data-type="status"],
.table th[data-type="status"] {
  text-align: center;
}
```

### Sortable Headers

```html
<thead>
  <tr>
    <th scope="col" aria-sort="none">
      <button class="table__sort" type="button">
        Name
        <svg class="table__sort-icon" aria-hidden="true" width="14" height="14" viewBox="0 0 14 14">
          <path d="M7 2l4 4H3z" fill="currentColor" opacity="0.4"/>
          <path d="M7 12l4-4H3z" fill="currentColor" opacity="0.4"/>
        </svg>
      </button>
    </th>
    <th scope="col" aria-sort="ascending">
      <button class="table__sort table__sort--asc" type="button">
        Date
        <svg class="table__sort-icon" aria-hidden="true" width="14" height="14" viewBox="0 0 14 14">
          <path d="M7 2l4 4H3z" fill="currentColor"/>
          <path d="M7 12l4-4H3z" fill="currentColor" opacity="0.25"/>
        </svg>
      </button>
    </th>
  </tr>
</thead>
```

```css
.table__sort {
  display: inline-flex;
  align-items: center;
  gap: var(--space-1);
  background: none;
  border: none;
  padding: 0;
  color: inherit;
  font: inherit;
  cursor: pointer;
  transition: color var(--dur-fast) var(--ease);
}
.table__sort:hover {
  color: var(--fg);
}
.table__sort:focus-visible {
  outline: 2px solid var(--focus-ring);
  outline-offset: 2px;
  border-radius: var(--radius-sm);
}
.table__sort-icon {
  color: var(--table-sort-icon-color);
}
.table__sort--asc .table__sort-icon,
.table__sort--desc .table__sort-icon {
  color: var(--table-sort-icon-active-color);
}
```

### Row Selection

```html
<table class="table">
  <thead>
    <tr>
      <th scope="col" class="table__select-col">
        <input type="checkbox"
               class="table__select-all"
               aria-label="Select all rows" />
      </th>
      <th scope="col">Name</th>
      <th scope="col">Status</th>
    </tr>
  </thead>
  <tbody>
    <tr class="table__row--selected">
      <td class="table__select-col">
        <input type="checkbox"
               class="table__select-row"
               checked
               aria-label="Select row: Jane Doe" />
      </td>
      <td>Jane Doe</td>
      <td><span class="badge badge--success">Active</span></td>
    </tr>
  </tbody>
</table>
```

```css
.table__select-col {
  width: 48px;
  text-align: center;
}

/* Selected row */
.table__row--selected td {
  background: var(--table-row-selected-bg);
  border-inline-start: 3px solid var(--table-row-selected-border);
}

.table__row--selected:hover td {
  background: color-mix(in srgb, var(--table-row-selected-bg) 80%, var(--table-row-hover-bg));
}
```

### Bulk Action Toolbar

When rows are selected, a toolbar appears above or below the table:

```html
<div class="table-toolbar" role="toolbar" aria-label="Bulk actions">
  <span class="table-toolbar__count">3 selected</span>
  <div class="table-toolbar__actions">
    <button class="btn btn-sm btn-secondary" type="button">Export</button>
    <button class="btn btn-sm btn-destructive" type="button">Delete</button>
  </div>
</div>
```

```css
.table-toolbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--space-3) var(--space-4);
  background: var(--bg-alt);
  border-radius: var(--radius-md) var(--radius-md) 0 0;
  border: 1px solid var(--border);
  border-block-end: none;
}
.table-toolbar__count {
  font-family: var(--font-display);
  font-size: var(--fs-body-sm);
  font-weight: var(--fw-medium);
  color: var(--fg);
}
.table-toolbar__actions {
  display: flex;
  gap: var(--space-2);
}
```

### Sticky First Column

For horizontally scrollable tables with many columns:

```css
.table--sticky-first .td:first-child,
.table--sticky-first .th:first-child {
  position: sticky;
  inset-inline-start: 0;
  z-index: calc(var(--table-sticky-z) + 1);
  background: var(--bg);
}

/* Clip shadow for scrolling context */
.table--sticky-first .td:first-child::after,
.table--sticky-first .th:first-child::after {
  content: '';
  position: absolute;
  inset: 0;
  inset-inline-start: auto;
  width: 6px;
  background: linear-gradient(to end, rgba(0,0,0,0.06), transparent);
  opacity: 0;
  transition: opacity var(--dur-fast) var(--ease);
}
.table--sticky-first td:first-child::after {
  opacity: 1;
}
```

### Table States

```html
<!-- Loading: skeleton rows -->
<tbody aria-busy="true">
  <tr class="table__row--skeleton" aria-hidden="true">
    <td><div class="skeleton skeleton--text" style="width:60%"></div></td>
    <td><div class="skeleton skeleton--text" style="width:40%"></div></td>
    <td><div class="skeleton skeleton--text" style="width:25%"></div></td>
  </tr>
  <!-- repeat for ~5 rows -->
</tbody>

<!-- Empty state -->
<tbody>
  <tr>
    <td colspan="3" class="table__empty">
      <div class="table__empty-content">
        <p class="table__empty-title">No records found</p>
        <p class="table__empty-description">Try adjusting your filters or add a new record.</p>
        <button class="btn btn-sm btn-primary" type="button">Add Record</button>
      </div>
    </td>
  </tr>
</tbody>

<!-- Error state -->
<tbody>
  <tr>
    <td colspan="3" class="table__error">
      <div role="alert" class="alert alert--error alert--inline">
        Failed to load data. <button class="btn btn-ghost btn-sm" type="button">Retry</button>
      </div>
    </td>
  </tr>
</tbody>
```

```css
.table__empty,
.table__error {
  text-align: center;
  padding: var(--space-8) var(--space-4);
}
.table__empty-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--space-3);
  max-width: 320px;
  margin-inline: auto;
}
.table__empty-title {
  font-family: var(--font-display);
  font-size: var(--fs-body);
  font-weight: var(--fw-semibold);
  color: var(--fg);
  margin: 0;
}
.table__empty-description {
  font-size: var(--fs-body-sm);
  color: var(--fg-muted);
  margin: 0;
}
```

### Size Variants

```css
/* Compact — admin panels, data-dense views */
.table--compact {
  --table-cell-padding: var(--space-2) var(--space-3);
  --table-cell-font: var(--fs-label);
}

/* Default — standard data display */
/* Uses base tokens */

/* Spacious — public-facing tables, comfortable reading */
.table--spacious {
  --table-cell-padding: var(--space-4) var(--space-5);
  --table-cell-font: var(--fs-body);
}
```

### Dark Mode

```css
@media (prefers-color-scheme: dark) {
  .table {
    --table-header-bg: var(--bg-elevated);
    --table-header-color: var(--fg-muted);
    --table-header-border: 2px solid var(--border-subtle);
    --table-row-border: 1px solid var(--border-subtle);
    --table-row-hover-bg: rgba(255, 255, 255, 0.04);
    --table-row-selected-bg: color-mix(in srgb, var(--accent) 10%, transparent);
  }
  .table--sticky-first .td:first-child,
  .table--sticky-first .th:first-child {
    background: var(--bg-elevated);
  }
}
```

### Responsive Behavior

- **Horizontal scroll** (preferred for comparison-heavy tables): wrap table in `.table-wrapper` with `overflow-x: auto`. Sticky first column keeps the identifier visible.
- **Card-list refactor** (for narrow CRUD/admin views): hide `<table>` below a breakpoint and show card-based row summaries instead.
- **Preserve numeric alignment** and tabular numerals in all modes.
- **Minimum column width**: set `min-width` on `<th>` to prevent content crushing (e.g., `min-width: 120px`).
- **Pagination or virtualization** guidance for large datasets (> 100 rows): use offset-based pagination controls or virtual scrolling.

```css
@media (max-width: 640px) {
  .table--responsive-card thead { display: none; }
  .table--responsive-card tbody tr {
    display: block;
    border: 1px solid var(--border);
    border-radius: var(--radius-md);
    padding: var(--space-4);
    margin-block-end: var(--space-3);
  }
  .table--responsive-card tbody td {
    display: flex;
    justify-content: space-between;
    padding: var(--space-2) 0;
    border: none;
  }
  .table--responsive-card tbody td::before {
    content: attr(data-label);
    font-family: var(--font-display);
    font-weight: var(--fw-semibold);
    font-size: var(--fs-label);
    color: var(--fg-muted);
    text-transform: uppercase;
    letter-spacing: 0.04em;
  }
}
```

### Accessibility

- Use `<table>`, `<thead>`, `<tbody>`, `<tr>`, `<th>`, `<td>` semantic elements. Never use CSS grid/flex to fake a table for data.
- `<th scope="col">` for column headers, `<th scope="row">` for row headers.
- Sortable columns: use `aria-sort="ascending" | "descending" | "none"` on `<th>`. Sort button has focus ring.
- Row selection: checkbox inputs have descriptive `aria-label` (e.g., "Select row: Jane Doe"). "Select all" checkbox has `aria-label="Select all rows"`.
- Loading state: `aria-busy="true"` on `<tbody>` or the table wrapper.
- Empty state: announced naturally by screen readers via the visible text.
- Error state: `role="alert"` on the error message for immediate announcement.
- Sticky columns must not trap or hide content from screen readers.
- Caption: use `<caption class="visually-hidden">` for tables that lack a visible heading.
