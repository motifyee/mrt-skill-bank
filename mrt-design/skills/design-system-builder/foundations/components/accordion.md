## Design Principles

- **Each accordion item is independently expandable.** Single mode (one-at-a-time) and multiple mode (any open) are both valid; the choice depends on whether users need to compare sections side-by-side.
- **The expand/collapse animation uses CSS grid rows, not JavaScript heights.** `grid-template-rows: 0fr` to `1fr` provides a smooth, native-feeling transition without measuring DOM heights.
- **The chevron icon rotates to indicate state.** A 180-degree rotation on `aria-expanded="true"` gives instant visual feedback of open vs closed without relying on text changes.
- **Full-width trigger buttons ensure the entire header is clickable.** Accordion triggers span the full row width so users do not need to precisely target a small icon.

### CSS
```css
.accordion {
  display: flex;
  flex-direction: column;
  border: 1px solid var(--border);
  border-radius: var(--radius-md);
  overflow: hidden;
}
.accordion-item {
  border-bottom: 1px solid var(--border);
}
.accordion-item:last-child {
  border-bottom: none;
}
.accordion-trigger {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--space-3);
  width: 100%;
  padding: var(--space-4) var(--space-5);
  font-family: var(--font-display);
  font-size: var(--fs-body);
  font-weight: var(--fw-semibold);
  color: var(--fg);
  background: var(--bg);
  border: none;
  cursor: pointer;
  text-align: left;
  min-height: 44px;
  transition: background var(--dur-fast) var(--ease);
}
.accordion-trigger:hover {
  background: var(--bg-alt);
}
.accordion-trigger:focus-visible {
  outline: 2px solid var(--focus-ring);
  outline-offset: -2px;
}
.accordion-trigger__icon {
  flex-shrink: 0;
  transition: transform var(--dur-base) var(--ease);
}
.accordion-trigger[aria-expanded="true"] .accordion-trigger__icon {
  transform: rotate(180deg);
}
.accordion-content {
  display: grid;
  grid-template-rows: 0fr;
  transition: grid-template-rows var(--dur-base) var(--ease);
}
.accordion-item--open .accordion-content {
  grid-template-rows: 1fr;
}
.accordion-content__inner {
  overflow: hidden;
}
.accordion-content__text {
  padding: 0 var(--space-5) var(--space-4) var(--space-5);
  font-family: var(--font-body);
  font-size: var(--fs-body-sm);
  color: var(--fg-muted);
  line-height: 1.6;
}
```

### HTML Pattern
```html
<div class="accordion" data-accordion data-allow-multiple="false">
  <div class="accordion-item accordion-item--open" data-accordion-item>
    <button class="accordion-trigger"
            type="button"
            aria-expanded="true"
            aria-controls="acc-panel-1"
            id="acc-trigger-1">
      <span>What is your return policy?</span>
      <svg class="accordion-trigger__icon" width="20" height="20" viewBox="0 0 20 20" aria-hidden="true">
        <path d="M5 8l5 5 5-5" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
      </svg>
    </button>
    <div class="accordion-content" id="acc-panel-1" role="region" aria-labelledby="acc-trigger-1">
      <div class="accordion-content__inner">
        <p class="accordion-content__text">
          We offer a 30-day return policy on all unused items in original packaging.
          Please contact support to initiate a return.
        </p>
      </div>
    </div>
  </div>
  <div class="accordion-item" data-accordion-item>
    <button class="accordion-trigger"
            type="button"
            aria-expanded="false"
            aria-controls="acc-panel-2"
            id="acc-trigger-2">
      <span>How long does shipping take?</span>
      <svg class="accordion-trigger__icon" width="20" height="20" viewBox="0 0 20 20" aria-hidden="true">
        <path d="M5 8l5 5 5-5" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
      </svg>
    </button>
    <div class="accordion-content" id="acc-panel-2" role="region" aria-labelledby="acc-trigger-2">
      <div class="accordion-content__inner">
        <p class="accordion-content__text">
          Standard shipping takes 5-7 business days. Express options are available at checkout.
        </p>
      </div>
    </div>
  </div>
</div>
```

### JS Behavior
- **Triggers**: Click on a trigger toggles the associated panel open/closed.
- **Single mode** (`data-allow-multiple="false"`): Opening one item closes all others.
- **Multiple mode** (`data-allow-multiple="true"`): Each item toggles independently.
- **Keyboard**:
  - `Enter` / `Space`: Toggle the focused trigger.
  - `ArrowDown`: Move focus to the next trigger. Wrap to first at end.
  - `ArrowUp`: Move focus to the previous trigger. Wrap to last at start.
  - `Home`: Focus first trigger.
  - `End`: Focus last trigger.
- **Focus management**: Focus stays on the trigger button. The animated expand/collapse uses `grid-template-rows: 0fr` to `1fr` for smooth height transitions without JavaScript height calculation.

### Accessibility
- Each trigger is a `<button>` with `aria-expanded` and `aria-controls` (panel id)
- Each panel uses `role="region"` with `aria-labelledby` (trigger id)
- The `grid-template-rows: 0fr/1fr` technique provides smooth animation without needing explicit height values or `max-height` hacks
- Keyboard navigation follows WAI-ARIA Accordion pattern
- Icon rotation indicates expand/collapse state visually
