## Design Principles

- **Sidebar navigation anchors spatial orientation.** In dashboard and admin layouts, the sidebar tells users where they are and where they can go at all times.
- **Collapsible sidebars preserve screen real estate.** A collapsed state (icon-only) must remain functional and accessible; expand on hover or toggle on click.
- **Fixed sidebars create stable layout; overlay sidebars adapt to small screens.** Use fixed positioning for desktop and overlay behavior for mobile, never both simultaneously.
- **Section headers group related navigation items.** Labeled sections like "Projects", "Team", and "Settings" reduce cognitive load and scanning time.
- **The active item is visually unmistakable.** Use background fill, accent color, and font weight together -- never rely on color alone to indicate the current page.

## Brand Expression

The sidebar is a persistent structural element. Apply `character_rules.navigation`
and `components.component_style_contract.navigation` to shape, spacing, and active
indicators:

- `safe`: clean background fill on active item, consistent icon sizing, minimal decoration.
- `elevated`: branded active rail (accent `border-inline-start`), signature hover transition, or subtle gradient background.
- `bold`: oversized icons, asymmetric section spacing, or accent-colored section headers when hierarchy remains clear.
- `experimental`: animated transitions between states only for non-critical apps; collapse/expand animation must respect `prefers-reduced-motion`.

### Component Tokens

```css
.sidebar {
  --sidebar-width: 260px;
  --sidebar-width-collapsed: 64px;
  --sidebar-bg: var(--bg);
  --sidebar-border-color: var(--border);
  --sidebar-item-height: 40px;
  --sidebar-item-radius: var(--radius-sm);
  --sidebar-item-padding: var(--space-2) var(--space-3);
  --sidebar-item-gap: var(--space-3);
  --sidebar-icon-size: 20px;
  --sidebar-section-color: var(--fg-muted);
  --sidebar-section-size: var(--fs-label);
  --sidebar-active-bg: var(--accent-tint);
  --sidebar-active-color: var(--accent);
  --sidebar-hover-bg: var(--bg-alt);
  --sidebar-transition-duration: var(--dur-base);
}
```

### CSS

```css
.sidebar {
  width: var(--sidebar-width);
  height: 100vh;
  position: fixed;
  inset-block-start: 0;
  inset-inline-start: 0;
  z-index: 50;
  background: var(--sidebar-bg);
  border-inline-end: 1px solid var(--sidebar-border-color);
  display: flex;
  flex-direction: column;
  overflow-y: auto;
  overflow-x: hidden;
  transition: width var(--sidebar-transition-duration) var(--ease);
}

/* Collapsed state */
.sidebar--collapsed {
  width: var(--sidebar-width-collapsed);
}

.sidebar--collapsed .sidebar__label,
.sidebar--collapsed .sidebar__section-title,
.sidebar--collapsed .sidebar__footer-content {
  display: none;
}

.sidebar--collapsed .sidebar__item {
  justify-content: center;
  padding-inline: 0;
}

/* Header */
.sidebar__header {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-4) var(--space-4);
  border-block-end: 1px solid var(--sidebar-border-color);
  flex-shrink: 0;
  min-height: 64px;
}

.sidebar__brand {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  text-decoration: none;
  overflow: hidden;
  white-space: nowrap;
}

.sidebar__logo {
  width: var(--sidebar-icon-size);
  height: var(--sidebar-icon-size);
  flex-shrink: 0;
}

.sidebar__brand-name {
  font-family: var(--font-display);
  font-size: var(--fs-body);
  font-weight: var(--fw-bold);
  color: var(--fg);
}

/* Navigation sections */
.sidebar__nav {
  flex: 1;
  padding: var(--space-3) var(--space-3);
  display: flex;
  flex-direction: column;
  gap: var(--space-1);
}

.sidebar__section {
  display: flex;
  flex-direction: column;
  gap: var(--space-1);
}

.sidebar__section + .sidebar__section {
  margin-block-start: var(--space-4);
}

.sidebar__section-title {
  font-family: var(--font-display);
  font-size: var(--sidebar-section-size);
  font-weight: var(--fw-semibold);
  color: var(--sidebar-section-color);
  text-transform: uppercase;
  letter-spacing: 0.06em;
  padding: var(--space-2) var(--space-3);
  white-space: nowrap;
  overflow: hidden;
}

/* Nav items */
.sidebar__item {
  display: flex;
  align-items: center;
  gap: var(--sidebar-item-gap);
  padding: var(--sidebar-item-padding);
  min-height: var(--sidebar-item-height);
  border-radius: var(--sidebar-item-radius);
  font-family: var(--font-display);
  font-size: var(--fs-body-sm);
  font-weight: var(--fw-medium);
  color: var(--fg-muted);
  text-decoration: none;
  white-space: nowrap;
  transition: background-color var(--dur-fast) var(--ease),
              color var(--dur-fast) var(--ease);
}

.sidebar__item:hover {
  background: var(--sidebar-hover-bg);
  color: var(--fg);
}

.sidebar__item:focus-visible {
  outline: 2px solid var(--focus-ring);
  outline-offset: -2px;
}

.sidebar__item.active {
  background: var(--sidebar-active-bg);
  color: var(--sidebar-active-color);
  font-weight: var(--fw-semibold);
}

.sidebar__item-icon {
  width: var(--sidebar-icon-size);
  height: var(--sidebar-icon-size);
  flex-shrink: 0;
}

.sidebar__label {
  overflow: hidden;
  text-overflow: ellipsis;
}

/* Badge / count on item */
.sidebar__item-badge {
  margin-inline-start: auto;
  font-size: var(--fs-label);
  font-weight: var(--fw-semibold);
  color: var(--fg-muted);
  background: var(--bg-alt);
  padding: 0 var(--space-2);
  border-radius: var(--radius-full);
  min-width: 20px;
  text-align: center;
  line-height: 20px;
}

/* Footer */
.sidebar__footer {
  padding: var(--space-3) var(--space-4);
  border-block-start: 1px solid var(--sidebar-border-color);
  flex-shrink: 0;
}

.sidebar__footer-content {
  display: flex;
  align-items: center;
  gap: var(--space-3);
}

/* Collapse toggle */
.sidebar__toggle {
  position: absolute;
  inset-block-end: var(--space-4);
  inset-inline-end: calc(-1 * var(--space-3));
  width: 28px;
  height: 28px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--bg);
  border: 1px solid var(--border);
  border-radius: var(--radius-full);
  cursor: pointer;
  color: var(--fg-muted);
  transition: color var(--dur-fast) var(--ease),
              background-color var(--dur-fast) var(--ease),
              transform var(--dur-fast) var(--ease);
  z-index: 1;
}

.sidebar__toggle:hover {
  color: var(--fg);
  background: var(--bg-alt);
}

.sidebar__toggle:focus-visible {
  outline: 2px solid var(--focus-ring);
  outline-offset: 2px;
}

.sidebar--collapsed .sidebar__toggle {
  transform: rotate(180deg);
}
```

### HTML Pattern

```html
<aside class="sidebar" role="navigation" aria-label="Sidebar navigation">
  <!-- Header -->
  <div class="sidebar__header">
    <a href="/" class="sidebar__brand">
      <svg class="sidebar__logo" aria-hidden="true" width="20" height="20"><!-- logo --></svg>
      <span class="sidebar__label sidebar__brand-name">AppName</span>
    </a>
  </div>

  <!-- Navigation -->
  <nav class="sidebar__nav">
    <div class="sidebar__section">
      <span class="sidebar__section-title">Overview</span>
      <a href="/dashboard" class="sidebar__item active" aria-current="page">
        <svg class="sidebar__item-icon" aria-hidden="true" width="20" height="20"><!-- icon --></svg>
        <span class="sidebar__label">Dashboard</span>
      </a>
      <a href="/analytics" class="sidebar__item">
        <svg class="sidebar__item-icon" aria-hidden="true" width="20" height="20"><!-- icon --></svg>
        <span class="sidebar__label">Analytics</span>
      </a>
    </div>

    <div class="sidebar__section">
      <span class="sidebar__section-title">Workspace</span>
      <a href="/projects" class="sidebar__item">
        <svg class="sidebar__item-icon" aria-hidden="true" width="20" height="20"><!-- icon --></svg>
        <span class="sidebar__label">Projects</span>
        <span class="sidebar__item-badge">5</span>
      </a>
      <a href="/team" class="sidebar__item">
        <svg class="sidebar__item-icon" aria-hidden="true" width="20" height="20"><!-- icon --></svg>
        <span class="sidebar__label">Team</span>
      </a>
    </div>
  </nav>

  <!-- Footer -->
  <div class="sidebar__footer">
    <div class="sidebar__footer-content">
      <svg class="sidebar__item-icon" aria-hidden="true" width="20" height="20"><!-- user icon --></svg>
      <span class="sidebar__label">Jane Doe</span>
    </div>
  </div>

  <button class="sidebar__toggle" type="button" aria-label="Collapse sidebar">
    <svg width="16" height="16" viewBox="0 0 16 16" aria-hidden="true">
      <path d="M10 3L5 8l5 5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round" fill="none"/>
    </svg>
  </button>
</aside>
```

### Mobile Overlay Behavior

On screens below `lg` breakpoint, the sidebar becomes an overlay:

```css
@media (max-width: 1023px) {
  .sidebar {
    transform: translateX(-100%);
    transition: transform var(--sidebar-transition-duration) var(--ease);
  }
  [dir="rtl"] .sidebar {
    transform: translateX(100%);
  }
  .sidebar.is-open {
    transform: translateX(0);
  }
  .sidebar--collapsed {
    width: var(--sidebar-width); /* always full width on mobile */
  }
}

.sidebar-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
  z-index: 49;
  opacity: 0;
  pointer-events: none;
  transition: opacity var(--dur-normal) var(--ease);
}
.sidebar-overlay.is-open {
  opacity: 1;
  pointer-events: auto;
}
```

### Keyboard Navigation

- **Tab**: Moves focus between sidebar items in sequence.
- **ArrowDown / ArrowUp**: Optional roving tabindex pattern for rapid navigation within the sidebar.
- **Home**: Focus first item.
- **End**: Focus last item.
- **Enter / Space**: Activate the focused item (navigate to page).
- **Escape**: If sidebar is in overlay mode (mobile), close the sidebar and return focus to the trigger.
- **Collapse toggle**: `Tab` reaches the toggle button; `Enter` or `Space` toggles collapsed state.

### Dark Mode

```css
[data-theme="dark"] .sidebar {
  --sidebar-bg: var(--bg-alt);
  --sidebar-border-color: var(--border);
  --sidebar-hover-bg: rgba(255, 255, 255, 0.06);
  --sidebar-active-bg: color-mix(in srgb, var(--accent) 12%, transparent);
  --sidebar-section-color: var(--fg-muted);
}
```

### Accessibility

- `<aside>` or `<nav>` with `aria-label="Sidebar navigation"`.
- Active item uses `aria-current="page"`.
- Section titles are decorative grouping labels; use `role="group"` with `aria-labelledby` if programmatic association is needed.
- Collapsed sidebar: hidden text labels remain in the DOM for screen readers; use `visually-hidden` class instead of `display: none` for labels when in collapsed mode.
- Collapse toggle: `aria-label="Collapse sidebar"` / `aria-label="Expand sidebar"` (toggled dynamically), `aria-expanded`.
- Mobile overlay: `aria-modal="true"` on the overlay container, `inert` on main content while open.
- Footer user info should be a link or button for keyboard access when interactive.
- Badge counts are supplementary text; the link text is the primary accessible name.

### States

- **Default**: muted text, no background.
- **Hover**: subtle background fill, foreground color change.
- **Focus**: visible focus ring (offset inward to stay within the sidebar).
- **Active**: accent background tint, accent text, semibold weight.
- **Collapsed**: icon-only mode, labels hidden (but still accessible to screen readers).

### Responsive Summary

- **Desktop (>= 1024px)**: Fixed sidebar with optional collapse toggle. Main content offset by sidebar width.
- **Tablet (768-1023px)**: Overlay sidebar triggered by a menu button in the top nav. Full sidebar width.
- **Mobile (< 768px)**: Same overlay behavior. Close on Escape, overlay tap, or close button.
