## Design Principles

- **The dropdown panel opens below the trigger with a clear visual connection.** A 4px gap plus matching border radius makes the panel feel attached, not floating arbitrarily.
- **Keyboard navigation mirrors native select behavior.** Arrow keys move the highlight, Enter/Space selects, Escape closes -- users should not need to learn a new paradigm.
- **Focus stays on the trigger; visual highlight tracks via aria-activedescendant.** This pattern keeps the accessibility tree clean while giving visual feedback on the highlighted option.
- **Click-outside dismissal is expected.** A document-level pointerdown listener closes the panel when the user clicks elsewhere -- the dropdown never stays open orphaned on the page.
- **Selected option is immediately identifiable.** A checkmark, bold text, and accent color on the selected option confirm the current value at a glance.

### CSS
```css
.select-wrapper {
  position: relative;
  display: inline-block;
  min-width: 200px;
}
.select-trigger {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--space-2);
  width: 100%;
  padding: 10px 14px;
  font-family: var(--font-body);
  font-size: var(--fs-body);
  color: var(--fg);
  background: var(--bg);
  border: 1.5px solid var(--border);
  border-radius: var(--radius-sm);
  cursor: pointer;
  min-height: 44px;
  transition: border-color var(--dur-base) var(--ease), box-shadow var(--dur-base) var(--ease);
}
.select-trigger:focus-visible {
  outline: none;
  border-color: var(--accent);
  box-shadow: 0 0 0 3px var(--accent-tint);
}
.select-trigger[aria-expanded="true"] {
  border-color: var(--accent);
  box-shadow: 0 0 0 3px var(--accent-tint);
}
.select-trigger__icon {
  transition: transform var(--dur-base) var(--ease);
  flex-shrink: 0;
}
.select-trigger[aria-expanded="true"] .select-trigger__icon {
  transform: rotate(180deg);
}
.select-trigger--placeholder {
  color: var(--fg-subtle);
}
.select-panel {
  position: absolute;
  top: calc(100% + 4px);
  left: 0;
  right: 0;
  background: var(--bg);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  box-shadow: var(--shadow-lg);
  max-height: 260px;
  overflow-y: auto;
  z-index: 200;
  padding: var(--space-1) 0;
  opacity: 0;
  visibility: hidden;
  transform: translateY(-4px);
  transition: opacity var(--dur-base) var(--ease),
              transform var(--dur-base) var(--ease),
              visibility var(--dur-base) var(--ease);
}
.select-panel--open {
  opacity: 1;
  visibility: visible;
  transform: translateY(0);
}
.select-option {
  display: flex;
  align-items: center;
  gap: var(--space-2);
  padding: var(--space-2) var(--space-3);
  font-family: var(--font-body);
  font-size: var(--fs-body-sm);
  color: var(--fg);
  cursor: pointer;
  transition: background var(--dur-fast) var(--ease);
}
.select-option:hover,
.select-option--highlighted {
  background: var(--bg-alt);
}
.select-option--selected {
  font-weight: var(--fw-semibold);
  color: var(--accent);
}
.select-option--selected::after {
  content: '✓';
  margin-left: auto;
  font-size: var(--fs-label);
}
.select-option--disabled {
  opacity: 0.4;
  pointer-events: none;
}
```

### HTML Pattern
```html
<div class="select-wrapper" data-select>
  <button class="select-trigger"
          role="combobox"
          type="button"
          aria-haspopup="listbox"
          aria-expanded="false"
          aria-activedescendant=""
          aria-labelledby="select-label-id"
          id="select-trigger-id">
    <span class="select-trigger__text select-trigger--placeholder">Choose an option</span>
    <svg class="select-trigger__icon" width="16" height="16" viewBox="0 0 16 16" aria-hidden="true">
      <path d="M4 6l4 4 4-4" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
    </svg>
  </button>
  <div class="select-panel" role="listbox" aria-labelledby="select-label-id" id="select-panel-id">
    <div class="select-option" role="option" id="opt-1" aria-selected="false" data-value="apple">Apple</div>
    <div class="select-option" role="option" id="opt-2" aria-selected="false" data-value="banana">Banana</div>
    <div class="select-option" role="option" id="opt-3" aria-selected="true" data-value="cherry">Cherry</div>
  </div>
</div>
<!-- Associated visible label -->
<label class="label" id="select-label-id" for="select-trigger-id">Fruit</label>
```

### JS Behavior
- **Triggers**: Click on trigger button opens/closes the panel. Click outside closes. Escape closes.
- **Keyboard**:
  - `ArrowDown` / `ArrowUp`: Move highlight between options. Wrap around at boundaries.
  - `Enter` / `Space`: Select the currently highlighted option and close.
  - `Escape`: Close without selecting.
  - `Home` / `End`: Jump to first / last option.
- **Focus management**: On open, highlight the selected option (or first option if none). Scroll the highlighted option into view (`scrollIntoView({ block: 'nearest' })`). On close, return focus to the trigger button.
- **State sync**: Update `aria-activedescendant` on the trigger to match the highlighted option's `id`. Set `aria-selected="true"` on the chosen option, `false` on all others. Update the trigger text to reflect the selected label.

### Accessibility
- Trigger uses `role="combobox"` with `aria-haspopup="listbox"` and `aria-expanded`
- Panel uses `role="listbox"`, options use `role="option"` with `aria-selected`
- `aria-activedescendant` on trigger tracks the visually highlighted option
- Click-outside dismissal via a document-level pointerdown listener
- Focus stays on the trigger; visual highlight is managed via `aria-activedescendant`
- Minimum touch target: 44x44px on trigger
