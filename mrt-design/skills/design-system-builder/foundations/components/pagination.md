## Design Principles

- **The current page is visually dominant and uses aria-current.** Accent background color plus `aria-current="page"` communicate "you are here" through both visual and assistive channels.
- **Ellipsis replaces invisible pages, never removes them arbitrarily.** Show first, last, current, and adjacent siblings; bridge gaps with a single ellipsis character that is `aria-hidden`.
- **Prev/Next are disabled, not removed, at boundaries.** A dimmed, non-interactive prev arrow on page 1 preserves spatial consistency so the user knows navigation exists in that direction.
- **Every page link has an aria-label.** "Page 2", "Previous page", "Next page" ensure screen readers announce purpose beyond the visible numeral or icon.

### CSS
```css
.pagination {
  display: flex;
  align-items: center;
  gap: var(--space-1);
  list-style: none;
  padding: 0;
  margin: 0;
}
.pagination__item {
  display: inline-flex;
  align-items: center;
  justify-content: center;
}
.pagination__link {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 36px;
  height: 36px;
  padding: 0 var(--space-2);
  font-family: var(--font-display);
  font-size: var(--fs-body-sm);
  font-weight: var(--fw-medium);
  color: var(--fg-muted);
  background: transparent;
  border: 1px solid transparent;
  border-radius: var(--radius-sm);
  text-decoration: none;
  cursor: pointer;
  transition: background var(--dur-fast) var(--ease), color var(--dur-fast) var(--ease), border-color var(--dur-fast) var(--ease);
}
.pagination__link:hover {
  background: var(--bg-alt);
  color: var(--fg);
}
.pagination__link:focus-visible {
  outline: 2px solid var(--focus-ring);
  outline-offset: 2px;
}
.pagination__link--active {
  background: var(--accent);
  color: var(--on-accent);
  border-color: var(--accent);
  font-weight: var(--fw-semibold);
}
.pagination__link--active:hover {
  background: var(--accent-hover);
  color: var(--on-accent);
}
.pagination__link--disabled {
  opacity: 0.4;
  pointer-events: none;
}
.pagination__ellipsis {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 36px;
  height: 36px;
  font-family: var(--font-display);
  font-size: var(--fs-body-sm);
  color: var(--fg-muted);
  user-select: none;
}
```

### HTML Pattern
```html
<nav aria-label="Pagination">
  <ul class="pagination" data-pagination>
    <li class="pagination__item">
      <a class="pagination__link pagination__link--disabled"
         href="#" aria-disabled="true" aria-label="Previous page">
        <svg width="16" height="16" viewBox="0 0 16 16" aria-hidden="true">
          <path d="M10 4l-4 4 4 4" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
        </svg>
      </a>
    </li>
    <li class="pagination__item">
      <a class="pagination__link pagination__link--active"
         href="#" aria-current="page" aria-label="Page 1">1</a>
    </li>
    <li class="pagination__item">
      <a class="pagination__link" href="#" aria-label="Page 2">2</a>
    </li>
    <li class="pagination__item">
      <a class="pagination__link" href="#" aria-label="Page 3">3</a>
    </li>
    <li class="pagination__item">
      <span class="pagination__ellipsis" aria-hidden="true">&hellip;</span>
    </li>
    <li class="pagination__item">
      <a class="pagination__link" href="#" aria-label="Page 24">24</a>
    </li>
    <li class="pagination__item">
      <a class="pagination__link" href="#" aria-label="Next page">
        <svg width="16" height="16" viewBox="0 0 16 16" aria-hidden="true">
          <path d="M6 4l4 4-4 4" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
        </svg>
      </a>
    </li>
  </ul>
</nav>
```

### JS Behavior
- **Triggers**: Click on a page number or prev/next arrow triggers page change. Call a callback with the new page number.
- **Ellipsis logic**: Show first page, last page, current page, and one sibling on each side. Use ellipsis to bridge gaps. Example for page 5 of 24: `1 ... 4 [5] 6 ... 24`.
- **Keyboard**: All links are natively focusable. `Enter` activates. Tab navigates between links. For pagination within a larger interactive widget, consider arrow key navigation.
- **Prev/Next**: Previous link is disabled (`.pagination__link--disabled` + `aria-disabled="true"`) on the first page. Next link is disabled on the last page.
- **URL update**: Optionally update the URL query string (`?page=N`) on page change.

### Accessibility
- Wrapped in `<nav>` with `aria-label="Pagination"` (to distinguish from other nav landmarks)
- Active page uses `aria-current="page"`
- Prev/next links have `aria-label` (e.g., "Previous page", "Next page") since they use icons
- Page links have `aria-label` (e.g., "Page 2") for clarity
- Disabled links use `aria-disabled="true"` instead of removing the element entirely
- Ellipsis is `aria-hidden="true"` since it is decorative
