## Design Principles

- **The toggle visually communicates binary state through thumb position.** Thumb left = off, thumb right = on -- spatial position is the primary signal, reinforced by track color change.
- **The native checkbox is the real interactive element.** A visually hidden `<input type="checkbox">` with `role="switch"` ensures every mouse click, keyboard press, and screen reader interaction works without custom event handling.
- **Loading state prevents premature toggling.** During async saves, a pulsing thumb animation signals "processing" and the control becomes non-interactive until the operation completes.
- **The label text describes the effect of the "on" state.** "Enable notifications" tells the user what happens when toggled on; "Notifications" alone is ambiguous.

### CSS
```css
.toggle {
  display: inline-flex;
  align-items: center;
  gap: var(--space-3);
  cursor: pointer;
}
.toggle__track {
  position: relative;
  width: 44px;
  height: 24px;
  background: var(--border-strong);
  border-radius: var(--radius-full);
  transition: background var(--dur-base) var(--ease);
  flex-shrink: 0;
}
.toggle--active .toggle__track {
  background: var(--accent);
}
.toggle--disabled {
  opacity: 0.4;
  cursor: not-allowed;
  pointer-events: none;
}
.toggle__thumb {
  position: absolute;
  top: 2px;
  left: 2px;
  width: 20px;
  height: 20px;
  background: var(--bg);
  border-radius: var(--radius-full);
  box-shadow: var(--shadow-sm);
  transition: transform var(--dur-base) var(--ease);
}
.toggle--active .toggle__thumb {
  transform: translateX(20px);
}
.toggle__label {
  font-family: var(--font-display);
  font-size: var(--fs-body-sm);
  font-weight: var(--fw-medium);
  color: var(--fg);
  user-select: none;
}
/* Loading state */
.toggle--loading .toggle__thumb {
  animation: togglePulse 1s var(--ease) infinite;
}
@keyframes togglePulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}
```

### HTML Pattern
```html
<label class="toggle toggle--active" data-toggle>
  <input type="checkbox"
         role="switch"
         aria-checked="true"
         checked
         class="toggle__input"
         id="toggle-notifications"
         style="position:absolute;width:1px;height:1px;overflow:hidden;clip:rect(0,0,0,0);white-space:nowrap;" />
  <span class="toggle__track">
    <span class="toggle__thumb"></span>
  </span>
  <span class="toggle__label">Enable notifications</span>
</label>
```

### JS Behavior
- **Triggers**: Click on the label/track toggles the switch. The native checkbox handles the click event.
- **Keyboard**: `Space` or `Enter` toggles (native checkbox behavior when focused).
- **State sync**: Toggle the `.toggle--active` class on the root element. Update `aria-checked` to match the checkbox's `checked` state.
- **Loading state**: Add `.toggle--loading` class during async operations (e.g., saving a preference to the server). Remove loading state on success/failure. Prevent further toggles during loading.
- **Disabled state**: Set the `disabled` attribute on the input. The `.toggle--disabled` class is applied via CSS `:disabled` selector or manually.

### Accessibility
- Native `<input type="checkbox">` with `role="switch"` and `aria-checked`
- The input is visually hidden but remains in the tab order and accessible to screen readers
- Clicking the `<label>` toggles the input (via `for`/`id` association or wrapping)
- Focus-visible ring should appear on the track when the input is focused (add `:focus-visible + .toggle__track` outline)
- Minimum touch target: 44x44px (the track is 44x24, label adds tap area)
- State change should be announced; `aria-checked` update handles this
