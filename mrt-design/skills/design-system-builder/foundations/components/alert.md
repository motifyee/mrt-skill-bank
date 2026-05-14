## Design Principles

- **Alerts are persistent page-level messages that demand attention.** Unlike toasts (transient, non-blocking), alerts remain visible until the user dismisses them or the condition resolves.
- **Severity is communicated through both color and icon.** An alert never relies on color alone; the icon and text label reinforce the semantic meaning.
- **Alerts sit contextually near the content they relate to.** Page-level alerts appear at the top of the main content area. Inline alerts appear within forms or sections near the relevant field or data.
- **Dismissible alerts provide a clear close action.** Persistent alerts (system-wide notices) do not include a dismiss button; they remain until the underlying condition changes.
- **Action buttons within alerts provide immediate resolution.** If the user can fix the problem directly from the alert, include a primary action button.

## Brand Expression

Alerts are functional-first but still express the system's design language.
Apply `creative_brief` restraint and `character_rules` to shape, icon style,
and action layout:

- `safe`: standard rounded rectangle with colored accent bar, clear icon, and action buttons.
- `elevated`: signature border treatment, branded icon style, or accent-colored action buttons.
- `bold`: full-width banner treatment, larger icon, or prominent action layout.
- `experimental`: avoid experimental treatments on error/critical alerts; reserve for informational banners only.

### Component Tokens

```css
.alert {
  --alert-padding: var(--space-4);
  --alert-gap: var(--space-3);
  --alert-radius: var(--radius-md);
  --alert-font: var(--font-body);
  --alert-font-size: var(--fs-body-sm);
  --alert-icon-size: 20px;
  --alert-border-width: 1px;
  --alert-border-color: transparent;
  --alert-accent-width: 4px;
}
```

### CSS

```css
.alert {
  display: flex;
  align-items: flex-start;
  gap: var(--alert-gap);
  padding: var(--alert-padding);
  border-radius: var(--alert-radius);
  border: var(--alert-border-width) solid var(--alert-border-color);
  border-inline-start: var(--alert-accent-width) solid var(--alert-border-color);
  font-family: var(--alert-font);
  font-size: var(--alert-font-size);
  line-height: 1.5;
  color: var(--fg);
}

/* Icon */
.alert__icon {
  flex-shrink: 0;
  width: var(--alert-icon-size);
  height: var(--alert-icon-size);
  margin-block-start: 2px;
}

/* Content */
.alert__content {
  flex: 1;
  min-width: 0;
}

.alert__title {
  font-family: var(--font-display);
  font-weight: var(--fw-semibold);
  font-size: var(--fs-body-sm);
  margin: 0 0 var(--space-1) 0;
  color: var(--fg);
}

.alert__message {
  color: var(--fg-muted);
  margin: 0;
}

.alert__message p {
  margin: 0;
}

/* Actions */
.alert__actions {
  display: flex;
  align-items: center;
  gap: var(--space-2);
  margin-block-start: var(--space-3);
  flex-wrap: wrap;
}

/* Dismiss button */
.alert__dismiss {
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 28px;
  height: 28px;
  background: transparent;
  border: none;
  border-radius: var(--radius-sm);
  color: var(--fg-muted);
  cursor: pointer;
  margin-inline-start: auto;
  transition: color var(--dur-fast) var(--ease),
              background-color var(--dur-fast) var(--ease);
}
.alert__dismiss:hover {
  color: var(--fg);
  background: rgba(0, 0, 0, 0.06);
}
.alert__dismiss:focus-visible {
  outline: 2px solid var(--focus-ring);
  outline-offset: 2px;
}
```

### Semantic Variants

```css
/* Info */
.alert--info {
  --alert-border-color: var(--accent);
  background: var(--accent-tint);
}
.alert--info .alert__icon {
  color: var(--accent);
}

/* Warning */
.alert--warning {
  --alert-border-color: var(--warning, #f59e0b);
  background: color-mix(in srgb, var(--warning, #f59e0b) 8%, transparent);
}
.alert--warning .alert__icon {
  color: var(--warning, #f59e0b);
}

/* Error */
.alert--error {
  --alert-border-color: var(--error);
  background: color-mix(in srgb, var(--error) 8%, transparent);
}
.alert--error .alert__icon {
  color: var(--error);
}

/* Success */
.alert--success {
  --alert-border-color: var(--success);
  background: color-mix(in srgb, var(--success) 8%, transparent);
}
.alert--success .alert__icon {
  color: var(--success);
}

/* Neutral */
.alert--neutral {
  --alert-border-color: var(--border-strong);
  background: var(--bg-alt);
}
.alert--neutral .alert__icon {
  color: var(--fg-muted);
}
```

### HTML Pattern

```html
<!-- Dismissible info alert with action -->
<div class="alert alert--info" role="alert">
  <svg class="alert__icon" viewBox="0 0 20 20" aria-hidden="true">
    <circle cx="10" cy="10" r="10" fill="currentColor" opacity="0.15"/>
    <path d="M10 6v4M10 14h.01" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
  </svg>
  <div class="alert__content">
    <p class="alert__title">Browser update available</p>
    <p class="alert__message">Some features may not work correctly in your current browser version.</p>
    <div class="alert__actions">
      <button class="btn btn-sm btn-primary" type="button">Update browser</button>
      <button class="btn btn-sm btn-ghost" type="button">Dismiss</button>
    </div>
  </div>
  <button class="alert__dismiss" type="button" aria-label="Dismiss alert">
    <svg width="16" height="16" viewBox="0 0 16 16" aria-hidden="true">
      <path d="M4 4l8 8M12 4l-8 8" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
    </svg>
  </button>
</div>

<!-- Persistent error alert (no dismiss) -->
<div class="alert alert--error" role="alert">
  <svg class="alert__icon" viewBox="0 0 20 20" aria-hidden="true">
    <circle cx="10" cy="10" r="10" fill="currentColor" opacity="0.15"/>
    <path d="M7 7l6 6M13 7l-6 6" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
  </svg>
  <div class="alert__content">
    <p class="alert__title">Payment failed</p>
    <p class="alert__message">Your subscription payment could not be processed. Please update your billing information.</p>
    <div class="alert__actions">
      <button class="btn btn-sm btn-primary" type="button">Update billing</button>
    </div>
  </div>
</div>

<!-- Success alert -->
<div class="alert alert--success" role="status">
  <svg class="alert__icon" viewBox="0 0 20 20" aria-hidden="true">
    <circle cx="10" cy="10" r="10" fill="currentColor" opacity="0.15"/>
    <path d="M6 10l3 3 5-5" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round"/>
  </svg>
  <div class="alert__content">
    <p class="alert__title">Settings saved</p>
    <p class="alert__message">Your changes have been applied successfully.</p>
  </div>
  <button class="alert__dismiss" type="button" aria-label="Dismiss alert">
    <!-- close icon -->
  </button>
</div>
```

### Banner Variant (Full-Width)

For system-wide announcements at the very top of the page:

```css
.alert--banner {
  --alert-radius: 0;
  --alert-accent-width: 0;
  border: none;
  border-block-end: 1px solid var(--alert-border-color);
  justify-content: center;
  text-align: center;
  padding: var(--space-3) var(--space-5);
}

.alert--banner .alert__content {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-3);
  flex: unset;
}

.alert--banner .alert__title {
  margin: 0;
}
```

### Inline Variant

For alerts embedded within forms or sections:

```css
.alert--inline {
  --alert-radius: var(--radius-sm);
  --alert-padding: var(--space-3);
  --alert-icon-size: 16px;
  --alert-accent-width: 3px;
  font-size: var(--fs-label);
}
.alert--inline .alert__title {
  font-size: var(--fs-label);
}
```

### Layout Variants

```html
<!-- Banner: full-width at page top -->
<div class="alert alert--warning alert--banner" role="alert">
  <div class="alert__content">
    <svg class="alert__icon" aria-hidden="true"><!-- icon --></svg>
    <span class="alert__title">Scheduled maintenance: May 5, 2026 02:00-04:00 UTC</span>
  </div>
</div>

<!-- Inline: embedded in a form section -->
<div class="alert alert--error alert--inline" role="alert">
  <svg class="alert__icon" aria-hidden="true"><!-- icon --></svg>
  <div class="alert__content">
    <p class="alert__title">3 errors found</p>
    <p class="alert__message">Please fix the highlighted fields below.</p>
  </div>
</div>
```

### Dark Mode

```css
@media (prefers-color-scheme: dark) {
  .alert--info {
    background: color-mix(in srgb, var(--accent) 10%, transparent);
  }
  .alert--warning {
    background: color-mix(in srgb, var(--warning, #f59e0b) 10%, transparent);
  }
  .alert--error {
    background: color-mix(in srgb, var(--error) 10%, transparent);
  }
  .alert--success {
    background: color-mix(in srgb, var(--success) 10%, transparent);
  }
  .alert__dismiss:hover {
    background: rgba(255, 255, 255, 0.1);
  }
}
```

### Responsive Behavior

- Alerts are full-width within their container by default.
- On narrow screens, action buttons stack vertically below the message.
- Banner alerts remain centered and may wrap text to a second line.
- Inline alerts maintain their position relative to the form section they describe.

```css
@media (max-width: 480px) {
  .alert__actions {
    flex-direction: column;
    align-items: flex-start;
  }
  .alert__actions .btn {
    width: 100%;
  }
}
```

### Accessibility

- Use `role="alert"` for error and warning alerts (assertive -- announced immediately).
- Use `role="status"` for info and success alerts (polite -- announced after current task).
- `aria-live="assertive"` or `aria-live="polite"` can be used on the container if `role` is not set directly.
- Dismiss button has `aria-label="Dismiss alert"`.
- Title text provides the primary accessible name; message provides detail.
- Icon is decorative (`aria-hidden="true"`); text carries all meaning.
- When an alert is dismissed, it should be removed from the DOM or have `aria-hidden="true"` applied.
- Color contrast: alert text against alert background must pass WCAG AA (4.5:1).
- Action buttons within alerts are standard interactive elements and follow button accessibility guidelines.
- Banner alerts at the top of the page should appear before the main navigation landmark for proper reading order, or be linked via a skip link.

### Dismissible vs Persistent

- **Dismissible**: Includes close button. Used for transient conditions the user can acknowledge and dismiss (info, success, non-critical warnings).
- **Persistent**: No close button. Used for conditions that remain until the underlying state changes (payment failure, account suspension, system-wide notices). The user must resolve the condition; the alert cannot be hidden.
- **Session-persistent**: Dismissible but reappears on next page load until the underlying condition is resolved (e.g., "Verify your email"). Use `localStorage` or session state to track dismissal within a session.

### States

- **Default**: visible, static background, icon + text + optional actions.
- **Dismissing**: optional exit animation (fade out, slide up).
- **Dismissed**: removed from DOM or `aria-hidden="true"`.
- **Focused**: action buttons and dismiss button have standard focus rings.
