## Design Principles

- **The calendar grid is navigable entirely by keyboard.** Arrow keys move day-to-day, Page Up/Down change months, Home/End jump to week boundaries -- every date is reachable without a mouse.
- **Today and selected are visually distinct states.** Today gets a border ring; selected gets an accent fill. A date can be both today and selected, so both indicators must coexist without overlap.
- **The popup opens below the input and closes on outside click or Escape.** Focus is trapped within the popup while open and returns to the input on close.
- **Full date labels on every cell ensure screen reader clarity.** "May 3, 2026, Selected" as an aria-label is unambiguous; "3" alone is not.

### CSS
```css
.datepicker {
  position: relative;
  display: inline-block;
}
.datepicker-popup {
  position: absolute;
  top: calc(100% + 4px);
  left: 0;
  background: var(--bg);
  border: 1px solid var(--border);
  border-radius: var(--radius-md);
  box-shadow: var(--shadow-lg);
  padding: var(--space-4);
  z-index: 200;
  min-width: 280px;
  opacity: 0;
  visibility: hidden;
  transform: translateY(-4px);
  transition: opacity var(--dur-base) var(--ease), transform var(--dur-base) var(--ease), visibility var(--dur-base) var(--ease);
}
.datepicker-popup--open {
  opacity: 1;
  visibility: visible;
  transform: translateY(0);
}
.datepicker-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: var(--space-3);
}
.datepicker-header__title {
  font-family: var(--font-display);
  font-size: var(--fs-body-sm);
  font-weight: var(--fw-semibold);
  color: var(--fg);
}
.datepicker-header__btn {
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
  transition: background var(--dur-fast) var(--ease), color var(--dur-fast) var(--ease);
}
.datepicker-header__btn:hover {
  background: var(--bg-alt);
  color: var(--fg);
}
.datepicker-grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 2px;
  text-align: center;
}
.datepicker-weekday {
  font-family: var(--font-display);
  font-size: var(--fs-label);
  font-weight: var(--fw-semibold);
  color: var(--fg-muted);
  padding: var(--space-1) 0;
  text-transform: uppercase;
}
.datepicker-day {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 36px;
  height: 36px;
  font-family: var(--font-body);
  font-size: var(--fs-body-sm);
  color: var(--fg);
  background: transparent;
  border: none;
  border-radius: var(--radius-sm);
  cursor: pointer;
  transition: background var(--dur-fast) var(--ease), color var(--dur-fast) var(--ease);
}
.datepicker-day:hover {
  background: var(--bg-alt);
}
.datepicker-day:focus-visible {
  outline: 2px solid var(--focus-ring);
  outline-offset: -2px;
}
.datepicker-day--today {
  font-weight: var(--fw-bold);
  border: 1px solid var(--accent);
}
.datepicker-day--selected {
  background: var(--accent);
  color: var(--on-accent);
  font-weight: var(--fw-semibold);
}
.datepicker-day--selected:hover {
  background: var(--accent-hover);
}
.datepicker-day--other-month {
  color: var(--fg-subtle);
  opacity: 0.5;
}
.datepicker-day--disabled {
  opacity: 0.3;
  pointer-events: none;
}
```

### HTML Pattern
```html
<div class="datepicker" data-datepicker>
  <div class="form-group">
    <label class="label" for="date-input">Appointment date</label>
    <div class="input-wrapper" style="position:relative;">
      <input class="input" type="text" id="date-input"
             placeholder="MM/DD/YYYY"
             autocomplete="off"
             aria-haspopup="dialog"
             aria-expanded="false"
             readonly />
    </div>
  </div>
  <div class="datepicker-popup" role="dialog" aria-label="Choose date" id="dp-popup">
    <div class="datepicker-header">
      <button class="datepicker-header__btn" type="button" aria-label="Previous month">
        <svg width="16" height="16" viewBox="0 0 16 16" aria-hidden="true">
          <path d="M10 4l-4 4 4 4" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
        </svg>
      </button>
      <span class="datepicker-header__title">May 2026</span>
      <button class="datepicker-header__btn" type="button" aria-label="Next month">
        <svg width="16" height="16" viewBox="0 0 16 16" aria-hidden="true">
          <path d="M6 4l4 4-4 4" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
        </svg>
      </button>
    </div>
    <div class="datepicker-grid" role="grid" aria-label="May 2026">
      <span class="datepicker-weekday" role="columnheader">Su</span>
      <span class="datepicker-weekday" role="columnheader">Mo</span>
      <span class="datepicker-weekday" role="columnheader">Tu</span>
      <span class="datepicker-weekday" role="columnheader">We</span>
      <span class="datepicker-weekday" role="columnheader">Th</span>
      <span class="datepicker-weekday" role="columnheader">Fr</span>
      <span class="datepicker-weekday" role="columnheader">Sa</span>
      <!-- Day cells -->
      <button class="datepicker-day" type="button" role="gridcell" aria-label="May 1, 2026">1</button>
      <button class="datepicker-day datepicker-day--today" type="button" role="gridcell" aria-label="May 2, 2026, Today">2</button>
      <button class="datepicker-day datepicker-day--selected" type="button" role="gridcell" aria-label="May 3, 2026, Selected" aria-selected="true">3</button>
      <!-- ...remaining days -->
    </div>
  </div>
</div>
```

### JS Behavior
- **Triggers**: Click on the input opens the popup. Click outside closes. Selecting a date closes and writes the formatted date to the input.
- **Month navigation**: Prev/Next buttons change the displayed month and re-render the grid.
- **Keyboard**:
  - `ArrowRight` / `ArrowLeft`: Move focus to next/previous day.
  - `ArrowDown` / `ArrowUp`: Move focus down/up by one week.
  - `Home`: Move focus to start of the week (Sunday). `End`: Move focus to end of the week (Saturday).
  - `Page Up` / `Page Down`: Previous/next month.
  - `Enter` / `Space`: Select the focused day.
  - `Escape`: Close the popup without selecting.
- **Focus management**: On open, focus the selected day (or today if no selection). On close, return focus to the input. Focus moves between day buttons via arrow keys.
- **Date formatting**: Write selected date to input in a locale-aware format (e.g., `MM/DD/YYYY`). The raw value can be stored in a `data-value` attribute in ISO format (`YYYY-MM-DD`).

### Accessibility
- Popup uses `role="dialog"` with `aria-label="Choose date"`
- Calendar grid uses `role="grid"` with day cells as `role="gridcell"`
- Weekday headers use `role="columnheader"`
- Each day button has `aria-label` with the full date (e.g., "May 3, 2026")
- Today cell is labeled with "Today", selected cell with "Selected"
- Selected cell uses `aria-selected="true"`
- Input uses `aria-haspopup="dialog"` and `aria-expanded`
- Focus is trapped within the popup while open
