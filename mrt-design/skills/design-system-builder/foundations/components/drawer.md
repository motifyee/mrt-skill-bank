## Design Principles

- **Drawers slide in from the edge** and push or overlay content. They are for focused tasks that benefit from retaining page context — filters, detail panels, forms, settings.
- **A drawer is narrower than a modal and taller than a dropdown.** It occupies a side of the viewport, not the center.
- **Escape closes the drawer.** A visible close button or "Done" action is always present.
- **Focus is trapped inside the drawer while open** and returns to the trigger on close.
- **The scrim dims the page behind** to indicate the drawer is the active surface.

## Brand Expression

Drawers are utility surfaces. Apply `creative_brief` through header treatment, border style, and action layout:

- `safe`: standard right-side panel with neutral border, flat header.
- `elevated`: accent-tinted header divider, branded close button, or subtle accent rail.
- `bold`: left-side drawer for navigation-style panels, branded header with illustration.
- `experimental`: multi-panel drawer, resizable width, or collapsible sub-sections.

### Component Tokens

```css
.drawer {
  --drawer-width: 400px;
  --drawer-bg: var(--bg);
  --drawer-border: 1px solid var(--border);
  --drawer-shadow: var(--shadow-lg);
  --drawer-radius: var(--radius-lg) 0 0 var(--radius-lg);
  --drawer-header-border: 1px solid var(--border);
  --drawer-padding: var(--space-5);
}
```

### CSS

```css
.drawer-scrim {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.4);
  z-index: var(--z-modal, 300);
  animation: scrimIn var(--dur-fast) var(--ease) both;
}
@keyframes scrimIn {
  from { opacity: 0; }
  to   { opacity: 1; }
}

.drawer {
  position: fixed;
  inset-block-start: 0;
  inset-block-end: 0;
  inset-inline-end: 0;
  width: var(--drawer-width);
  max-width: 90vw;
  background: var(--drawer-bg);
  border-inline-start: var(--drawer-border);
  box-shadow: var(--drawer-shadow);
  border-radius: var(--drawer-radius);
  display: flex;
  flex-direction: column;
  z-index: var(--z-modal, 300);
  animation: drawerSlideIn var(--dur-base) var(--ease) both;
}
@keyframes drawerSlideIn {
  from { transform: translateX(100%); }
  to   { transform: translateX(0); }
}

[dir="rtl"] .drawer {
  inset-inline-start: 0;
  inset-inline-end: auto;
  border-inline-start: none;
  border-inline-end: var(--drawer-border);
  border-radius: 0 var(--drawer-radius) var(--drawer-radius) 0;
  animation-name: drawerSlideInRtl;
}
@keyframes drawerSlideInRtl {
  from { transform: translateX(-100%); }
  to   { transform: translateX(0); }
}

.drawer--exiting {
  animation: drawerSlideOut var(--dur-fast) var(--ease) both;
}
@keyframes drawerSlideOut {
  to { transform: translateX(100%); opacity: 0; }
}

.drawer__header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--drawer-padding);
  border-block-end: var(--drawer-header-border);
  flex-shrink: 0;
}

.drawer__title {
  font-family: var(--font-display);
  font-size: var(--fs-h4);
  font-weight: var(--fw-semibold);
  color: var(--fg);
  margin: 0;
}

.drawer__close {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 32px;
  height: 32px;
  background: transparent;
  border: none;
  border-radius: var(--radius-sm);
  color: var(--fg-muted);
  cursor: pointer;
  transition: color var(--dur-fast) var(--ease),
              background-color var(--dur-fast) var(--ease);
}
.drawer__close:hover {
  color: var(--fg);
  background: var(--bg-alt);
}
.drawer__close:focus-visible {
  outline: 2px solid var(--focus-ring);
  outline-offset: 2px;
}

.drawer__body {
  padding: var(--drawer-padding);
  overflow-y: auto;
  flex: 1;
}

.drawer__footer {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  gap: var(--space-3);
  padding: var(--space-4) var(--drawer-padding);
  border-block-start: 1px solid var(--border);
  flex-shrink: 0;
}
```

### Size Variants

```css
.drawer--sm {
  --drawer-width: 320px;
}

.drawer--lg {
  --drawer-width: 560px;
}
```

### HTML Pattern

```html
<div class="drawer-scrim" aria-hidden="true"></div>
<aside class="drawer" role="dialog" aria-modal="true" aria-labelledby="drawer-title-1">
  <div class="drawer__header">
    <h2 class="drawer__title" id="drawer-title-1">Filters</h2>
    <button class="drawer__close" type="button" aria-label="Close panel">
      <svg width="16" height="16" viewBox="0 0 16 16" aria-hidden="true">
        <path d="M4 4l8 8M12 4l-8 8" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
      </svg>
    </button>
  </div>
  <div class="drawer__body">
    <!-- Filter controls, form fields, or detail content -->
  </div>
  <div class="drawer__footer">
    <button class="btn btn-secondary" type="button">Reset</button>
    <button class="btn btn-primary" type="button">Apply Filters</button>
  </div>
</aside>
```

### Dark Mode

```css
[data-theme="dark"] .drawer {
  --drawer-bg: var(--bg-alt);
  --drawer-shadow: 0 8px 32px rgba(0, 0, 0, 0.5);
}
```

### Accessibility

- `role="dialog"` and `aria-modal="true"` on the drawer element.
- `aria-labelledby` pointing to the drawer title.
- Focus trap: Tab and Shift+Tab cycle within the drawer. Escape closes.
- Return focus to the trigger element on close.
- Scrim is `aria-hidden="true"`.
- Close button has `aria-label="Close panel"`.

### Responsive Behavior

- Below 640px, drawer becomes full-width (minus small margin).
- Touch drag to dismiss is optional but should not conflict with scroll inside the body.
- Footer actions stack vertically on narrow drawers.
