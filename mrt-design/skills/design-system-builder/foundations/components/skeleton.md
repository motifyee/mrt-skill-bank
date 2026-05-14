## Design Principles

- **Skeletons set expectations for content layout before it arrives.** They mirror the shape and spacing of real content so the user's eye knows where to look.
- **Animation is subtle and continuous, not distracting.** A gentle shimmer or soft pulse communicates "loading" without competing for attention.
- **Skeletons replace content, not augment it.** Show skeletons where content will appear; never show skeletons alongside real content in the same container.
- **Skeletons are removed the instant real content is available.** A flash of skeleton followed by content is acceptable; persistent skeletons after load is a bug.
- **Choose skeleton over spinner for structured layouts; spinner for brief, indeterminate waits.** If the loading area has a known shape (card, table row, text block), use skeleton. If the wait is under 500ms or the shape is unknown, use a spinner.

## Brand Expression

Skeletons are utility-first. Apply `creative_brief` restraint to animation style:

- `safe`: neutral gray blocks with a subtle shimmer animation.
- `elevated`: shimmer uses the accent color at very low opacity, or the skeleton shapes match branded card/article proportions.
- `bold`: skeleton blocks use the system's border radius and spacing language to feel like real content shadows.
- `experimental`: pulse animation instead of shimmer is acceptable; do not use multi-color or dramatic wave effects.

### Component Tokens

```css
.skeleton {
  --skeleton-bg: var(--bg-alt);
  --skeleton-shimmer: rgba(255, 255, 255, 0.4);
  --skeleton-radius: var(--radius-sm);
  --skeleton-line-height: 12px;
  --skeleton-line-gap: var(--space-3);
  --skeleton-animation-duration: 1.5s;
}
```

### Base CSS

```css
.skeleton {
  background: var(--skeleton-bg);
  border-radius: var(--skeleton-radius);
  position: relative;
  overflow: hidden;
}

/* Shimmer animation */
.skeleton--shimmer::after {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(
    90deg,
    transparent 0%,
    var(--skeleton-shimmer) 50%,
    transparent 100%
  );
  animation: shimmer var(--skeleton-animation-duration) var(--ease) infinite;
}

@keyframes shimmer {
  0%   { transform: translateX(-100%); }
  100% { transform: translateX(100%); }
}

/* Pulse animation (alternative) */
.skeleton--pulse {
  animation: pulse var(--skeleton-animation-duration) var(--ease) infinite;
}

@keyframes pulse {
  0%, 100% { opacity: 1; }
  50%      { opacity: 0.4; }
}

/* Respect reduced motion */
@media (prefers-reduced-motion: reduce) {
  .skeleton--shimmer::after {
    animation: none;
  }
  .skeleton--pulse {
    animation: none;
    opacity: 0.6;
  }
}
```

### Text Skeleton (Lines)

```css
.skeleton--text {
  height: var(--skeleton-line-height);
  width: 100%;
}

.skeleton--text-short {
  width: 60%;
}

.skeleton--text-medium {
  width: 80%;
}

/* Heading skeleton */
.skeleton--heading {
  height: 20px;
  width: 45%;
}

/* Label / caption skeleton */
.skeleton--label {
  height: 10px;
  width: 30%;
}
```

```html
<div aria-busy="true" aria-label="Loading content">
  <!-- Heading -->
  <div class="skeleton skeleton--shimmer skeleton--heading"></div>

  <!-- Text lines -->
  <div class="skeleton skeleton--shimmer skeleton--text" style="margin-block-start: var(--space-3)"></div>
  <div class="skeleton skeleton--shimmer skeleton--text" style="margin-block-start: var(--skeleton-line-gap)"></div>
  <div class="skeleton skeleton--shimmer skeleton--text skeleton--text-short" style="margin-block-start: var(--skeleton-line-gap)"></div>
</div>
```

### Avatar Skeleton

```css
.skeleton--avatar {
  border-radius: var(--radius-full);
  width: var(--avatar-size, 40px);
  height: var(--avatar-size, 40px);
}

.skeleton--avatar-xs { --avatar-size: 24px; }
.skeleton--avatar-sm { --avatar-size: 32px; }
.skeleton--avatar-md { --avatar-size: 40px; }
.skeleton--avatar-lg { --avatar-size: 48px; }
.skeleton--avatar-xl { --avatar-size: 64px; }
```

```html
<div class="skeleton skeleton--shimmer skeleton--avatar skeleton--avatar-md"></div>
```

### Card Skeleton

```html
<div class="skeleton-card" aria-hidden="true">
  <div class="skeleton skeleton--shimmer skeleton-card__media"></div>
  <div class="skeleton-card__body">
    <div class="skeleton skeleton--shimmer skeleton--heading"></div>
    <div class="skeleton skeleton--shimmer skeleton--text" style="margin-block-start: var(--space-2)"></div>
    <div class="skeleton skeleton--shimmer skeleton--text skeleton--text-short" style="margin-block-start: var(--skeleton-line-gap)"></div>
  </div>
  <div class="skeleton-card__footer">
    <div class="skeleton skeleton--shimmer" style="width: 80px; height: 36px; border-radius: var(--radius-sm)"></div>
  </div>
</div>
```

```css
.skeleton-card {
  border: 1px solid var(--border);
  border-radius: var(--radius-md);
  overflow: hidden;
  padding: 0;
}

.skeleton-card__media {
  aspect-ratio: 16 / 9;
  width: 100%;
}

.skeleton-card__body {
  padding: var(--space-5);
  display: flex;
  flex-direction: column;
  gap: var(--skeleton-line-gap);
}

.skeleton-card__footer {
  padding: 0 var(--space-5) var(--space-5);
}
```

### Table Row Skeleton

```html
<tbody aria-busy="true" aria-hidden="true">
  <tr class="skeleton-table-row">
    <td><div class="skeleton skeleton--shimmer skeleton--text" style="width:70%"></div></td>
    <td><div class="skeleton skeleton--shimmer skeleton--text" style="width:50%"></div></td>
    <td><div class="skeleton skeleton--shimmer skeleton--text" style="width:30%"></div></td>
    <td><div class="skeleton skeleton--shimmer skeleton--text" style="width:40%"></div></td>
  </tr>
  <tr class="skeleton-table-row">
    <td><div class="skeleton skeleton--shimmer skeleton--text" style="width:65%"></div></td>
    <td><div class="skeleton skeleton--shimmer skeleton--text" style="width:55%"></div></td>
    <td><div class="skeleton skeleton--shimmer skeleton--text" style="width:35%"></div></td>
    <td><div class="skeleton skeleton--shimmer skeleton--text" style="width:45%"></div></td>
  </tr>
  <!-- Repeat for ~5 rows -->
</tbody>
```

```css
.skeleton-table-row td {
  padding: var(--space-3) var(--space-4);
  border-block-end: 1px solid var(--border);
}
```

### Image / Media Skeleton

```css
.skeleton--image {
  width: 100%;
  aspect-ratio: 16 / 9;
}

.skeleton--square {
  aspect-ratio: 1;
}

.skeleton--thumbnail {
  width: 80px;
  height: 80px;
}
```

### Button Skeleton

```css
.skeleton--button {
  height: 36px;
  width: 100px;
  border-radius: var(--radius-sm);
}
```

### When to Use Skeleton vs Spinner

| Scenario | Use | Reason |
|---|---|---|
| Card grid loading | Skeleton | Content shape is known |
| Table data loading | Skeleton rows | Column structure is known |
| Article / blog content | Skeleton text lines | Layout shape is predictable |
| Avatar list loading | Skeleton circles | Size and shape are known |
| Form submission wait | Spinner | Duration is short, shape is unknown |
| File upload progress | Progress bar | Progress is measurable |
| Quick data refresh (< 500ms) | Spinner | Skeleton flash is jarring |
| Full page initial load | Spinner + splash | Layout is not yet determined |

### Dark Mode

```css
@media (prefers-color-scheme: dark) {
  .skeleton {
    --skeleton-bg: rgba(255, 255, 255, 0.08);
    --skeleton-shimmer: rgba(255, 255, 255, 0.06);
  }
}
```

### Responsive Behavior

- Skeleton shapes should match the responsive layout of the real content they replace.
- In a responsive card grid, skeleton cards use the same `minmax()` column widths.
- Text skeleton line widths may use percentage-based widths that adapt to container width.
- On mobile, skeleton table rows may switch to skeleton card shapes if the real table uses the responsive card pattern.

### Accessibility

- Skeleton containers must use `aria-busy="true"` to signal loading state to screen readers.
- Individual skeleton blocks are decorative: use `aria-hidden="true"`.
- Provide a text alternative via `aria-label` on the container (e.g., `aria-label="Loading user profile"`).
- When content arrives, remove `aria-busy`, remove `aria-hidden`, and replace skeletons with real content.
- If content fails to load, replace skeletons with an error state and `role="alert"`.
- Never use skeletons for content that will never appear (e.g., empty states).
- `prefers-reduced-motion`: disable shimmer/pulse animations; use static gray blocks instead.
