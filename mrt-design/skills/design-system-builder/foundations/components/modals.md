## Design Principles

- **Modals demand attention and must justify the interruption.** Use modals only for confirmations, focused tasks, or critical information that requires a decision before proceeding.
- **Focus never escapes an open modal.** A focus trap cycles through interactive elements within the modal; Tab and Shift+Tab never reach the obscured page behind it.
- **Escape always closes.** The Escape key is a universal dismissal mechanism; the close button in the corner is the visual equivalent.
- **The overlay dims and blurs the background to establish depth.** `backdrop-filter: blur(4px)` combined with a semi-transparent black overlay ensures the modal is unambiguously in the foreground.
- **Focus returns to the trigger on close.** After the modal dismisses, focus must land on the element that opened it so keyboard users do not lose their place.

## Brand Expression

Modals should inherit the system's component contract without becoming decorative.
Apply `creative_brief` through shell shape, divider style, focus ring, and action
layout:

- `safe`: standard centered dialog with restrained border/shadow.
- `elevated`: branded header divider, signature focus ring, or subtle accent rail.
- `bold`: side sheet, stepped dialog, or asymmetric header treatment when it improves task clarity.
- `experimental`: only for non-critical marketing/onboarding dialogs, never destructive confirmations.

Do not apply decorative signature moments near irreversible actions unless the
effect improves confidence and orientation.

### Component Tokens

```css
.modal {
  --modal-radius: var(--radius-lg);
  --modal-max-width: 560px;
  --modal-bg: var(--bg);
  --modal-shadow: var(--shadow-lg);
  --modal-padding: var(--space-7);
  --modal-header-border: 1px solid var(--border);
  --modal-footer-bg: transparent;
  --modal-footer-border: 1px solid var(--border);
}
```

### CSS

```css
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  backdrop-filter: blur(4px);
  animation: overlayEnter 0.2s var(--ease) both;
}
@keyframes overlayEnter {
  from { opacity: 0; }
  to   { opacity: 1; }
}

.modal {
  background: var(--modal-bg);
  border-radius: var(--modal-radius);
  max-width: var(--modal-max-width);
  width: 90%;
  max-height: 85vh;
  display: flex;
  flex-direction: column;
  box-shadow: var(--modal-shadow);
  animation: modalEnter 0.25s var(--ease) both;
}
@keyframes modalEnter {
  from { opacity: 0; transform: translateY(16px) scale(0.98); }
  to   { opacity: 1; transform: translateY(0) scale(1); }
}

.modal--exiting {
  animation: modalExit 0.15s var(--ease) both;
}
@keyframes modalExit {
  from { opacity: 1; transform: translateY(0) scale(1); }
  to   { opacity: 0; transform: translateY(8px) scale(0.98); }
}

/* Header */
.modal__header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--modal-padding);
  padding-block-end: var(--space-4);
  border-block-end: var(--modal-header-border);
  flex-shrink: 0;
}

.modal__title {
  font-family: var(--font-display);
  font-size: var(--fs-h4);
  font-weight: var(--fw-semibold);
  color: var(--fg);
  margin: 0;
}

.modal__close {
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
.modal__close:hover {
  color: var(--fg);
  background: var(--bg-alt);
}
.modal__close:focus-visible {
  outline: 2px solid var(--focus-ring);
  outline-offset: 2px;
}

/* Body */
.modal__body {
  padding: var(--space-5) var(--modal-padding);
  overflow-y: auto;
  flex: 1;
}

/* Footer / Actions */
.modal__footer {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  gap: var(--space-3);
  padding: var(--space-4) var(--modal-padding);
  border-block-start: var(--modal-footer-border);
  background: var(--modal-footer-bg);
  flex-shrink: 0;
}
```

### HTML Pattern

```html
<!-- Confirmation Modal -->
<div class="modal-overlay" role="dialog" aria-modal="true" aria-labelledby="modal-title-1">
  <div class="modal" role="document">
    <div class="modal__header">
      <h2 class="modal__title" id="modal-title-1">Delete Item?</h2>
      <button class="modal__close" type="button" aria-label="Close dialog">
        <svg width="16" height="16" viewBox="0 0 16 16" aria-hidden="true">
          <path d="M4 4l8 8M12 4l-8 8" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
        </svg>
      </button>
    </div>
    <div class="modal__body">
      <p>This action cannot be undone. The item and all associated data will be permanently removed.</p>
    </div>
    <div class="modal__footer">
      <button class="btn btn-secondary" type="button">Cancel</button>
      <button class="btn btn-destructive" type="button">Delete</button>
    </div>
  </div>
</div>
```

### Multi-Step Modal

```html
<div class="modal-overlay" role="dialog" aria-modal="true" aria-labelledby="modal-title-step">
  <div class="modal modal--wizard">
    <div class="modal__header">
      <div class="modal__stepper">
        <span class="modal__step modal__step--active" aria-current="step">1. Details</span>
        <span class="modal__step">2. Configure</span>
        <span class="modal__step">3. Review</span>
      </div>
      <button class="modal__close" type="button" aria-label="Close dialog">
        <!-- close icon -->
      </button>
    </div>
    <div class="modal__body">
      <!-- Step content injected dynamically -->
    </div>
    <div class="modal__footer">
      <button class="btn btn-secondary" type="button" disabled>Back</button>
      <button class="btn btn-primary" type="button">Next</button>
    </div>
  </div>
</div>
```

```css
.modal__stepper {
  display: flex;
  gap: var(--space-4);
  font-family: var(--font-display);
  font-size: var(--fs-label);
  font-weight: var(--fw-medium);
  color: var(--fg-muted);
}
.modal__step--active {
  color: var(--accent);
  font-weight: var(--fw-semibold);
}
.modal__step--completed {
  color: var(--success);
}
```

### Side Sheet / Drawer

```css
.modal-overlay--sheet {
  justify-content: flex-end;
}

.modal--sheet {
  --modal-max-width: 480px;
  height: 100vh;
  max-height: 100vh;
  border-radius: var(--modal-radius) 0 0 var(--modal-radius);
  animation: sheetEnter 0.3s var(--ease) both;
}
@keyframes sheetEnter {
  from { transform: translateX(100%); }
  to   { transform: translateX(0); }
}

/* RTL support */
[dir="rtl"] .modal--sheet {
  border-radius: 0 var(--modal-radius) var(--modal-radius) 0;
}
@supports (animation-name: sheetEnter) {
  [dir="rtl"] .modal--sheet {
    animation-name: sheetEnterRtl;
  }
}
@keyframes sheetEnterRtl {
  from { transform: translateX(-100%); }
  to   { transform: translateX(0); }
}
```

### Scrolling Behavior

- **Short content**: Modal body has no scroll. Footer stays at the bottom.
- **Long content**: Modal body scrolls independently. Header and footer are fixed (sticky within the modal).
- **Footer remains visible**: When body scrolls, the footer with actions stays accessible.

```css
.modal__header {
  position: sticky;
  inset-block-start: 0;
  z-index: 1;
  background: var(--modal-bg);
}

.modal__footer {
  position: sticky;
  inset-block-end: 0;
  z-index: 1;
  background: var(--modal-bg);
}
```

### Stacked Modals

Stacked modals are strongly discouraged. If unavoidable:

- Each overlay increases z-index by +1 (base 1000, then 1001, 1002).
- Only the topmost modal is interactive; all others receive `pointer-events: none` and `inert` attribute.
- Backdrop opacity compounds: each additional overlay adds ~0.1 opacity.
- Escape dismisses only the top modal.
- Announce context: the top modal's `aria-label` should clarify the relationship (e.g., "Change plan inside Create Project").

```css
.modal-overlay--stacked {
  z-index: 1001;
  background: rgba(0, 0, 0, 0.6);
}
.modal-overlay--stacked .modal {
  max-width: 480px; /* slightly smaller to visually indicate nesting */
}
```

### Size Variants

```css
/* Small — confirmations, simple alerts */
.modal--sm {
  --modal-max-width: 400px;
}

/* Default — forms, detail views */
/* Uses base tokens */

/* Large — complex forms, multi-column content */
.modal--lg {
  --modal-max-width: 720px;
}

/* Full screen — immersive editors, onboarding */
.modal--fullscreen {
  --modal-max-width: 100%;
  --modal-radius: 0;
  width: 100%;
  height: 100vh;
  max-height: 100vh;
}
```

### Dark Mode

```css
[data-theme="dark"] .modal-overlay {
  background: rgba(0, 0, 0, 0.7);
}
[data-theme="dark"] .modal {
  --modal-bg: var(--bg-alt);
  --modal-shadow: 0 8px 32px rgba(0, 0, 0, 0.5);
  --modal-header-border: 1px solid var(--border);
  --modal-footer-border: 1px solid var(--border);
}
```

### Responsive Behavior

- Below 640px, modals become full-screen sheets with rounded top corners only.
- Side sheets on mobile become full-screen overlays.
- Close button is always visible and reachable on small screens.
- Action buttons stack vertically on narrow modals.

```css
@media (max-width: 640px) {
  .modal {
    width: 100%;
    max-width: 100%;
    max-height: 100vh;
    height: 100vh;
    border-radius: var(--modal-radius) var(--modal-radius) 0 0;
    margin-block-start: auto;
  }
  .modal-overlay {
    align-items: flex-end;
  }
  .modal__footer {
    flex-direction: column-reverse;
  }
  .modal__footer .btn {
    width: 100%;
  }
}
```

### Modal Accessibility

- `role="dialog"` and `aria-modal="true"` on the overlay.
- `aria-labelledby` pointing to the modal title's `id`.
- `aria-describedby` pointing to the modal body or a description element when present.
- Trap focus within the modal using `inert` on all siblings of the overlay, or a JS focus trap.
- Close on Escape key.
- Return focus to trigger element on close.
- Modal uses `role="document"` on the inner container when nested inside the dialog overlay.
- Close button has `aria-label="Close dialog"`.
- Multi-step modals announce step changes via `aria-live="polite"` on a step indicator.
- Side sheets should announce their presence: `aria-label="Settings panel"` or similar.

### Required Variants

- Confirmation modal: compact, clear primary/destructive action, explicit cancel.
- Form modal: scrollable body, sticky action row when content is long.
- Side sheet/drawer: use for settings, filters, or inspect/edit flows.
- Multi-step modal: stepper/header progress, back/next controls, preserved state.
- Alert/modal dialog: single message with acknowledge button, `role="alertdialog"`.

### Edge Cases

- Stacked modals are discouraged. If unavoidable, only the top modal is interactive.
- Long content scrolls inside the modal body, not the page.
- Mobile modals become full-screen sheets with the same focus rules.
- If the trigger element is removed from the DOM while the modal is open, return focus to the document body.
- Modals should not open from within another modal's body; use inline expansion or multi-step flow instead.
