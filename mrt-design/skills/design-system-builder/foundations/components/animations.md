## Design Principles

- **Animations communicate hierarchy and change, not decoration.** Every animation should answer "what appeared, what moved, where did it go?" -- if it does not, remove it.
- **Duration stays between 200ms and 600ms.** Faster than 200ms is imperceptible; slower than 600ms feels sluggish. Page-load reveals can stretch to 600ms; hover feedback should be near-instant at 150-200ms.
- **Staggered reveals create rhythm without chaos.** `nth-child` delays of 100ms intervals produce a natural cascade; never stagger more than 5 items or the sequence feels slow.
- **Easing curves match physical expectation.** Use the design system's `--ease` token (a ease-out curve) for entrances and a neutral ease for state transitions; avoid `linear` for visible motion.

### Page Load Reveal
```css
@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(16px); }
  to { opacity: 1; transform: translateY(0); }
}
.reveal {
  animation: fadeInUp 0.6s var(--ease) both;
}
.reveal:nth-child(1) { animation-delay: 0.1s; }
.reveal:nth-child(2) { animation-delay: 0.2s; }
.reveal:nth-child(3) { animation-delay: 0.3s; }
```

### Hover Lift
```css
.hover-lift {
  transition: transform var(--dur-slow) var(--ease), box-shadow var(--dur-slow) var(--ease);
}
.hover-lift:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-md);
}
```

### Underline Grow (for links)
```css
.link-underline {
  position: relative;
  text-decoration: none;
}
.link-underline::after {
  content: '';
  position: absolute;
  bottom: -2px;
  left: 0;
  width: 0;
  height: 2px;
  background: var(--accent);
  transition: width var(--dur-base) var(--ease);
}
.link-underline:hover::after {
  width: 100%;
}
```
