## Design Principles

- **Only one tab panel is visible at a time.** Inactive panels must be fully hidden (`display: none` + `hidden` attribute) to prevent screen reader ghost content.
- **The active tab uses both color and an underline indicator.** A bottom border in the accent color plus bolder text weight makes the selected tab unmistakable without relying on color alone.
- **Arrow keys navigate between tabs, not Tab.** Only the active tab is in the tab order (`tabindex="0"`); arrow keys move focus between siblings, following WAI-ARIA Tabs pattern.
- **Tab panels are independently focusable.** A `tabindex="0"` on the panel allows users to jump directly to panel content after navigating the tab list.

### CSS
```css
.tab-list {
  display: flex;
  gap: 0;
  border-bottom: 2px solid var(--border);
  list-style: none;
  padding: 0;
  margin: 0;
}
.tab {
  padding: var(--space-3) var(--space-5);
  font-family: var(--font-display);
  font-size: var(--fs-body-sm);
  font-weight: var(--fw-medium);
  color: var(--fg-muted);
  background: transparent;
  border: none;
  border-bottom: 2px solid transparent;
  margin-bottom: -2px;
  cursor: pointer;
  transition: color var(--dur-fast) var(--ease),
              border-color var(--dur-fast) var(--ease);
  white-space: nowrap;
}
.tab:hover {
  color: var(--fg);
}
.tab:focus-visible {
  outline: 2px solid var(--focus-ring);
  outline-offset: -2px;
  border-radius: var(--radius-sm) var(--radius-sm) 0 0;
}
.tab--active {
  color: var(--accent);
  border-bottom-color: var(--accent);
  font-weight: var(--fw-semibold);
}
.tab-panel {
  padding: var(--space-5) 0;
  animation: fadeIn 0.2s var(--ease);
}
.tab-panel--hidden {
  display: none;
}
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}
```

### HTML Pattern
```html
<div class="tabs" data-tabs>
  <ul class="tab-list" role="tablist" aria-label="Account settings">
    <li>
      <button class="tab tab--active"
              role="tab"
              id="tab-profile"
              aria-selected="true"
              aria-controls="panel-profile"
              tabindex="0">
        Profile
      </button>
    </li>
    <li>
      <button class="tab"
              role="tab"
              id="tab-security"
              aria-selected="false"
              aria-controls="panel-security"
              tabindex="-1">
        Security
      </button>
    </li>
    <li>
      <button class="tab"
              role="tab"
              id="tab-notifications"
              aria-selected="false"
              aria-controls="panel-notifications"
              tabindex="-1">
        Notifications
      </button>
    </li>
  </ul>
  <div class="tab-panel"
       role="tabpanel"
       id="panel-profile"
       aria-labelledby="tab-profile"
       tabindex="0">
    <p>Profile settings content here.</p>
  </div>
  <div class="tab-panel tab-panel--hidden"
       role="tabpanel"
       id="panel-security"
       aria-labelledby="tab-security"
       tabindex="0"
       hidden>
    <p>Security settings content here.</p>
  </div>
  <div class="tab-panel tab-panel--hidden"
       role="tabpanel"
       id="panel-notifications"
       aria-labelledby="tab-notifications"
       tabindex="0"
       hidden>
    <p>Notification preferences here.</p>
  </div>
</div>
```

### JS Behavior
- **Triggers**: Click on a tab activates it and shows the associated panel.
- **Keyboard**:
  - `ArrowRight` / `ArrowLeft`: Move focus between tabs (horizontal orientation). For vertical tabs, use `ArrowDown` / `ArrowUp`.
  - `Home`: Focus first tab.
  - `End`: Focus last tab.
- **Focus management**: Only the active tab is in the tab order (`tabindex="0"`). Inactive tabs have `tabindex="-1"`. Arrow keys move focus between tabs and optionally activate on focus (follow-focus pattern). The newly activated panel receives `tabindex="0"` for direct access.
- **State sync**: Set `aria-selected="true"` on active tab, `false` on others. Show the matching panel, hide all others (`hidden` attribute + CSS class).

### Accessibility
- Tab list uses `role="tablist"` with `aria-label` describing the purpose
- Each tab uses `role="tab"` with `aria-selected` and `aria-controls` (panel id)
- Each panel uses `role="tabpanel"` with `aria-labelledby` (tab id)
- Only active tab is in page tab order; arrow keys navigate between tabs
- Panels are focusable (`tabindex="0"`) for direct access
- Scrollable tab lists should have arrow-button scroll indicators when overflow occurs
