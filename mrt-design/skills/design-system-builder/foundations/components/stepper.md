## Design Principles

- **Steppers make multi-step processes scannable.** The user always knows where they are, where they've been, and what's left.
- **Each step has a clear label.** Numbers alone are not enough. "Payment" is better than "3".
- **Completed steps are visually distinct from current and future steps.** Use color and iconography — not just opacity — to communicate state.
- **Linear steppers are default.** Non-linear (skippable) steps must be explicitly marked with a "Skip" action, not implied by silence.
- **Validation gates each step.** The "Next" button is disabled until the current step's required fields are valid.

## Brand Expression

Steppers are structural components. Apply `creative_brief` through connector style, step marker shape, and active state:

- `safe`: numbered circles, neutral connectors, accent active marker.
- `elevated`: custom step icons replacing numbers, animated connector fill, or branded active state.
- `bold`: labeled progress bar instead of discrete steps, or vertical stepper with card-style step containers.
- `experimental`: interactive timeline, branching paths, or animated step transitions.

### Component Tokens

```css
.stepper {
  --step-size: 32px;
  --step-font: var(--font-body);
  --step-font-size: var(--fs-label);
  --step-font-weight: var(--fw-medium);
  --step-color: var(--fg-muted);
  --step-active-color: var(--accent);
  --step-complete-color: var(--accent);
  --step-complete-bg: var(--accent-tint);
  --step-connector-color: var(--border);
  --step-connector-active: var(--accent);
  --step-label-color: var(--fg-muted);
  --step-label-active: var(--fg);
  --step-connector-height: 2px;
}
```

### CSS

```css
.stepper {
  display: flex;
  align-items: flex-start;
  gap: 0;
  font-family: var(--step-font);
}

.stepper__step {
  display: flex;
  flex-direction: column;
  align-items: center;
  flex: 1;
  position: relative;
}

/* Connector line between steps */
.stepper__step + .stepper__step::before {
  content: "";
  position: absolute;
  top: calc(var(--step-size) / 2);
  inset-inline-start: calc(-50% + var(--step-size) / 2);
  width: calc(100% - var(--step-size));
  height: var(--step-connector-height);
  background: var(--step-connector-color);
  transform: translateY(-50%);
}

.stepper__step--completed + .stepper__step::before,
.stepper__step--active + .stepper__step::before {
  background: var(--step-connector-active);
}

/* Step marker */
.stepper__marker {
  width: var(--step-size);
  height: var(--step-size);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: var(--step-font-size);
  font-weight: var(--step-font-weight);
  color: var(--step-color);
  border: 2px solid var(--step-connector-color);
  background: var(--bg);
  position: relative;
  z-index: 1;
  transition: all var(--dur-fast) var(--ease);
}

.stepper__step--active .stepper__marker {
  color: var(--step-active-color);
  border-color: var(--step-active-color);
  box-shadow: 0 0 0 4px var(--accent-tint);
}

.stepper__step--completed .stepper__marker {
  color: var(--step-complete-color);
  background: var(--step-complete-bg);
  border-color: var(--step-complete-color);
}

/* Step label */
.stepper__label {
  margin-block-start: var(--space-2);
  font-size: var(--fs-label);
  color: var(--step-label-color);
  text-align: center;
  max-width: 120px;
}

.stepper__step--active .stepper__label {
  color: var(--step-label-active);
  font-weight: var(--fw-semibold);
}

.stepper__step--completed .stepper__label {
  color: var(--fg);
}
```

### HTML Pattern

```html
<nav class="stepper" aria-label="Progress">
  <div class="stepper__step stepper__step--completed">
    <div class="stepper__marker" aria-hidden="true">&#10003;</div>
    <span class="stepper__label">Account</span>
  </div>
  <div class="stepper__step stepper__step--active">
    <div class="stepper__marker" aria-current="step">2</div>
    <span class="stepper__label">Profile</span>
  </div>
  <div class="stepper__step">
    <div class="stepper__marker">3</div>
    <span class="stepper__label">Review</span>
  </div>
  <div class="stepper__step">
    <div class="stepper__marker">4</div>
    <span class="stepper__label">Confirm</span>
  </div>
</nav>
```

### Vertical Variant

```css
.stepper--vertical {
  flex-direction: column;
  gap: 0;
}

.stepper--vertical .stepper__step {
  flex-direction: row;
  align-items: flex-start;
  gap: var(--space-4);
  padding-block-end: var(--space-6);
}

.stepper--vertical .stepper__step + .stepper__step::before {
  inset-inline-start: calc(var(--step-size) / 2 - 1px);
  top: var(--step-size);
  width: var(--step-connector-height);
  height: calc(100% - var(--step-size));
  transform: none;
}

.stepper--vertical .stepper__label {
  margin-block-start: calc((var(--step-size) - 1lh) / 2);
  text-align: start;
  max-width: none;
}
```

### Dark Mode

```css
[data-theme="dark"] .stepper__marker {
  background: var(--bg-alt);
}
```

### Accessibility

- Use `<nav>` with `aria-label="Progress"` or `aria-label="Checkout steps"`.
- Active step marker has `aria-current="step"`.
- Completed steps are indicated visually; the checkmark replaces the number.
- Step labels provide text alternatives for screen readers.
- Announce step changes with `aria-live="polite"` on a status region.

### Responsive Behavior

- Below 640px: hide labels, show only markers with numbers. Use tooltips or a summary line below the stepper showing "Step 2 of 4: Profile".
- On very narrow screens (< 380px): switch to a simple "Step 2/4" text indicator instead of the full stepper.
