## Design Principles

- **Command palette is keyboard-first.** Cmd+K (Mac) / Ctrl+K (Windows) opens it. Arrow keys navigate results. Enter selects. Escape closes.
- **Search is instant.** Every keystroke filters results in real-time. No debounce below 150ms.
- **Results are grouped by category.** Actions, pages, settings, and recent items are visually separated with group headers.
- **The palette opens centered with a backdrop.** It should feel like a search overlay, not a modal — lighter visual weight, no heavy shadow.
- **Empty state provides guidance.** "No results for 'xyz'. Try searching for actions, pages, or settings."

## Brand Expression

The command palette is a utility surface. Apply `creative_brief` sparingly:

- `safe`: standard centered search with neutral border, minimal shadow.
- `elevated`: accent-tinted active result, branded keyboard shortcut badges.
- `bold`: custom result card layout, animated enter, or branded empty state illustration.
- `experimental`: full-bleed command palette with category tabs or AI-powered suggestions.

### Component Tokens

```css
.command-palette {
  --cp-max-width: 560px;
  --cp-bg: var(--bg);
  --cp-border: 1px solid var(--border);
  --cp-radius: var(--radius-lg);
  --cp-shadow: var(--shadow-lg);
  --cp-input-bg: var(--bg);
  --cp-input-border: 1px solid var(--border);
  --cp-result-hover: var(--bg-alt);
  --cp-result-active-bg: var(--accent-tint);
  --cp-result-active-fg: var(--accent);
  --cp-group-color: var(--fg-muted);
  --cp-kbd-bg: var(--bg-alt);
  --cp-kbd-border: 1px solid var(--border);
}
```

### CSS

```css
.cp-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  padding-block-start: 15vh;
  z-index: var(--z-modal, 300);
  backdrop-filter: blur(4px);
  animation: cpOverlayIn var(--dur-fast) var(--ease) both;
}
@keyframes cpOverlayIn {
  from { opacity: 0; }
  to   { opacity: 1; }
}

.command-palette {
  background: var(--cp-bg);
  border: var(--cp-border);
  border-radius: var(--cp-radius);
  box-shadow: var(--cp-shadow);
  width: 90%;
  max-width: var(--cp-max-width);
  max-height: 60vh;
  display: flex;
  flex-direction: column;
  animation: cpSlideIn var(--dur-base) var(--ease) both;
}
@keyframes cpSlideIn {
  from { opacity: 0; transform: translateY(-8px) scale(0.98); }
  to   { opacity: 1; transform: translateY(0) scale(1); }
}

.cp-input-wrapper {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-4);
  border-block-end: 1px solid var(--border);
}

.cp-input-wrapper svg {
  flex-shrink: 0;
  color: var(--fg-muted);
}

.cp-search {
  flex: 1;
  border: none;
  background: transparent;
  font-family: var(--font-body);
  font-size: var(--fs-body);
  color: var(--fg);
  outline: none;
}

.cp-search::placeholder {
  color: var(--fg-subtle);
}

.cp-results {
  overflow-y: auto;
  padding: var(--space-2) 0;
  flex: 1;
}

.cp-group-label {
  padding: var(--space-2) var(--space-4);
  font-family: var(--font-body);
  font-size: var(--fs-label);
  font-weight: var(--fw-semibold);
  color: var(--cp-group-color);
  letter-spacing: var(--tracking-label);
  text-transform: uppercase;
}

.cp-result {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-2) var(--space-4);
  cursor: pointer;
  transition: background var(--dur-micro) var(--ease);
}

.cp-result:hover,
.cp-result--active {
  background: var(--cp-result-hover);
}

.cp-result--selected {
  background: var(--cp-result-active-bg);
  color: var(--cp-result-active-fg);
}

.cp-result__icon {
  flex-shrink: 0;
  width: 20px;
  height: 20px;
  color: var(--fg-muted);
}

.cp-result__label {
  flex: 1;
  font-size: var(--fs-body-sm);
}

.cp-result__kbd {
  display: inline-flex;
  align-items: center;
  padding: 2px 6px;
  background: var(--cp-kbd-bg);
  border: var(--cp-kbd-border);
  border-radius: var(--radius-sm);
  font-family: var(--font-mono);
  font-size: 11px;
  color: var(--fg-muted);
}

.cp-empty {
  padding: var(--space-6) var(--space-4);
  text-align: center;
  color: var(--fg-muted);
  font-size: var(--fs-body-sm);
}

.cp-footer {
  padding: var(--space-2) var(--space-4);
  border-block-start: 1px solid var(--border);
  display: flex;
  gap: var(--space-4);
  font-size: var(--fs-label);
  color: var(--fg-subtle);
}
```

### HTML Pattern

```html
<div class="cp-overlay" role="dialog" aria-modal="true" aria-label="Command palette">
  <div class="command-palette" role="combobox" aria-expanded="true" aria-haspopup="listbox">
    <div class="cp-input-wrapper">
      <svg width="20" height="20" viewBox="0 0 20 20" aria-hidden="true">
        <circle cx="9" cy="9" r="6" fill="none" stroke="currentColor" stroke-width="1.5"/>
        <line x1="13.5" y1="13.5" x2="18" y2="18" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
      </svg>
      <input class="cp-search" type="text" placeholder="Search actions, pages, settings..." aria-autocomplete="list" aria-controls="cp-results"/>
    </div>
    <div class="cp-results" id="cp-results" role="listbox">
      <div class="cp-group-label" role="presentation">Actions</div>
      <div class="cp-result cp-result--selected" role="option" aria-selected="true">
        <svg class="cp-result__icon"><!-- icon --></svg>
        <span class="cp-result__label">Create new project</span>
        <kbd class="cp-result__kbd">N</kbd>
      </div>
    </div>
    <div class="cp-footer">
      <span><kbd>&uarr;&darr;</kbd> Navigate</span>
      <span><kbd>&crarr;</kbd> Select</span>
      <span><kbd>Esc</kbd> Close</span>
    </div>
  </div>
</div>
```

### Dark Mode

```css
[data-theme="dark"] .command-palette {
  --cp-bg: var(--bg-alt);
  --cp-border: 1px solid var(--border);
  --cp-shadow: 0 16px 48px rgba(0, 0, 0, 0.5);
  --cp-result-hover: rgba(255, 255, 255, 0.06);
}
```

### Accessibility

- `role="dialog"` and `aria-modal="true"` on the overlay.
- Input has `role="combobox"` with `aria-autocomplete="list"` and `aria-controls`.
- Results container has `role="listbox"`. Each result has `role="option"` with `aria-selected`.
- Keyboard: Arrow Up/Down navigates, Enter selects, Escape closes.
- Focus trap: focus stays within the palette while open. Returns to trigger on close.
- Announce result count on filter: `aria-live="polite"` on a hidden counter.

### Responsive Behavior

- Below 640px, palette stretches to full width minus `--space-4` margin on each side.
- Touch targets for results increase to 44px height.
- Footer hides on very small screens (< 380px) to preserve result space.
