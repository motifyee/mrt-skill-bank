# Motion Choreography

> **Token values are canonical in `schemas/token_schema.md`.** This file provides usage guidance. If values differ, `token_schema.md` takes precedence.

Page-level motion narrative -- how animations work together across a full page,
not just individual component animations. Coordinated motion communicates hierarchy,
guides attention, and creates a sense of polish that isolated animations cannot.

## Design Principles

1. **Animate only `opacity` and `transform`:** Restrict all scroll-triggered and entrance animations to GPU-composited properties; never animate layout-triggering properties like width, height, or margin.
2. **Cap routine interactions at 300ms:** Button presses, toggles, and modal opens must complete in under 300ms; reserve durations above 450ms for deliberate page transitions only.
3. **Prioritize entrance by content tier:** Reveal critical content (hero, primary CTA) first, important supporting content second, and decorative elements last using staggered delays.
4. **Respect `prefers-reduced-motion`:** Disable all non-essential animation and show content immediately when the user's OS preference requests reduced motion.

---

## Entrance Sequences

### Cascade

Elements appear top-to-bottom with staggered delays. The simplest and most
reliable entrance pattern.

```css
.cascade > * {
  opacity: 0;
  transform: translateY(16px);
  animation: fadeInUp 400ms var(--ease) forwards;
  animation-delay: calc(var(--stagger-index, 0) * 100ms);
}

@keyframes fadeInUp {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
```

Apply `--stagger-index` via HTML (`style="--stagger-index: 0"`) or JavaScript.
For a 5-element cascade, the last element enters at 400ms -- still within
perceived-immediate range.

```html
<ul class="cascade">
  <li style="--stagger-index: 0">First</li>
  <li style="--stagger-index: 1">Second</li>
  <li style="--stagger-index: 2">Third</li>
  <li style="--stagger-index: 3">Fourth</li>
  <li style="--stagger-index: 4">Fifth</li>
</ul>
```

### Fade-in Group

Sections fade in as they cross the viewport threshold. Uses IntersectionObserver
to add a `.revealed` class.

```css
.reveal {
  opacity: 0;
  transform: translateY(16px);
  transition: opacity 500ms var(--ease), transform 500ms var(--ease);
}
.reveal.revealed {
  opacity: 1;
  transform: translateY(0);
}
```

```js
const observer = new IntersectionObserver(
  (entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        entry.target.classList.add('revealed');
        observer.unobserve(entry.target);
      }
    });
  },
  { threshold: 0.15, rootMargin: '0px 0px -40px 0px' }
);

document.querySelectorAll('.reveal').forEach((el) => observer.observe(el));
```

### Priority Reveal

Hero appears first, then supporting content, then decorative elements.
Three tiers controlled by a custom property.

```css
[data-reveal] {
  opacity: 0;
  animation: fadeIn 400ms var(--ease) forwards;
  animation-delay: var(--reveal-delay, 0ms);
}
[data-reveal="critical"]  { --reveal-delay: 0ms; }
[data-reveal="important"] { --reveal-delay: 200ms; }
[data-reveal="decorative"] { --reveal-delay: 400ms; }

@keyframes fadeIn {
  to { opacity: 1; }
}
```

### Skeleton-to-Content

Content replaces skeleton with a crossfade swap, avoiding layout shift.

```css
.skeleton {
  background: var(--bg-alt);
  border-radius: var(--radius-sm);
  position: relative;
  overflow: hidden;
}
.skeleton::after {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(
    90deg,
    transparent 0%,
    var(--bg-hover) 50%,
    transparent 100%
  );
  background-size: 200% 100%;
  animation: shimmer 1.5s var(--ease) infinite;
}

/* Crossfade on data arrival */
.content-area {
  transition: opacity 300ms var(--ease);
}
.content-area[data-state="loading"] { opacity: 0; }
.content-area[data-state="ready"]   { opacity: 1; }
```

---

## Scroll-Triggered Animation System

A reusable system built on IntersectionObserver with CSS class toggling.

```css
/* Common scroll animation classes */
.scroll-fadeInUp {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 500ms var(--ease), transform 500ms var(--ease);
}
.scroll-slideInLeft {
  opacity: 0;
  transform: translateX(-24px);
  transition: opacity 500ms var(--ease), transform 500ms var(--ease);
}
.scroll-scaleIn {
  opacity: 0;
  transform: scale(0.95);
  transition: opacity 400ms var(--ease), transform 400ms var(--ease);
}
.scroll-in-view {
  opacity: 1;
  transform: translate(0) scale(1);
}
```

```js
const scrollObserver = new IntersectionObserver(
  (entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        entry.target.classList.add('scroll-in-view');
        scrollObserver.unobserve(entry.target);
      }
    });
  },
  { threshold: 0.1, rootMargin: '0px 0px -48px 0px' }
);

document.querySelectorAll('[class*="scroll-"]').forEach((el) => {
  scrollObserver.observe(el);
});
```

**Performance rules:** Only animate `opacity` and `transform`. These are
compositor-only properties that skip layout and paint. Never animate `width`,
`height`, `top`, `left`, `margin`, or `padding` in scroll-triggered animations.

**Reduced motion:** Disable all scroll animations and show content immediately.

```css
@media (prefers-reduced-motion: reduce) {
  [class*="scroll-"],
  .reveal,
  .cascade > *,
  [data-reveal] {
    animation: none !important;
    transition: none !important;
    opacity: 1 !important;
    transform: none !important;
  }
}
```

---

## Page Transition Patterns

For SPAs and multi-page apps. Keep every transition under 300ms total.

### Fade Through (Cross-fade)

The outgoing page fades out while the incoming page fades in. Simple and
universally applicable.

```css
.page-exit {
  animation: fadeOut 150ms var(--ease) forwards;
}
.page-enter {
  animation: fadeIn 150ms var(--ease) forwards;
}
@keyframes fadeOut { to { opacity: 0; } }
@keyframes fadeIn  { from { opacity: 0; } }
```

Total: 300ms (150ms exit + 150ms enter). Overlap by 50ms for smoother feel.

### Slide (Directional)

Left/right slide communicates back/forward navigation.

```css
.page-slide-forward-enter {
  animation: slideInRight 250ms var(--ease) forwards;
}
.page-slide-forward-exit {
  animation: slideOutLeft 250ms var(--ease) forwards;
}
@keyframes slideInRight  { from { transform: translateX(100%); opacity: 0; } }
@keyframes slideOutLeft  { to   { transform: translateX(-30%); opacity: 0; } }
```

### Shared Element Transition (View Transitions API)

```js
document.startViewTransition(async () => {
  await updateDOM();
});
```

```css
::view-transition-old(root) {
  animation: fadeOut 150ms var(--ease);
}
::view-transition-new(root) {
  animation: fadeIn 150ms var(--ease);
}
```

Use `view-transition-name` on shared elements (e.g., a hero image) so they
animate from old position to new position during the page swap.

---

## Motion States

Define distinct motion states for the entire page.

| State         | Behavior                                        |
|---------------|-------------------------------------------------|
| Loading       | Skeleton shimmer, spinner, pulse animation      |
| Idle          | No animation -- respect battery                  |
| Active        | Hover, focus, press feedback on interactive el.  |
| Transitioning | Page or section changes, progress indicators     |
| Reduced       | `prefers-reduced-motion: reduce` disables all    |
|               | non-essential motion; content shown immediately  |

> **Canonical values:** Duration tokens must match `schemas/token_schema.md` (the canonical authority). The canonical values are: `--dur-fast: 120ms`, `--dur-base: 200ms`, `--dur-slow: 320ms`. There is no `--dur-micro` token — use `--dur-fast` (120ms) for micro-interactions.

```css
:root {
  --ease:      cubic-bezier(0.2, 0.8, 0.2, 1);
  --dur-fast:  120ms;  /* Micro-interactions, hover/focus feedback */
  --dur-base:  200ms;  /* Element entrances, standard transitions */
  --dur-slow:  320ms;  /* Section transitions, complex animations */
}

@media (prefers-reduced-motion: reduce) {
  :root {
    --dur-fast:  0ms;
    --dur-base:  0ms;
    --dur-slow:  0ms;
  }
}
```

---

## Stagger Patterns

### List Items

Stagger by 50ms per item. Cap at 10 items to avoid long waits.

```css
.stagger-list > * {
  animation-delay: calc(min(var(--stagger-index, 0), 9) * 50ms);
}
```

### Grid Cards

Stagger by row (100ms per row) so cards in the same row appear together.

```css
.stagger-grid > * {
  animation-delay: calc(var(--row-index, 0) * 100ms);
}
```

### Metrics (Count-up)

Animate numbers from 0 to target value with staggered start times.
Use `requestAnimationFrame` for smooth count-up, duration 800-1200ms.
Stagger each metric by 150ms.

### Testimonials (Carousel)

Slide timing: 400ms per transition with `var(--ease)`.
Auto-advance interval: 6-8s. Pause on hover/focus.

---

## Duration Scale by Context

| Context            | Duration   | Reasoning                          |
|--------------------|------------|------------------------------------|
| Micro-feedback     | 80-120ms   | Must feel instant (hover, focus)   |
| Element entrance   | 200-400ms  | Needs to be perceived              |
| Section transition | 300-500ms  | Spatial change needs time          |
| Page transition    | 300-450ms  | Context switch                     |
| Decorative loop    | 2-5s       | Background, non-blocking           |

Map to tokens:

```css
:root {
  --dur-fast:  120ms;  /* Micro-feedback and element entrance */
  --dur-base:  200ms;  /* Section/page transition */
  --dur-slow:  320ms;  /* Complex transitions */
  --dur-loop:  3s;     /* Decorative loops */
}
```

---

## Anti-patterns

Avoid the following -- they degrade performance, accessibility, or trust.

- **Auto-playing video backgrounds.** Compete with content, drain battery,
  bandwidth-heavy. Use static images or subtle CSS animations instead.
- **Scroll-jacking.** Hijacking native scroll breaks user expectations and
  causes accessibility failures. Use native scroll with `scroll-snap` if needed.
- **Animations >500ms for routine actions.** Button presses, toggles, modal
  opens should complete in under 300ms. Long durations make the UI feel broken.
- **Bouncing or wiggling elements.** Draws unwanted attention, distracts from
  content. Reserve spring physics for meaningful state changes only.
- **Parallax on mobile.** Causes frame drops, jank, and motion sickness.
  Disable parallax entirely on touch devices or reduced-motion preference.

---

## Reusable Keyframe Library

A set of production-ready keyframe animations. All use `opacity` and `transform`
only (GPU-composited properties for 60fps).

### Fade
```css
@keyframes fadeIn {
  from { opacity: 0; }
  to   { opacity: 1; }
}
@keyframes fadeOut {
  from { opacity: 1; }
  to   { opacity: 0; }
}
```

### Slide
```css
@keyframes slideInUp {
  from { transform: translateY(16px); opacity: 0; }
  to   { transform: translateY(0); opacity: 1; }
}
@keyframes slideInDown {
  from { transform: translateY(-16px); opacity: 0; }
  to   { transform: translateY(0); opacity: 1; }
}
@keyframes slideInLeft {
  from { transform: translateX(-16px); opacity: 0; }
  to   { transform: translateX(0); opacity: 1; }
}
@keyframes slideInRight {
  from { transform: translateX(100%); opacity: 0; }
  to   { transform: translateX(0); opacity: 1; }
}
```

### Scale
```css
@keyframes scaleIn {
  from { transform: scale(0.95); opacity: 0; }
  to   { transform: scale(1); opacity: 1; }
}
@keyframes scaleOut {
  from { transform: scale(1); opacity: 1; }
  to   { transform: scale(0.95); opacity: 0; }
}
```

### Reveal (clip-path)
```css
@keyframes revealDown {
  from { clip-path: inset(0 0 100% 0); }
  to   { clip-path: inset(0 0 0 0); }
}
@keyframes revealUp {
  from { clip-path: inset(100% 0 0 0); }
  to   { clip-path: inset(0 0 0 0); }
}
```

### Shimmer (loading placeholder)
```css
@keyframes shimmer {
  from { background-position: -200% 0; }
  to   { background-position: 200% 0; }
}
```

### Spin (loading indicator)
```css
@keyframes spin {
  from { transform: rotate(0deg); }
  to   { transform: rotate(360deg); }
}
```

### Pulse (attention)
```css
@keyframes pulse {
  0%, 100% { transform: scale(1); }
  50%      { transform: scale(1.05); }
}
```

### Usage Pattern
```css
.animate-in {
  animation: slideInUp 400ms var(--ease) both;
}

.stagger > *:nth-child(1) { animation-delay: 0ms; }
.stagger > *:nth-child(2) { animation-delay: 80ms; }
.stagger > *:nth-child(3) { animation-delay: 160ms; }

@media (prefers-reduced-motion: reduce) {
  .animate-in { animation: fadeIn 200ms var(--ease) both; }
}
```
