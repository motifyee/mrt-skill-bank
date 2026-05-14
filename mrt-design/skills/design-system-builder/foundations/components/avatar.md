## Design Principles

- **Avatars represent people, teams, or entities visually.** Use photos when available; fall back to initials; never show a broken image.
- **Size is consistent within a context.** Avatars in a list, table, or avatar group must use the same size variant. Mixing sizes is only acceptable for hierarchy emphasis (e.g., current user larger).
- **Status indicators supplement, not replace, text status.** A green dot means "available" visually, but a tooltip or adjacent label must confirm it for screen readers.
- **Avatar groups communicate scale without overwhelming.** Stacked avatars with a "+N" overflow badge show team membership at a glance.

## Brand Expression

Avatars are identity markers. Apply `character_rules` through shape, border,
and status indicator style:

- `safe`: circular shape, solid border on status indicator, neutral fallback background.
- `elevated`: branded status indicator animation (gentle pulse), signature border on the avatar, or themed fallback gradient.
- `bold`: square or rounded-rectangle shape, oversized status indicator, or branded initials background using accent color.
- `experimental`: animated border, holographic fallback, or dynamic status effects only for non-critical social features.

### Component Tokens

```css
.avatar {
  --avatar-size: 40px;
  --avatar-radius: var(--radius-full);
  --avatar-bg: var(--bg-alt);
  --avatar-color: var(--fg-muted);
  --avatar-font: var(--font-display);
  --avatar-font-size: var(--fs-body-sm);
  --avatar-font-weight: var(--fw-semibold);
  --avatar-border: 2px solid var(--bg);
  --avatar-status-size: 12px;
  --avatar-status-border: 2px solid var(--bg);
}
```

### CSS

```css
.avatar {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: var(--avatar-size);
  height: var(--avatar-size);
  border-radius: var(--avatar-radius);
  background: var(--avatar-bg);
  color: var(--avatar-color);
  font-family: var(--avatar-font);
  font-size: var(--avatar-font-size);
  font-weight: var(--avatar-font-weight);
  overflow: hidden;
  position: relative;
  flex-shrink: 0;
  user-select: none;
}

.avatar img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

/* Initials fallback */
.avatar__initials {
  text-transform: uppercase;
  letter-spacing: 0.02em;
  line-height: 1;
}
```

### Size Variants

```css
.avatar--xs {
  --avatar-size: 24px;
  --avatar-font-size: 0.625rem;
  --avatar-status-size: 8px;
  --avatar-status-border: 1.5px solid var(--bg);
}

.avatar--sm {
  --avatar-size: 32px;
  --avatar-font-size: var(--fs-label);
  --avatar-status-size: 10px;
}

.avatar--md {
  /* Uses base tokens (40px) */
}

.avatar--lg {
  --avatar-size: 48px;
  --avatar-font-size: var(--fs-body);
  --avatar-status-size: 14px;
}

.avatar--xl {
  --avatar-size: 64px;
  --avatar-font-size: var(--fs-body-lg, 1.125rem);
  --avatar-status-size: 16px;
}
```

### Status Indicator

```css
.avatar__status {
  position: absolute;
  inset-block-end: 0;
  inset-inline-end: 0;
  width: var(--avatar-status-size);
  height: var(--avatar-status-size);
  border-radius: var(--radius-full);
  border: var(--avatar-status-border);
  z-index: 1;
}

.avatar__status--online {
  background: var(--success);
}

.avatar__status--offline {
  background: var(--fg-muted);
}

.avatar__status--busy {
  background: var(--error);
}

.avatar__status--away {
  background: var(--warning, #f59e0b);
}

/* Animated pulse for online status (respects reduced motion) */
.avatar__status--online::after {
  content: '';
  position: absolute;
  inset: -2px;
  border-radius: var(--radius-full);
  background: var(--success);
  opacity: 0;
  animation: statusPulse 2s var(--ease) infinite;
}
@media (prefers-reduced-motion: reduce) {
  .avatar__status--online::after {
    animation: none;
  }
}
@keyframes statusPulse {
  0%   { opacity: 0.4; transform: scale(1); }
  100% { opacity: 0;   transform: scale(1.5); }
}
```

### HTML Pattern

```html
<!-- Avatar with image -->
<div class="avatar avatar--md" role="img" aria-label="Jane Doe">
  <img src="jane-doe.jpg" alt="" />
  <span class="avatar__status avatar__status--online" aria-label="Online"></span>
</div>

<!-- Avatar with initials fallback -->
<div class="avatar avatar--md" role="img" aria-label="John Smith">
  <span class="avatar__initials">JS</span>
</div>

<!-- Avatar with image error handling (use initials as fallback) -->
<div class="avatar avatar--md" role="img" aria-label="Jane Doe">
  <img src="broken.jpg" alt="" onerror="this.style.display='none'; this.nextElementSibling.style.display='flex';" />
  <span class="avatar__initials" style="display:none">JD</span>
</div>
```

### Avatar Group (Stacked)

```html
<div class="avatar-group" role="group" aria-label="Team members: Jane Doe, John Smith, Alice Lee, and 5 more">
  <div class="avatar avatar--sm avatar-group__item" role="img" aria-label="Jane Doe">
    <img src="jane.jpg" alt="" />
  </div>
  <div class="avatar avatar--sm avatar-group__item" role="img" aria-label="John Smith">
    <span class="avatar__initials">JS</span>
  </div>
  <div class="avatar avatar--sm avatar-group__item" role="img" aria-label="Alice Lee">
    <img src="alice.jpg" alt="" />
  </div>
  <span class="avatar avatar--sm avatar-group__overflow" aria-label="5 more members">
    +5
  </span>
</div>
```

```css
.avatar-group {
  display: inline-flex;
  align-items: center;
  flex-direction: row-reverse;
  padding-inline-start: var(--space-1);
}

.avatar-group__item {
  border: var(--avatar-border);
  margin-inline-start: calc(-1 * var(--space-2));
}
.avatar-group__item:first-child {
  margin-inline-start: 0;
}

/* Ensure proper z-stacking: first in DOM is on top */
.avatar-group__item:nth-child(1) { z-index: 5; }
.avatar-group__item:nth-child(2) { z-index: 4; }
.avatar-group__item:nth-child(3) { z-index: 3; }
.avatar-group__item:nth-child(4) { z-index: 2; }

/* Overflow badge */
.avatar-group__overflow {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: var(--avatar-size);
  height: var(--avatar-size);
  padding-inline: var(--space-2);
  border-radius: var(--avatar-radius);
  background: var(--bg-alt);
  color: var(--fg-muted);
  font-family: var(--font-display);
  font-size: var(--fs-label);
  font-weight: var(--fw-semibold);
  border: var(--avatar-border);
  margin-inline-start: calc(-1 * var(--space-2));
  z-index: 1;
}
```

### Image Error Handling

When an avatar image fails to load, show the initials fallback:

```js
// Attach to each avatar img element
img.addEventListener('error', () => {
  img.style.display = 'none';
  const initials = img.parentElement.querySelector('.avatar__initials');
  if (initials) initials.style.display = 'flex';
});
```

- Never show a broken image icon.
- Always render the initials `<span>` in the DOM with `display: none` as a fallback.
- Generate initials from the first and last word of the display name (e.g., "Jane Doe" -> "JD").

### Dark Mode

```css
@media (prefers-color-scheme: dark) {
  .avatar {
    --avatar-bg: var(--bg-elevated);
    --avatar-color: var(--fg-muted);
    --avatar-border: 2px solid var(--bg-elevated);
    --avatar-status-border: 2px solid var(--bg-elevated);
  }
  .avatar-group__overflow {
    background: var(--bg-elevated);
    border-color: var(--bg-elevated);
  }
}
```

### Responsive Behavior

- Avatars are fixed-size and do not scale with viewport.
- On small screens, prefer `avatar--sm` or `avatar--xs` for avatar groups to prevent horizontal overflow.
- Avatar groups may truncate to fewer visible items on narrow containers (e.g., show 3 + overflow on mobile vs. 5 + overflow on desktop).
- In mobile list views, avatars use consistent `avatar--sm` sizing to match touch-target row heights.

### Accessibility

- Use `role="img"` on each avatar with `aria-label` containing the person's name.
- Status indicators need `aria-label` (e.g., `aria-label="Online"`). They are supplementary to the avatar's accessible name.
- Avatar groups use `role="group"` with `aria-label` listing visible names and the overflow count (e.g., "Team members: Jane Doe, John Smith, Alice Lee, and 5 more").
- The overflow badge should have `aria-label` describing the count (e.g., "5 more members").
- Initials fallback is accessible by default via the visible text content.
- Decorative avatar usage (no person/entity association) should use `aria-hidden="true"`.
- `alt=""` on `<img>` inside avatars because the accessible name comes from the parent's `aria-label`, not the image alt.

### States

- **Default**: visible image or initials.
- **Hover**: optional tooltip showing full name on avatar or avatar group item.
- **Focus**: if the avatar is interactive (link to profile), show focus ring.
- **Loading**: show skeleton circle matching the avatar size variant.
- **Error (image)**: display initials fallback, never show broken image.
