# Accessibility Patterns

Cross-cutting accessibility patterns used across multiple components. Individual component files
reference these patterns rather than duplicating them.

---

## 1. Focus Trap

For modals, drawers, command palettes, and any overlay that must capture focus while open.

```css
/* Applied to all siblings of the overlay via the `inert` attribute */
[inert] {
  pointer-events: none;
  user-select: none;
}
```

```js
// Minimal focus trap — call when overlay opens
function trapFocus(container) {
  const focusable = container.querySelectorAll(
    'a[href], button:not([disabled]), input:not([disabled]), select:not([disabled]), textarea:not([disabled]), [tabindex]:not([tabindex="-1"])'
  );
  const first = focusable[0];
  const last = focusable[focusable.length - 1];

  container.addEventListener('keydown', (e) => {
    if (e.key !== 'Tab') return;
    if (e.shiftKey) {
      if (document.activeElement === first) {
        e.preventDefault();
        last.focus();
      }
    } else {
      if (document.activeElement === last) {
        e.preventDefault();
        first.focus();
      }
    }
  });

  first.focus();
}
```

**Rules:**
- Focus must never escape the trap while the overlay is open.
- Tab and Shift+Tab cycle within the trap boundary.
- Escape closes the overlay and returns focus to the trigger element.
- If the trigger element is removed from the DOM, return focus to `document.body`.

---

## 2. Skip Link

Allows keyboard users to bypass repetitive navigation.

```css
.skip-link {
  position: absolute;
  top: -100%;
  left: var(--space-4);
  padding: var(--space-2) var(--space-4);
  background: var(--accent);
  color: var(--on-accent);
  font-family: var(--font-body);
  font-size: var(--fs-body-sm);
  font-weight: var(--fw-semibold);
  border-radius: var(--radius-md);
  z-index: 9999;
  text-decoration: none;
  transition: top var(--dur-fast) var(--ease);
}

.skip-link:focus {
  top: var(--space-4);
}
```

```html
<a href="#main-content" class="skip-link">Skip to main content</a>
<!-- ... navigation ... -->
<main id="main-content" tabindex="-1">
  <!-- content -->
</main>
```

**Rules:**
- Must be the first focusable element on the page.
- Target element should have `tabindex="-1"` to receive programmatic focus without being in the tab order.
- Use `#main-content` as the standard target ID across all generated pages.

---

## 3. Screen Reader Utilities

```css
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
```

**Usage patterns:**

```html
<!-- Icon button without visible text -->
<button aria-label="Close dialog">
  <svg aria-hidden="true"><!-- icon --></svg>
</button>

<!-- Decorative image -->
<img src="pattern.svg" alt="" role="presentation"/>

<!-- Status announced to screen readers -->
<div class="sr-only" aria-live="polite" role="status">
  3 results found
</div>

<!-- Visible label paired with additional context -->
<label for="email">Email</label>
<span class="sr-only" id="email-hint">Enter your work email address</span>
<input id="email" type="email" aria-describedby="email-hint"/>
```

---

## 4. Live Regions

For dynamic content that changes without a page reload (toast notifications, search results, loading states).

### Polite (announces after current speech)

```html
<div aria-live="polite" aria-atomic="true" role="status">
  Form saved successfully.
</div>
```

Use for: form save confirmations, search result counts, progress updates.

### Assertive (interrupts current speech)

```html
<div aria-live="assertive" role="alert">
  Connection lost. Changes may not be saved.
</div>
```

Use for: error messages, validation failures, critical warnings.

### Pattern for search results

```html
<div class="sr-only" aria-live="polite" role="status" id="search-status"></div>
<!-- Update #search-status text content via JS after results load:
     "5 results found for 'dashboard'" -->
```

---

## 5. Reduced Motion

Respect user preferences for reduced motion across all animations.

```css
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

**Component-specific overrides** (use when the global rule is too aggressive):

```css
@media (prefers-reduced-motion: reduce) {
  .modal {
    animation: none;
  }
  .toast {
    transition: none;
  }
  .progress-bar--indeterminate .progress-bar__fill {
    animation: none;
    width: 100%;
    opacity: 0.5;
  }
}
```

**Rules:**
- Every CSS animation and transition must be reducible.
- Never remove functional state changes (opacity for visibility, display for show/hide) — only remove decorative motion.
- If an animation conveys meaning (e.g., a progress bar filling), replace it with a static equivalent (e.g., instant fill to current value).

---

## 6. Keyboard Navigation Patterns

### Arrow key navigation (for lists, menus, tabs, toolbars)

```js
function setupArrowNavigation(container, itemSelector, orientation = 'vertical') {
  container.addEventListener('keydown', (e) => {
    const items = Array.from(container.querySelectorAll(itemSelector));
    const current = items.indexOf(document.activeElement);
    let next;

    if (orientation === 'vertical') {
      if (e.key === 'ArrowDown') next = (current + 1) % items.length;
      if (e.key === 'ArrowUp') next = (current - 1 + items.length) % items.length;
    } else {
      if (e.key === 'ArrowRight') next = (current + 1) % items.length;
      if (e.key === 'ArrowLeft') next = (current - 1 + items.length) % items.length;
    }

    if (next !== undefined) {
      e.preventDefault();
      items[next].focus();
    }
  });
}
```

### Standard key bindings

| Key | Action | Context |
|---|---|---|
| Tab | Move to next focusable element | Global |
| Shift+Tab | Move to previous focusable element | Global |
| Enter / Space | Activate button, link, or control | Interactive elements |
| Escape | Close overlay, cancel action, return focus | Modals, drawers, dropdowns, command palette |
| Arrow Up/Down | Navigate list items vertically | Menus, listboxes, command palette |
| Arrow Left/Right | Navigate items horizontally, or adjust value | Tabs, sliders, horizontal menus |
| Home / End | Move to first / last item | Lists, menus, tables |
| Page Up / Down | Scroll by page height | Scrollable containers |

### Focus-visible ring

All interactive elements must show a visible focus indicator when focused via keyboard.

```css
:focus-visible {
  outline: 2px solid var(--focus-ring);
  outline-offset: 2px;
}

/* Never use :focus alone for visible rings — it shows on mouse click too */
/* Use :focus-visible which only activates for keyboard navigation */
```

---

## 7. Color Contrast Requirements

### Minimum ratios (WCAG AA)

| Element type | Minimum ratio | Standard |
|---|---|---|
| Normal text (< 24px / 18.66px bold) | 4.5:1 | AA |
| Large text (>= 24px / 18.66px bold) | 3:1 | AA |
| UI components and graphical objects | 3:1 | AA |
| Focus indicators | 3:1 | AA |

### Checking method

```
L = 0.2126 * R_lin + 0.7152 * G_lin + 0.0722 * B_lin
For each channel: if sRGB <= 0.04045, linear = sRGB / 12.92
                  else linear = ((sRGB + 0.055) / 1.055) ^ 2.4
Contrast ratio = (L_lighter + 0.05) / (L_darker + 0.05)
```

### Token-level pairs that must pass

| Pair | Minimum |
|---|---|
| `--fg` on `--bg` | 4.5:1 |
| `--fg-muted` on `--bg` | 3:1 |
| `--accent` on `--bg` | 3:1 (UI) or 4.5:1 (text) |
| `--error` on `--bg` | 3:1 |
| `--success` on `--bg` | 3:1 |
| `--fg` on `--accent` (via `--on-accent`) | 4.5:1 |

Both light and dark mode must be validated independently.
