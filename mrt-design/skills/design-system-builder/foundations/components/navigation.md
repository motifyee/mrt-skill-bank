## Design Principles

- **Navigation stays fixed and accessible.** A sticky top bar with backdrop blur ensures nav links are always reachable without scrolling back to the top.
- **Active state is unmistakable.** The current page link uses both a color change and an accent underline indicator -- never rely on color alone to show "you are here."
- **Mobile navigation is a full pattern, not a breakpoint afterthought.** The hamburger menu must include a close button, focus trap, and Escape-to-dismiss to be production-ready.
- **Link spacing is generous enough for touch.** Nav links need adequate hit areas even on desktop; 44px vertical padding minimum for mobile hamburger items.

## Brand Expression

Navigation is the system's orientation layer. Apply `character_rules.navigation`,
`components.component_style_contract.navigation`, and signature DNA to active,
hover, focus, and mobile states:

- `safe`: clear active underline or background, no decorative motion.
- `elevated`: branded active rail/underline, signature focus ring, or subtle surface blur.
- `bold`: asymmetric nav layout, oversized active marker, or split navigation if still scannable.
- `experimental`: kinetic nav transitions only for marketing surfaces; product navigation stays predictable.

Navigation must use the same active-state language across dashboard, docs,
marketing, and preview files.

### Component Tokens

```css
.nav {
  --nav-height: 64px;
  --nav-bg: var(--bg);
  --nav-border-color: var(--border);
  --nav-link-color: var(--fg-muted);
  --nav-link-active-color: var(--fg);
  --nav-link-active-indicator: var(--accent);
  --nav-link-focus-ring: var(--focus-ring);
  --nav-link-font: var(--font-display);
  --nav-link-size: var(--fs-body-sm);
  --nav-link-weight: var(--fw-medium);
  --nav-mobile-bg: var(--bg);
  --nav-overlay-bg: rgba(0, 0, 0, 0.5);
}
```

### Top Navigation Bar

```css
.nav {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-inline: var(--space-7);
  padding-block: 0;
  height: var(--nav-height);
  background: var(--nav-bg);
  border-block-end: 1px solid var(--nav-border-color);
  position: sticky;
  top: 0;
  z-index: 100;
  backdrop-filter: blur(12px);
}

.nav__brand {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  text-decoration: none;
  flex-shrink: 0;
}

.nav__logo {
  height: 32px;
  width: auto;
}

.nav__brand-name {
  font-family: var(--font-display);
  font-size: var(--fs-body);
  font-weight: var(--fw-bold);
  color: var(--fg);
}

.nav-links {
  display: flex;
  gap: var(--space-6);
  list-style: none;
  margin: 0;
  padding: 0;
}

.nav-link {
  font-family: var(--nav-link-font);
  font-size: var(--nav-link-size);
  font-weight: var(--nav-link-weight);
  color: var(--nav-link-color);
  text-decoration: none;
  padding: var(--space-2) 0;
  position: relative;
  transition: color var(--dur-fast) var(--ease);
}

.nav-link:hover {
  color: var(--nav-link-active-color);
}

.nav-link:focus-visible {
  outline: 2px solid var(--nav-link-focus-ring);
  outline-offset: 4px;
  border-radius: var(--radius-sm);
}

.nav-link.active {
  color: var(--nav-link-active-color);
  font-weight: var(--fw-semibold);
}

.nav-link.active::after {
  content: '';
  position: absolute;
  bottom: -2px;
  inset-inline-start: 0;
  inset-inline-end: 0;
  height: 2px;
  background: var(--nav-link-active-indicator);
  border-radius: 1px;
}

.nav__actions {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  flex-shrink: 0;
}
```

### HTML Pattern

```html
<nav class="nav" role="navigation" aria-label="Main navigation">
  <a href="/" class="nav__brand">
    <img class="nav__logo" src="/logo.svg" alt="" width="32" height="32" />
    <span class="nav__brand-name">Brand</span>
  </a>

  <ul class="nav-links">
    <li><a href="/dashboard" class="nav-link active" aria-current="page">Dashboard</a></li>
    <li><a href="/projects" class="nav-link">Projects</a></li>
    <li><a href="/team" class="nav-link">Team</a></li>
    <li><a href="/settings" class="nav-link">Settings</a></li>
  </ul>

  <div class="nav__actions">
    <button class="btn btn-ghost btn-icon" aria-label="Notifications">
      <!-- bell icon -->
    </button>
    <button class="btn btn-ghost" aria-label="User menu">
      <!-- avatar + chevron -->
    </button>
  </div>

  <button class="nav__hamburger" type="button" aria-label="Open menu" aria-expanded="false" aria-controls="nav-mobile">
    <svg width="24" height="24" viewBox="0 0 24 24" aria-hidden="true">
      <rect y="5" width="24" height="2" rx="1" fill="currentColor"/>
      <rect y="11" width="24" height="2" rx="1" fill="currentColor"/>
      <rect y="17" width="24" height="2" rx="1" fill="currentColor"/>
    </svg>
  </button>
</nav>
```

### Mobile Navigation

```css
/* Hide hamburger on desktop */
.nav__hamburger {
  display: none;
  background: none;
  border: none;
  color: var(--fg);
  cursor: pointer;
  padding: var(--space-2);
  border-radius: var(--radius-sm);
  transition: color var(--dur-fast) var(--ease),
              background-color var(--dur-fast) var(--ease);
}
.nav__hamburger:hover {
  background: var(--bg-alt);
}
.nav__hamburger:focus-visible {
  outline: 2px solid var(--focus-ring);
  outline-offset: 2px;
}

/* Mobile overlay */
.nav-mobile {
  position: fixed;
  inset: 0;
  z-index: 200;
  display: flex;
  pointer-events: none;
  visibility: hidden;
}
.nav-mobile.is-open {
  pointer-events: auto;
  visibility: visible;
}

.nav-mobile__overlay {
  position: absolute;
  inset: 0;
  background: var(--nav-overlay-bg);
  opacity: 0;
  transition: opacity var(--dur-normal) var(--ease);
}
.nav-mobile.is-open .nav-mobile__overlay {
  opacity: 1;
}

.nav-mobile__panel {
  position: absolute;
  inset-block-start: 0;
  inset-block-end: 0;
  inset-inline-start: 0;
  width: min(320px, 85vw);
  background: var(--nav-mobile-bg);
  padding: var(--space-5);
  display: flex;
  flex-direction: column;
  gap: var(--space-2);
  transform: translateX(-100%);
  transition: transform var(--dur-normal) var(--ease);
  overflow-y: auto;
}
[dir="rtl"] .nav-mobile__panel {
  inset-inline-start: auto;
  inset-inline-end: 0;
  transform: translateX(100%);
}
.nav-mobile.is-open .nav-mobile__panel {
  transform: translateX(0);
}

.nav-mobile__close {
  align-self: flex-end;
  width: 44px;
  height: 44px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: none;
  border: none;
  color: var(--fg-muted);
  cursor: pointer;
  border-radius: var(--radius-sm);
  margin-block-end: var(--space-4);
  transition: color var(--dur-fast) var(--ease),
              background-color var(--dur-fast) var(--ease);
}
.nav-mobile__close:hover {
  color: var(--fg);
  background: var(--bg-alt);
}
.nav-mobile__close:focus-visible {
  outline: 2px solid var(--focus-ring);
  outline-offset: 2px;
}

.nav-mobile__link {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding-block: var(--space-4);
  padding-inline: var(--space-3);
  font-family: var(--nav-link-font);
  font-size: var(--fs-body);
  font-weight: var(--nav-link-weight);
  color: var(--nav-link-color);
  text-decoration: none;
  border-radius: var(--radius-sm);
  min-height: 44px;
  transition: color var(--dur-fast) var(--ease),
              background-color var(--dur-fast) var(--ease);
}
.nav-mobile__link:hover {
  background: var(--bg-alt);
  color: var(--fg);
}
.nav-mobile__link.active {
  color: var(--accent);
  background: var(--accent-tint);
  font-weight: var(--fw-semibold);
}
.nav-mobile__link:focus-visible {
  outline: 2px solid var(--focus-ring);
  outline-offset: -2px;
}
```

### Mobile HTML Pattern

```html
<div class="nav-mobile" id="nav-mobile" role="dialog" aria-modal="true" aria-label="Navigation menu">
  <div class="nav-mobile__overlay" data-close-mobile-nav></div>
  <div class="nav-mobile__panel">
    <button class="nav-mobile__close" type="button" aria-label="Close menu">
      <svg width="24" height="24" viewBox="0 0 24 24" aria-hidden="true">
        <path d="M6 6l12 12M18 6L6 18" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
      </svg>
    </button>
    <a href="/dashboard" class="nav-mobile__link active" aria-current="page">
      <svg aria-hidden="true" width="20" height="20"><!-- icon --></svg>
      Dashboard
    </a>
    <a href="/projects" class="nav-mobile__link">
      <svg aria-hidden="true" width="20" height="20"><!-- icon --></svg>
      Projects
    </a>
    <a href="/team" class="nav-mobile__link">
      <svg aria-hidden="true" width="20" height="20"><!-- icon --></svg>
      Team
    </a>
    <a href="/settings" class="nav-mobile__link">
      <svg aria-hidden="true" width="20" height="20"><!-- icon --></svg>
      Settings
    </a>
  </div>
</div>
```

### Navigation Variants

- **Top nav**: marketing and simple apps. Horizontal link list with logo and actions.
- **Sidebar nav**: dashboards, docs, admin tools. See sidebar.md component.
- **Breadcrumb nav**: deep hierarchy and settings/admin flows. See breadcrumbs.md component.
- **Command/nav launcher**: power-user products when command palette exists.
- **Tab-style nav**: secondary navigation within a page section. Uses tab pattern with active underline.

### States

- **Default**: muted color, medium weight.
- **Hover**: foreground color change, no background (desktop) or subtle background (mobile).
- **Focus**: visible focus ring offset from link text.
- **Active / current**: `aria-current="page"`, accent color, bold weight, underline indicator.
- **Disabled**: muted color, reduced opacity, `aria-disabled="true"`, removed from tab order.

### Dark Mode

```css
@media (prefers-color-scheme: dark) {
  .nav {
    --nav-bg: rgba(var(--bg-rgb), 0.85);
    --nav-border-color: var(--border-subtle);
    --nav-link-color: var(--fg-muted);
    --nav-link-active-color: var(--fg);
    --nav-mobile-bg: var(--bg-elevated);
    --nav-overlay-bg: rgba(0, 0, 0, 0.6);
  }
  .nav {
    backdrop-filter: blur(16px);
  }
}
```

### Responsive / RTL

- Use logical properties (`inset-inline-start`, `inset-inline-end`, `padding-inline`) for active rails and padding.
- Below `lg` breakpoint (1024px): hide `.nav-links` and `.nav__actions`; show `.nav__hamburger`.
- In RTL, mirror icons and slide directions where the icon implies movement.
- Mobile nav panel slides in from `inline-start` side; in RTL, slides from `inline-end`.
- Mobile nav must trap focus and restore focus to the trigger on close.
- Close with Escape key as an alternative to the close button.

```css
@media (max-width: 1023px) {
  .nav-links,
  .nav__actions {
    display: none;
  }
  .nav__hamburger {
    display: flex;
    align-items: center;
    justify-content: center;
  }
}

@media (min-width: 1024px) {
  .nav-mobile {
    display: none;
  }
}
```

### JS Behavior

- **Mobile toggle**: Click hamburger sets `aria-expanded="true"` on trigger and adds `is-open` to `.nav-mobile`. Focus first link inside panel.
- **Close**: Click close button, overlay, or press Escape. Remove `is-open`, set `aria-expanded="false"`, return focus to hamburger.
- **Focus trap**: When mobile nav is open, Tab / Shift+Tab cycles through items inside `.nav-mobile__panel` only. Use `inert` on main content.
- **Body scroll lock**: Prevent background scrolling when mobile nav is open.
- **Active link detection**: Apply `.active` class based on current URL path matching.

### Accessibility

- `<nav>` element with `aria-label="Main navigation"` to distinguish from other nav landmarks.
- Active link has `aria-current="page"` for screen reader announcement.
- Hamburger button: `aria-label="Open menu"` / `aria-label="Close menu"` (toggle dynamically), `aria-expanded`, `aria-controls`.
- Mobile panel: `role="dialog"`, `aria-modal="true"`, `aria-label`.
- Close button: `aria-label="Close menu"`.
- All nav links are keyboard accessible with visible focus indicators.
- Mobile links have minimum 44px touch target height.
- Skip navigation link (`<a href="#main" class="skip-link">Skip to main content</a>`) is the first focusable element in the document.
