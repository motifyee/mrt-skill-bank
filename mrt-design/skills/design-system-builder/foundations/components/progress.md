## Design Principles

- **Progress indicators show status, not speed.** They communicate that the system is working and, when possible, how much remains. Never fake determinacy.
- **Determinate vs indeterminate is a design decision.** Use determinate (percentage-based) when progress can be calculated. Use indeterminate (looping animation) when it cannot.
- **Linear progress is for sequential tasks** (file upload, form completion). Circular progress is for contained spaces (buttons, data loading, inline loading).
- **Progress bars are non-interactive.** They display state; they do not accept input. Sliders and range inputs are separate components.

## Brand Expression

Progress indicators are small but visible. Apply `creative_brief` through bar shape, color, and animation:

- `safe`: neutral track, accent fill, no animation beyond the fill.
- `elevated`: accent gradient fill, subtle pulse animation on active bar, rounded caps.
- `bold`: striped or animated fill, custom track color, label inside the bar.
- `experimental`: SVG-based circular progress with animated dash offset, multi-segment progress.

### Component Tokens

```css
.progress {
  --progress-height: 8px;
  --progress-radius: var(--radius-full);
  --progress-track: var(--bg-alt);
  --progress-fill: var(--accent);
  --progress-fill-indeterminate: var(--accent);
  --progress-label-font: var(--font-body);
  --progress-label-size: var(--fs-label);
  --progress-label-color: var(--fg-muted);
  --progress-circle-size: 40px;
  --progress-circle-stroke: 4px;
}
```

### CSS — Linear

```css
.progress-bar {
  position: relative;
}

.progress-bar__label {
  display: flex;
  justify-content: space-between;
  margin-block-end: var(--space-2);
  font-family: var(--progress-label-font);
  font-size: var(--progress-label-size);
  color: var(--progress-label-color);
}

.progress-bar__track {
  width: 100%;
  height: var(--progress-height);
  background: var(--progress-track);
  border-radius: var(--progress-radius);
  overflow: hidden;
}

.progress-bar__fill {
  height: 100%;
  background: var(--progress-fill);
  border-radius: var(--progress-radius);
  transition: width var(--dur-slow) var(--ease);
}

/* Indeterminate */
.progress-bar--indeterminate .progress-bar__fill {
  width: 40%;
  animation: progressIndeterminate 1.5s var(--ease) infinite;
}

@keyframes progressIndeterminate {
  0%   { transform: translateX(-100%); }
  100% { transform: translateX(300%); }
}
```

### CSS — Circular

```css
.progress-circle {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  position: relative;
  width: var(--progress-circle-size);
  height: var(--progress-circle-size);
}

.progress-circle svg {
  transform: rotate(-90deg);
  width: 100%;
  height: 100%;
}

.progress-circle__track {
  fill: none;
  stroke: var(--progress-track);
  stroke-width: var(--progress-circle-stroke);
}

.progress-circle__fill {
  fill: none;
  stroke: var(--progress-fill);
  stroke-width: var(--progress-circle-stroke);
  stroke-linecap: round;
  transition: stroke-dashoffset var(--dur-slow) var(--ease);
}

.progress-circle__label {
  position: absolute;
  font-family: var(--progress-label-font);
  font-size: var(--fs-label);
  font-weight: var(--fw-semibold);
  color: var(--fg);
}

/* Indeterminate circular */
.progress-circle--indeterminate .progress-circle__fill {
  animation: circleSpin 1.2s linear infinite;
  stroke-dasharray: 60 200;
}

@keyframes circleSpin {
  to { transform: rotate(360deg); }
}
```

### HTML Patterns

```html
<!-- Determinate linear -->
<div class="progress-bar" role="progressbar" aria-valuenow="65" aria-valuemin="0" aria-valuemax="100" aria-label="Upload progress">
  <div class="progress-bar__label">
    <span>Uploading...</span>
    <span>65%</span>
  </div>
  <div class="progress-bar__track">
    <div class="progress-bar__fill" style="width: 65%"></div>
  </div>
</div>

<!-- Indeterminate linear -->
<div class="progress-bar progress-bar--indeterminate" role="progressbar" aria-label="Processing">
  <div class="progress-bar__label">
    <span>Processing...</span>
  </div>
  <div class="progress-bar__track">
    <div class="progress-bar__fill"></div>
  </div>
</div>

<!-- Determinate circular -->
<div class="progress-circle" role="progressbar" aria-valuenow="75" aria-valuemin="0" aria-valuemax="100" aria-label="Loading">
  <svg viewBox="0 0 40 40">
    <circle class="progress-circle__track" cx="20" cy="20" r="16"/>
    <circle class="progress-circle__fill" cx="20" cy="20" r="16"
      stroke-dasharray="100.53"
      stroke-dashoffset="25.13"/>
  </svg>
  <span class="progress-circle__label">75%</span>
</div>
```

### Size Variants

```css
.progress-bar--thin {
  --progress-height: 4px;
}

.progress-bar--thick {
  --progress-height: 12px;
}

.progress-circle--sm {
  --progress-circle-size: 24px;
  --progress-circle-stroke: 3px;
}

.progress-circle--lg {
  --progress-circle-size: 56px;
  --progress-circle-stroke: 5px;
}
```

### Dark Mode

```css
[data-theme="dark"] .progress-bar__track {
  background: rgba(255, 255, 255, 0.08);
}
```

### Accessibility

- `role="progressbar"` on the container.
- `aria-valuenow` for determinate progress (current value).
- `aria-valuemin="0"` and `aria-valuemax="100"` for determinate progress.
- `aria-label` describing what is loading (e.g., "Upload progress", "Saving changes").
- For indeterminate progress, omit `aria-valuenow` — screen readers will announce "loading" based on the role.
- Respect `prefers-reduced-motion`: disable all looping animations.

```css
@media (prefers-reduced-motion: reduce) {
  .progress-bar--indeterminate .progress-bar__fill {
    animation: none;
    width: 100%;
    opacity: 0.5;
  }
  .progress-circle--indeterminate .progress-circle__fill {
    animation: none;
  }
}
```

### Responsive Behavior

- Linear progress bars stretch to fill their container width. No responsive changes needed.
- Circular progress maintains its fixed size across breakpoints.
- Labels truncate with ellipsis on narrow containers.
