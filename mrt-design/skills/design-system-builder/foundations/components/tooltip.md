## Design Principles

- **Tooltips are supplementary, never essential.** If the tooltip content is required to understand the UI, it belongs in visible text -- not behind a hover.
- **Show after a short delay, hide immediately on leave.** A 300ms hover delay prevents flickering on casual mouse passes; hiding on mouse-leave keeps the UI responsive.
- **Only one tooltip visible at a time.** Opening a new tooltip dismisses any existing one to avoid overlapping popup clutter.
- **Tooltip content is a single short sentence.** For richer content with links or multiple lines, use a popover or dialog component instead.

### CSS
```css
.tooltip-wrapper {
  position: relative;
  display: inline-flex;
}
.tooltip {
  position: absolute;
  z-index: 300;
  padding: var(--space-2) var(--space-3);
  font-family: var(--font-body);
  font-size: var(--fs-label);
  line-height: 1.4;
  color: var(--fg-on-inverted, #fff);
  background: var(--fg);
  border-radius: var(--radius-sm);
  white-space: nowrap;
  pointer-events: none;
  opacity: 0;
  transform: scale(0.95);
  transition: opacity var(--dur-base) var(--ease), transform var(--dur-base) var(--ease);
}
.tooltip--visible {
  opacity: 1;
  transform: scale(1);
}
/* Placement variants */
.tooltip--top {
  bottom: calc(100% + 8px);
  left: 50%;
  transform-origin: bottom center;
  translate: -50% 0;
}
.tooltip--top.tooltip--visible {
  translate: -50% 0;
}
.tooltip--bottom {
  top: calc(100% + 8px);
  left: 50%;
  transform-origin: top center;
  translate: -50% 0;
}
.tooltip--right {
  left: calc(100% + 8px);
  top: 50%;
  transform-origin: center left;
  translate: 0 -50%;
}
.tooltip--left {
  right: calc(100% + 8px);
  top: 50%;
  transform-origin: center right;
  translate: 0 -50%;
}
/* Arrow / caret */
.tooltip::after {
  content: '';
  position: absolute;
  width: 8px;
  height: 8px;
  background: var(--fg);
  transform: rotate(45deg);
}
.tooltip--top::after {
  bottom: -4px;
  left: 50%;
  margin-left: -4px;
}
.tooltip--bottom::after {
  top: -4px;
  left: 50%;
  margin-left: -4px;
}
.tooltip--right::after {
  left: -4px;
  top: 50%;
  margin-top: -4px;
}
.tooltip--left::after {
  right: -4px;
  top: 50%;
  margin-top: -4px;
}
```

### HTML Pattern
```html
<span class="tooltip-wrapper">
  <button class="btn btn-ghost" type="button"
          aria-describedby="tooltip-copy"
          data-tooltip data-tooltip-placement="top"
          data-tooltip-delay="300">
    Copy
  </button>
  <span class="tooltip tooltip--top"
        role="tooltip"
        id="tooltip-copy">
    Copy to clipboard
  </span>
</span>
```

### JS Behavior
- **Triggers**: Show on hover (`mouseenter`) and focus (`focusin`). Hide on `mouseleave` and `blur`.
- **Delay**: Show after 300ms hover/focus (configurable via `data-tooltip-delay`). Hide immediately on leave/blur.
- **Dismiss**: `Escape` key hides the tooltip immediately.
- **Placement**: Read from `data-tooltip-placement` (`top` | `bottom` | `left` | `right`). Optionally auto-flip if the tooltip overflows the viewport (use `getBoundingClientRect()` check).
- **Multiple**: Only one tooltip visible at a time; showing one hides any other open tooltip.

### Accessibility
- Tooltip element uses `role="tooltip"`
- Trigger references tooltip via `aria-describedby` (not `aria-labelledby`)
- Tooltip is in the DOM at all times (not injected dynamically) so screen readers can associate it
- `pointer-events: none` on tooltip prevents it from blocking interaction
- Tooltip content should be concise (single short sentence). For richer content, use a popover or dialog instead
- Dismiss on Escape ensures keyboard users can close it
