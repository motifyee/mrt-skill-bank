## Design Principles

- **Breadcrumbs show hierarchy, not history.** They represent the page's position in the site structure (Home > Products > Electronics), not the user's click trail.
- **The current page is a span, never a link.** The last breadcrumb is the current page -- linking to yourself is confusing and violates WAI-ARIA expectations (`aria-current="page"`).
- **Separators are decorative, not content.** Slash or chevron dividers use CSS `::after` pseudo-elements with `aria-hidden` so screen readers skip them.
- **On mobile, collapse intermediate crumbs into an ellipsis.** A dropdown reveals hidden items without losing the hierarchical breadcrumb structure.

### CSS
```css
.breadcrumbs {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  gap: 0;
  list-style: none;
  padding: 0;
  margin: 0;
}
.breadcrumbs__item {
  display: inline-flex;
  align-items: center;
}
.breadcrumbs__item:not(:last-child)::after {
  content: '/';
  margin: 0 var(--space-2);
  font-family: var(--font-body);
  font-size: var(--fs-body-sm);
  color: var(--fg-subtle);
}
/* Chevron variant: uncomment to use chevrons instead of slashes */
/*
.breadcrumbs__item:not(:last-child)::after {
  content: '';
  display: inline-block;
  width: 16px;
  height: 16px;
  margin: 0 var(--space-1);
  background: currentColor;
  mask: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16'%3E%3Cpath d='M6 4l4 4-4 4' fill='none' stroke='currentColor' stroke-width='1.5' stroke-linecap='round'/%3E%3C/svg%3E") center/contain no-repeat;
  -webkit-mask: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16'%3E%3Cpath d='M6 4l4 4-4 4' fill='none' stroke='currentColor' stroke-width='1.5' stroke-linecap='round'/%3E%3C/svg%3E") center/contain no-repeat;
  color: var(--fg-subtle);
}
*/
.breadcrumbs__link {
  font-family: var(--font-display);
  font-size: var(--fs-body-sm);
  font-weight: var(--fw-medium);
  color: var(--fg-muted);
  text-decoration: none;
  padding: var(--space-1) var(--space-1);
  border-radius: var(--radius-sm);
  transition: color var(--dur-fast) var(--ease), background var(--dur-fast) var(--ease);
}
.breadcrumbs__link:hover {
  color: var(--accent);
  text-decoration: underline;
}
.breadcrumbs__link:focus-visible {
  outline: 2px solid var(--focus-ring);
  outline-offset: 2px;
}
.breadcrumbs__current {
  font-family: var(--font-display);
  font-size: var(--fs-body-sm);
  font-weight: var(--fw-semibold);
  color: var(--fg);
  padding: var(--space-1);
}
```

### HTML Pattern
```html
<nav aria-label="Breadcrumb">
  <ol class="breadcrumbs">
    <li class="breadcrumbs__item">
      <a class="breadcrumbs__link" href="/">Home</a>
    </li>
    <li class="breadcrumbs__item">
      <a class="breadcrumbs__link" href="/products">Products</a>
    </li>
    <li class="breadcrumbs__item">
      <a class="breadcrumbs__link" href="/products/electronics">Electronics</a>
    </li>
    <li class="breadcrumbs__item">
      <span class="breadcrumbs__current" aria-current="page">Headphones</span>
    </li>
  </ol>
</nav>
```

### JS Behavior
- Breadcrumbs are generally static and server-rendered. No JavaScript is required.
- **Responsive truncation**: On narrow viewports, collapse intermediate breadcrumbs into a single ellipsis button. Show a dropdown with the hidden items on click/expand. This requires JS to detect overflow and inject the collapsed state.
- **Keyboard**: All links are natively focusable and activatable with `Enter`.

### Accessibility
- Wrapped in `<nav>` with `aria-label="Breadcrumb"` (to distinguish from other navigation landmarks)
- Uses an ordered list (`<ol>`) to convey hierarchy
- Current page is a `<span>` (not a link) with `aria-current="page"`
- The last item is never a link -- it represents the current page
- Each breadcrumb link has clear, descriptive text
- On mobile, a collapsed breadcrumb should still convey the full path to screen readers (consider `aria-label` on the collapsed trigger)
