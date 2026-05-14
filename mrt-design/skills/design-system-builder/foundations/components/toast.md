## Design Principles

- **Toasts are non-blocking notifications.** They appear in a fixed corner and never steal focus from the user's current task.
- **Each toast auto-dismisses with a visible timer.** A default of 5 seconds gives users time to read; pausing the timer on hover respects ongoing reading.
- **Variant is communicated through a colored accent bar, not the entire background.** A `border-inline-start` in success/error/warning/info color signals type without overwhelming the layout.
- **Stack newer toasts below older ones, cap at five visible.** More than five creates visual noise; the oldest toast is dismissed to make room.

### CSS
```css
.toast-container {
  position: fixed;
  inset-block-end: var(--space-5);
  inset-inline-end: var(--space-5);
  display: flex;
  flex-direction: column-reverse;
  gap: var(--space-3);
  z-index: 500;
  pointer-events: none;
}
.toast {
  display: flex;
  align-items: flex-start;
  gap: var(--space-3);
  padding: var(--space-3) var(--space-4);
  min-width: 300px;
  max-width: 420px;
  background: var(--bg);
  border: 1px solid var(--border);
  border-radius: var(--radius-md);
  box-shadow: var(--shadow-lg);
  pointer-events: auto;
  animation: toastEnter 0.3s var(--ease) both;
}
.toast--exiting {
  animation: toastExit 0.25s var(--ease) both;
}
@keyframes toastEnter {
  from { opacity: 0; transform: translateX(100%); } /* RTL: use translateX(-100%) */
  to   { opacity: 1; transform: translateX(0); }
}
@keyframes toastExit {
  from { opacity: 1; transform: translateX(0); }
  to   { opacity: 0; transform: translateX(100%); } /* RTL: use translateX(-100%) */
}
/* RTL override */
[dir="rtl"] .toast {
  animation-name: toastEnterRtl;
}
[dir="rtl"] .toast--exiting {
  animation-name: toastExitRtl;
}
@keyframes toastEnterRtl {
  from { opacity: 0; transform: translateX(-100%); }
  to   { opacity: 1; transform: translateX(0); }
}
@keyframes toastExitRtl {
  from { opacity: 1; transform: translateX(0); }
  to   { opacity: 0; transform: translateX(-100%); }
}
.toast__icon {
  flex-shrink: 0;
  width: 20px;
  height: 20px;
  margin-top: 2px;
}
.toast__content {
  flex: 1;
  font-family: var(--font-body);
  font-size: var(--fs-body-sm);
  color: var(--fg);
  line-height: 1.5;
}
.toast__title {
  font-family: var(--font-display);
  font-weight: var(--fw-semibold);
  margin-bottom: var(--space-1);
}
.toast__message {
  color: var(--fg-muted);
}
.toast__dismiss {
  flex-shrink: 0;
  background: transparent;
  border: none;
  color: var(--fg-muted);
  cursor: pointer;
  padding: var(--space-1);
  border-radius: var(--radius-sm);
  transition: color var(--dur-fast) var(--ease), background var(--dur-fast) var(--ease);
}
.toast__dismiss:hover {
  color: var(--fg);
  background: var(--bg-alt);
}
/* Variant accent bars */
.toast--success { border-inline-start: 4px solid var(--success); }
.toast--error   { border-inline-start: 4px solid var(--error); }
.toast--warning { border-inline-start: 4px solid var(--warning, #f59e0b); }
.toast--info    { border-inline-start: 4px solid var(--accent); }
```

### HTML Pattern
```html
<div class="toast-container" id="toast-container" aria-live="polite" aria-atomic="false">
  <!-- Toasts are injected here dynamically -->
  <div class="toast toast--success" role="status" data-toast data-auto-dismiss="5000">
    <svg class="toast__icon" viewBox="0 0 20 20" aria-hidden="true">
      <circle cx="10" cy="10" r="10" fill="var(--success)" opacity="0.15"/>
      <path d="M6 10l3 3 5-5" stroke="var(--success)" stroke-width="2" fill="none" stroke-linecap="round"/>
    </svg>
    <div class="toast__content">
      <div class="toast__title">Saved</div>
      <div class="toast__message">Your changes have been saved successfully.</div>
    </div>
    <button class="toast__dismiss" type="button" aria-label="Dismiss notification">
      <svg width="16" height="16" viewBox="0 0 16 16"><path d="M4 4l8 8M12 4l-8 8" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/></svg>
    </button>
  </div>
</div>
```

### JS Behavior
- **Triggers**: Toasts are created programmatically via a `showToast({ variant, title, message, duration })` function.
- **Auto-dismiss**: Configurable duration (3s-8s, default 5s). Read from `data-auto-dismiss` attribute. A progress bar at the bottom of the toast can visualize remaining time.
- **Manual dismiss**: Click the dismiss button to close immediately.
- **Stacking**: Multiple toasts stack vertically in `flex-direction: column-reverse`. Newer toasts appear at the bottom. Maximum of 5 visible toasts; oldest is dismissed when limit is exceeded.
- **Exit animation**: Add `.toast--exiting` class, then remove the element after the animation ends (`animationend` event).
- **Timer pause**: On `mouseenter`, pause the auto-dismiss timer. On `mouseleave`, resume. This prevents the toast from vanishing while the user is reading it.

### Accessibility
- Container uses `aria-live="polite"` so new toasts are announced without interrupting
- Use `role="status"` for informational toasts (polite) or `role="alert"` for critical toasts (assertive)
- Each toast's dismiss button has `aria-label="Dismiss notification"`
- `aria-atomic="false"` allows screen readers to announce only new toasts, not the entire stack
- Toasts do not receive focus by default; they are non-blocking
- For toasts with actions (e.g., "Undo"), the action link/button is focusable
