# Micro-interactions

Loading states, skeleton screens, progress indicators, toast notifications,
empty states, and other micro-interaction patterns. These small moments shape
how reliable and responsive an interface feels.

## Design Principles

1. **Match skeleton shape to actual content layout:** A card skeleton must mirror the card's image, title, and body structure so the transition from loading to loaded feels seamless.
2. **Choose the right loading indicator for the wait type:** Use skeleton screens for known layouts, progress bars for determinate durations, and spinners only for indeterminate short waits under 2 seconds.
3. **Every empty state explains why and offers a next action:** Provide the reason (first visit, no results, permission issue), a clear CTA, and an illustration or icon — never just a blank canvas.
4. **Destructive actions require explicit confirmation:** Use modal dialogs for irreversible deletes, toast-based undo patterns for reversible actions with a 5-second auto-confirm window.

---

## Loading States

### Skeleton Screens

Content-shaped placeholders that preview layout before data arrives.

```css
.skeleton {
  background: var(--bg-alt);
  border-radius: var(--radius-sm);
  position: relative;
  overflow: hidden;
  color: transparent !important;
}
.skeleton::after {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(
    90deg,
    transparent 0%,
    var(--bg-hover) 40%,
    var(--bg-hover) 60%,
    transparent 100%
  );
  animation: shimmer 1.8s var(--ease) infinite;
}
@keyframes shimmer {
  from { transform: translateX(-100%); }
  to   { transform: translateX(100%); }
}

/* Shape variants */
.skeleton-text   { height: 14px; margin-bottom: var(--space-2); border-radius: var(--radius-sm); }
.skeleton-title  { height: 24px; width: 60%; margin-bottom: var(--space-3); }
.skeleton-avatar { width: 40px; height: 40px; border-radius: var(--radius-full); }
.skeleton-image  { width: 100%; aspect-ratio: 16 / 9; }
```

Match skeleton shape to actual content layout. A card skeleton should mirror
the card's image, title, and body text structure. Color uses `var(--bg-alt)`
with an animated gradient shimmer on `var(--bg-hover)`.

### Spinner

For indeterminate waits longer than 2 seconds.

```css
.spinner {
  width: var(--spinner-size, 24px);
  height: var(--spinner-size, 24px);
  border: 2.5px solid var(--border);
  border-top-color: var(--accent);
  border-radius: var(--radius-full);
  animation: spin 700ms linear infinite;
}
.spinner--sm { --spinner-size: 16px; border-width: 2px; }
.spinner--md { --spinner-size: 24px; }
.spinner--lg { --spinner-size: 32px; border-width: 3px; }

@keyframes spin {
  to { transform: rotate(360deg); }
}
```

```html
<div class="spinner spinner--md" role="status" aria-label="Loading">
  <span class="sr-only">Loading...</span>
</div>
```

### Progress Bar

For determinate progress where completion percentage is known.

```css
.progress-bar {
  width: 100%;
  height: 6px;
  background: var(--bg-alt);
  border-radius: var(--radius-full);
  overflow: hidden;
}
.progress-bar__fill {
  height: 100%;
  background: var(--accent);
  border-radius: var(--radius-full);
  transition: width var(--dur-base) var(--ease);
  width: var(--progress, 0%);
}
```

Circular variant:

```css
.progress-ring {
  --ring-size: 48px;
  --ring-stroke: 4px;
  width: var(--ring-size);
  height: var(--ring-size);
}
.progress-ring circle {
  fill: none;
  stroke-width: var(--ring-stroke);
  stroke-linecap: round;
  transition: stroke-dashoffset var(--dur-base) var(--ease);
}
.progress-ring .track { stroke: var(--bg-alt); }
.progress-ring .fill  { stroke: var(--accent); transform: rotate(-90deg); transform-origin: center; }
```

### Progress Steps

For multi-step flows with completed, active, and upcoming states.

```css
.steps {
  display: flex;
  align-items: center;
  gap: 0;
}
.step {
  display: flex;
  align-items: center;
  gap: var(--space-2);
  font-size: var(--fs-label);
  color: var(--fg-muted);
}
.step__indicator {
  width: 28px;
  height: 28px;
  border-radius: var(--radius-full);
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: var(--fw-semibold);
  border: 2px solid var(--border);
  background: var(--bg);
  transition: all var(--dur-fast) var(--ease);
}
.step--completed .step__indicator {
  background: var(--accent);
  border-color: var(--accent);
  color: var(--on-accent);
}
.step--active .step__indicator {
  border-color: var(--accent);
  color: var(--accent);
}
.step-connector {
  flex: 1;
  height: 2px;
  background: var(--border);
  margin: 0 var(--space-2);
}
.step--completed + .step-connector { background: var(--accent); }
```

---

## Toast / Notification System

Canonical component implementation lives in `foundations/components/toast.md`.
This file only defines when to use toasts: non-blocking feedback, reversible undo
actions, and short-lived status updates. Do not duplicate toast CSS here; agents
should read the canonical component file when generating toast markup/styles.

---

## Empty States

Design patterns for when there is no data to display.

```css
.empty-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: var(--space-10) var(--space-6);
  gap: var(--space-4);
}
.empty-state__icon {
  width: 64px;
  height: 64px;
  color: var(--fg-muted);
}
.empty-state__title {
  font-size: var(--fs-h4);
  font-weight: var(--fw-semibold);
  color: var(--fg);
}
.empty-state__description {
  font-size: var(--fs-body);
  color: var(--fg-muted);
  max-width: 320px;
}
```

Structure: illustration or icon, primary message, secondary guidance, CTA
button. Example: icon of an empty folder, "No items yet", "Create your first
item to get started.", and a primary button labeled "Create Item".

---

## Confirmation Patterns

### Destructive Actions

Modal confirmation with a clearly marked danger button.

```css
.confirm-dialog {
  /* reuse modal pattern */
  max-width: 400px;
}
.confirm-dialog__body {
  padding: var(--space-6);
}
.btn-danger {
  background: var(--error);
  color: var(--on-error, #fff);
}
.btn-danger:hover { background: var(--error-hover); }
```

### Undo Pattern

Toast with an undo link. Action is auto-confirmed after 5 seconds.

```html
<div class="toast toast--info" role="status">
  <span>Item deleted.</span>
  <button class="toast__undo" style="color: var(--accent); font-weight: var(--fw-semibold);">Undo</button>
</div>
```

### Inline Confirmation

For minor actions: replace the trigger button with "Are you sure?" and a
confirm/cancel pair. Revert after 3 seconds of inactivity.

---

## Component Implementation References

`foundations/components/` is canonical for component-level CSS, HTML, JS, and
accessibility guidance. Use these files instead of duplicating implementations here:

- Toggle / switch: `foundations/components/toggle.md`
- Tooltip: `foundations/components/tooltip.md`
- Toast / notification: `foundations/components/toast.md`

Micro-interaction guidance in this file should describe behavior, timing, loading,
empty, and confirmation patterns only.


