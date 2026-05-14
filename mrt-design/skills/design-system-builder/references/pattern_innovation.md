# Pattern Innovation Reference

Unusual layouts, novel interactions, and techniques for creating signature moments.

---

## Unusual Layout Patterns

### 1. Asymmetric Hero

Off-center content with a decorative or illustrative background element filling the opposite side.

```
+--------------------------------------------------+
|                                    +----------+   |
|   HEADLINE                         |          |   |
|   Subheadline                      |  VISUAL  |   |
|   [CTA Button]                     |  ELEMENT |   |
|                                    |          |   |
|                                    +----------+   |
+--------------------------------------------------+
```

- **CSS Approach:** CSS Grid with `grid-template-columns: 1fr 1fr` or `45% 55%`. Content cell uses `align-self: center`. Visual cell can use `background-size: cover` or an absolutely positioned illustration that extends beyond its grid cell.
- **When appropriate:** Landing pages, product pages, any hero that needs visual punch. Works for brands with strong illustration or photography.
- **Accessibility:** Ensure reading order is logical in DOM (content before visual). Visual element must be decorative (`aria-hidden="true"`) or have alt text if informational. Maintain contrast regardless of background element.

### 2. Bento Dashboard

Mixed-size cards in a CSS Grid resembling a Japanese bento box layout.

```
+----------+------------------+---------+
|          |                  |         |
|  SMALL   |     LARGE        |  SMALL  |
|          |                  |         |
+----------+                  +---------+
|          |                  |         |
|  SMALL   |                  |  SMALL  |
|          |                  |         |
+----------+------------------+---------+
|                         |             |
|     WIDE                |   MEDIUM    |
|                         |             |
+-------------------------+-------------+
```

- **CSS Approach:** `grid-template-columns: repeat(4, 1fr)` with specific cells spanning `grid-column: span 2` or `grid-row: span 2`. Use `grid-template-areas` for named layout control. Gap-based spacing from the design system's spacing scale.
- **When appropriate:** Product feature showcases, dashboard overviews, pricing page alternatives, any page with 4-8 distinct content blocks of varying importance.
- **Accessibility:** Each card must be a semantic landmark or section. Ensure tab order follows a logical reading pattern (top-left to bottom-right). Card content must meet contrast requirements independently.

### 3. Split Scroll

Two panels scrolling independently within the same viewport.

```
+------------------------+------------------------+
|  LEFT PANEL            |  RIGHT PANEL           |
|  (scrolls              |  (scrolls              |
|   independently)       |   independently)       |
|                        |                        |
|  Content A             |  Content X             |
|  --------              |  --------              |
|  Content B             |  Content Y             |
|  --------              |  --------              |
|  Content C             |  Content Z             |
+------------------------+------------------------+
```

- **CSS Approach:** Flex container with two children. Each child gets `overflow-y: auto` and `height: 100vh` (or a fixed height). Optionally sync scroll positions with JavaScript using `scroll` event listeners and `scrollTop` calculations.
- **When appropriate:** Code editors, documentation with sidebar navigation, comparison views, case study text alongside imagery.
- **Accessibility:** Each panel needs its own scroll container with `tabindex="0"` for keyboard access. Provide a visual indicator that panels scroll independently. Test with screen readers to ensure content discoverability.

### 4. Scroll Zoom

Elements scale up from small to full-size as they enter the viewport.

```
Scroll Position 1 (element below fold):
+------------------------------------------+
|                                          |
|              [tiny card]                 |
|                                          |
+------------------------------------------+

Scroll Position 2 (element entering view):
+------------------------------------------+
|                                          |
|         [  medium card  ]                |
|                                          |
+------------------------------------------+

Scroll Position 3 (element centered):
+------------------------------------------+
|                                          |
|    [      FULL SIZE CARD      ]          |
|    [      with full content    ]         |
|                                          |
+------------------------------------------+
```

- **CSS Approach:** Intersection Observer with `threshold: [0, 0.25, 0.5, 0.75, 1]` array. JavaScript applies `transform: scale(${progress})` where progress maps from 0.8 to 1.0 based on intersection ratio. CSS `will-change: transform` for performance.
- **When appropriate:** Product showcases, portfolio items, storytelling pages with sequential reveals.
- **Accessibility:** Content must be readable at the smallest scale. Provide `prefers-reduced-motion` fallback where elements appear at full size without animation. Do not hide content behind zoom interaction.

### 5. Sticky Sections

Sections that stick and stack as the user scrolls, creating an overlapping card deck effect.

```
Initial state:           After scrolling:
+--------------------+   +--------------------+
|                    |   |   SECTION 3        |
|   SECTION 3        |   +--------------------+
|                    |   |   SECTION 2        |
+--------------------+   +--------------------+
|                    |   |   SECTION 1        |
|   SECTION 2        |   |   (sticky, visible)|
|                    |   +--------------------+
+--------------------+
|                    |
|   SECTION 1        |
|                    |
+--------------------+
```

- **CSS Approach:** Each section uses `position: sticky; top: ${index * offset}px`. Subsequent sections have a higher `z-index` and a background that covers the previous section. The offset creates the stacking visual.
- **When appropriate:** Process steps, feature lists, storytelling with sequential reveals, case study timelines.
- **Accessibility:** Ensure all content is accessible without scrolling (provide jump links). Sticky positioning must not trap keyboard focus. Screen readers should encounter content in DOM order.

### 6. Horizontal Scroll

Horizontal galleries or content sections embedded within a vertical scroll page.

```
+--------------------------------------------------+
|  Section Title                                    |
|  +-+  +-+  +-+  +-+  +-+  +-+                   |
|  |1|  |2|  |3|  |4|  |5|  |6|  --> scroll -->    |
|  +-+  +-+  +-+  +-+  +-+  +-+                   |
|                                                    |
+--------------------------------------------------+
|  Next vertical section...                          |
+--------------------------------------------------+
```

- **CSS Approach:** Container with `overflow-x: auto; scroll-snap-type: x mandatory;`. Children use `scroll-snap-align: start` and `flex: 0 0 auto` with a fixed width. Add custom scrollbar or scroll indicators. Optionally use CSS `scroll-timeline` for linked animations.
- **When appropriate:** Product carousels, team members, case study cards, logo walls, portfolio galleries, testimonial rows.
- **Accessibility:** Must be keyboard navigable (arrow keys, Tab). Provide scroll indicators (dots or arrows). Touch devices get native swipe. Announce current position to screen readers (`aria-roledescription="carousel"`).

### 7. Full-bleed Breakouts

Elements that break out of the content grid to span the full viewport width.

```
+--------------------------------------------------+
|        |  Content within grid  |                  |
|        +-----------------------+                  |
|        |                       |                  |
+--------+-----------------------+------------------+
|                                                   |
|  FULL-BLEED IMAGE OR SECTION                      |
|  (spans entire viewport width)                    |
|                                                   |
+--------------------------------------------------+
|        |  Content resumes in grid |               |
|        +-----------------------+                  |
+--------------------------------------------------+
```

- **CSS Approach:** Content in a centered `max-width` container. Breakout element uses `width: 100vw; margin-left: calc(-50vw + 50%)` or is placed outside the container. CSS Grid alternative: named grid areas that span edge-to-edge columns.
- **When appropriate:** Hero images, data visualizations, video embeds, case study photography, any visual that benefits from maximum impact.
- **Accessibility:** Breakout images need descriptive alt text. Ensure text over breakout images meets contrast requirements. Breakout should not create horizontal scrolling on mobile.

### 8. Overlapping Layers

Cards or sections that intentionally overlap using negative margins or absolute positioning.

```
+------------------+
|   CARD A         |
|                  +------------------+
+------------------+   CARD B         |
                   |                  +------------------+
                   +------------------+   CARD C         |
                                      |                  |
                                      +------------------+
```

- **CSS Approach:** Negative `margin-top` on subsequent cards (e.g., `margin-top: -40px`), or CSS Grid with overlapping `grid-area` assignments. Use `z-index` for stacking control. Ensure the overlapping element has a background to cover the edge of the previous element.
- **When appropriate:** Testimonial stacks, feature card cascades, pricing tier comparisons, portfolio entries.
- **Accessibility:** Overlapping must not hide interactive elements. Ensure focus order is logical. Content in overlapping cards must be independently readable. Test with zoom at 200% to verify layout holds.

### 9. Curtain Reveal

Sections slide up to reveal content beneath, creating a theatrical unveiling effect.

```
Before scroll:            After scroll:
+-------------------+     +-------------------+
|                   |     |   REVEALED        |
|   CURTAIN         |     |   CONTENT         |
|   (slides up)     |     |                   |
|                   |     +-------------------+
+-------------------+     |   NEXT SECTION    |
|   (hidden below)  |     |                   |
+-------------------+     +-------------------+
```

- **CSS Approach:** Curtain element uses `position: sticky; top: 0; z-index: 10` with a `transform: translateY()` driven by scroll progress via Intersection Observer or scroll event. The revealed content sits in normal flow below.
- **When appropriate:** Product reveals, storytelling transitions, brand moments, case study openers.
- **Accessibility:** Content below the curtain must be in the DOM and accessible to screen readers regardless of scroll position. Provide a "skip to content" option. Respect `prefers-reduced-motion`.

### 10. Parallax Depth

Background, midground, and foreground layers moving at different speeds.

```
+------------------------------------------+
|  Background: slow scroll (0.3x)         |
|     +------------------------------+     |
|     | Midground: normal scroll (1x)|     |
|     |   +------------------------+ |     |
|     |   | Foreground: fast (1.5x)| |     |
|     |   +------------------------+ |     |
|     +------------------------------+     |
+------------------------------------------+
```

- **CSS Approach:** CSS `background-attachment: fixed` for simple parallax. For multi-layer: `transform: translate3d(0, ${offset}px, 0)` driven by scroll position. Use `will-change: transform` and `transform: translateZ(0)` for GPU acceleration. CSS-only via `animation-timeline: scroll()`.
- **When appropriate:** Storytelling pages, product showcases, creative portfolios, immersive brand experiences.
- **Accessibility:** Provide a `prefers-reduced-motion` fallback where all layers scroll at the same speed. Do not rely on parallax for conveying information. Test on mobile (parallax often disabled for performance).

---

## Novel Interaction Patterns

### 1. Scroll-linked Video

Video playback is tied to scroll position, playing forward as you scroll down and backward as you scroll up.

- **Description:** A video element whose `currentTime` is mapped to the scroll progress of a container. Scrolling down advances the video; scrolling up reverses it. Creates cinematic control through scrolling.
- **JavaScript Approach:** Use Intersection Observer to detect when the video container is in view. On scroll events within the container, calculate `progress = scrollY / containerHeight` and set `video.currentTime = progress * video.duration`. Remove audio, preload the full video, and use `video.pause()` immediately.
- **UX Rules:** Video must be short (3-10 seconds). Provide a play/pause toggle for users who prefer autoplay. Show a subtle progress indicator. Video quality should degrade gracefully on slow connections (use poster image fallback).
- **When to use:** Product demonstrations (assembly, transformation), brand storytelling, hero sections that need cinematic impact without requiring user patience.

### 2. Magnetic Buttons

CTA buttons that slightly follow the cursor when the cursor is nearby, creating a subtle pull effect.

- **Description:** When the cursor enters a proximity zone around a button, the button translates slightly toward the cursor position. When the cursor leaves, the button springs back to its original position.
- **JavaScript Approach:** Listen for `mousemove` on the document. Calculate distance between cursor and button center. If within threshold (e.g., 100px), apply `transform: translate(${dx * 0.3}px, ${dy * 0.3}px)` where dx/dy are the offset from cursor to button center. Use `requestAnimationFrame` for smooth updates. Spring-back uses CSS `transition: transform 0.4s cubic-bezier(0.2, 0.8, 0.2, 1)`.
- **UX Rules:** Movement should be subtle (max 8-12px offset). Only apply to primary CTAs, not every button. Disable on touch devices (no hover). Disable with `prefers-reduced-motion`.
- **When to use:** Primary CTAs on landing pages, hero buttons, premium brand experiences.

### 3. Expandable Cards

Cards that expand in-place to reveal full content without navigating to a new page.

- **Description:** Clicking or hovering a compact card triggers an animation that expands it to show additional detail, images, or actions. The expansion uses the card's existing position to avoid disorienting layout shifts.
- **JavaScript Approach:** Use FLIP animation technique (First, Last, Invert, Play). Record the card's initial position, apply the expanded state, calculate the difference, animate from the inverted position to the final position. Libraries like `flip-toolkit` or GSAP `Flip` plugin simplify this.
- **UX Rules:** Expansion must be reversible (click to collapse). Other cards should reflow smoothly. The expanded state should feel like a natural growth, not a new page. Maintain focus management for keyboard users.
- **When to use:** Portfolio entries, team member cards, product cards in e-commerce, feature explanations.

### 4. Drag-to-Reveal

A drag interaction that reveals hidden content beneath a surface layer.

- **Description:** Users drag a handle or the surface itself to peel back a layer and reveal content underneath. Can be horizontal wipe, vertical pull, or circular reveal.
- **JavaScript Approach:** Track pointer events (`pointerdown`, `pointermove`, `pointerup`). Calculate drag distance and apply `clip-path: inset(0 ${100 - progress}% 0 0)` for a horizontal wipe. Use `clip-path: circle(${radius}px at ${x}px ${y}px)` for a circular reveal from the drag point.
- **UX Rules:** Provide a clear visual affordance (handle, edge hint, "drag to reveal" label). The reveal should be completable with a single gesture. On touch devices, ensure the drag does not conflict with scroll. Provide a "reveal all" button for accessibility.
- **When to use:** Before/after comparisons, reveal animations, interactive storytelling, product transformation demos.

### 5. Cursor Effects

Custom cursor states that enhance the brand identity through subtle visual modifications.

- **Description:** The default cursor is replaced or augmented with a custom visual element that changes based on context: larger on hoverable elements, different color on dark/light backgrounds, branded shape near CTAs.
- **JavaScript Approach:** Hide the default cursor with `cursor: none`. Create a fixed-position element that follows the cursor position using `requestAnimationFrame`. Apply different states via CSS classes (`cursor--hover`, `cursor--click`). Use `mix-blend-mode: difference` for visibility on all backgrounds. Add lerp (linear interpolation) for smooth trailing effect.
- **UX Rules:** The custom cursor must never obscure content. It should be subtle (small, translucent). Provide a `prefers-reduced-motion` fallback to the system cursor. Never use on touch devices. The custom cursor must not delay interaction (no lag on click).
- **When to use:** Creative agency sites, portfolio sites, brand experiences, artistic projects. Avoid on productivity tools and e-commerce.

### 6. Keyboard Shortcuts

Power-user keyboard navigation with visual hints showing available shortcuts.

- **Description:** Interactive elements display keyboard shortcut badges (e.g., `J/K` for navigation, `Enter` to select, `/` to search). Pressing the key triggers the action. A cheat sheet overlay (press `?`) shows all shortcuts.
- **JavaScript Approach:** Global `keydown` event listener with a shortcut map. Elements display `<kbd>` tags as visual hints. Use `event.key` matching. Shortcut overlay is a modal toggled by `?`. Store user preference for showing/hiding hints.
- **UX Rules:** Shortcuts must never override browser defaults on input fields. Display hints progressively (show after first interaction, or on hover). Ensure all shortcut actions are achievable without the keyboard (mouse/touch alternatives).
- **When to use:** Developer tools, productivity applications, dashboards, any application where users return frequently.

### 7. Command Palette

A Cmd+K (or Ctrl+K) activated search and command interface.

- **Description:** A modal overlay with a search input that fuzzy-matches across actions, pages, settings, and content. Patterned after VS Code's command palette and Raycast. Supports keyboard navigation through results.
- **JavaScript Approach:** Listen for `Cmd+K` / `Ctrl+K` keyboard event. Render a fixed overlay with search input. Index available commands with prebuilt search index (Fuse.js for fuzzy search). Arrow keys navigate results, Enter selects. Close on Escape or click outside.
- **UX Rules:** Must open instantly (no loading). Search results appear after 1 character. Keyboard navigation is primary (arrow keys + Enter). Visual hierarchy: icon, name, description, shortcut badge. Must work on mobile (tap to open).
- **When to use:** Applications with 10+ pages or actions, documentation sites, productivity tools, any product where navigation depth exceeds 2 levels.

### 8. Contextual Toolbars

A floating toolbar that appears near selected or hovered content with relevant actions.

- **Description:** When a user selects text, hovers an element, or right-clicks, a small toolbar appears adjacent to the element with context-specific actions (copy, link, format, share).
- **JavaScript Approach:** Detect selection via `selectionchange` event. Calculate toolbar position from selection range using `range.getBoundingClientRect()`. Render a positioned element with `position: absolute`. Dismiss on click outside, Escape, or selection clear. Animate in with `transform: scale(0.9)` to `scale(1)`.
- **UX Rules:** Toolbar appears after a 300ms delay on hover (not instant). Contains 3-5 actions maximum. Actions are icon-only with tooltips. Does not overlap the selected content. Disappears immediately when selection is lost.
- **When to use:** Content editors, documentation readers, email clients, any interface where users interact with selectable content.

---

## Type Treatments That Create Identity

### 1. Variable Font Animation

Animating custom font variation axes to create living typography.

- **Approach:** Use `font-variation-settings` in CSS `@keyframes` to animate between axis values. Example: animating the "wght" axis from 300 to 800 on hover, or cycling the "wonk" axis in Fraunces for character.
- **Best for:** Hero headings, brand identity moments, loading states.
- **CSS:** `@keyframes weight-pulse { from { font-variation-settings: 'wght' 400; } to { font-variation-settings: 'wght' 800; } }`

### 2. Split-color Text

Text rendered with two colors across different parts (top/bottom or left/right split).

- **Approach:** Use `background: linear-gradient(to bottom, #color1 50%, #color2 50%)` on the text element with `-webkit-background-clip: text` and `-webkit-text-fill-color: transparent`. Adjust the split point and angle.
- **Best for:** Hero headlines, section dividers, brand identity headings.
- **CSS:** `.split-text { background: linear-gradient(180deg, #fff 49%, #666 51%); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }`

### 3. Outline + Fill

Outlined headings that fill with color on scroll or hover.

- **Approach:** Default state uses `-webkit-text-stroke: 2px currentColor` with transparent fill. On hover or scroll trigger, animate `-webkit-text-fill-color` from transparent to the accent color. Optionally use `clip-path` for directional fill.
- **Best for:** Section headings, navigation items, feature labels, brand typography moments.
- **CSS:** `.outline-fill { -webkit-text-stroke: 2px var(--accent); -webkit-text-fill-color: transparent; transition: -webkit-text-fill-color 0.3s; } .outline-fill:hover { -webkit-text-fill-color: var(--accent); }`

### 4. Gradient Text

Text filled with a gradient rather than a solid color.

- **Approach:** Apply a gradient background with `background-clip: text` and `transparent` text fill. Animate the gradient position for shimmer effects. Use brand colors in the gradient.
- **Best for:** Hero headlines, brand name display, accent headings.
- **CSS:** `.gradient-text { background: linear-gradient(135deg, var(--accent), var(--accent-light)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }`

### 5. Monospace Accent

Mixing monospace typeface into headings for a technical or editorial feel.

- **Approach:** Wrap specific words or numbers in a `<span>` with a monospace font family. Creates contrast between the primary font's personality and the monospace's precision.
- **Best for:** Technical products, developer tools, data-driven content, version numbers, code references.
- **CSS:** `.mono-accent span { font-family: var(--font-mono); letter-spacing: -0.02em; opacity: 0.7; }`

### 6. Oversized Drop Caps

A large first letter serving as a design element, not just a typographic convention.

- **Approach:** Use `::first-letter` pseudo-element with oversized font-size (4-6rem), float left, negative margin for overlap, and a distinct color or font weight. Optionally use a display or serif font for the drop cap only.
- **Best for:** Editorial layouts, blog posts, long-form content, newsletter-style pages.
- **CSS:** `.drop-cap::first-letter { font-size: 4.5rem; float: left; line-height: 0.8; margin-right: 0.1em; margin-top: 0.1em; color: var(--accent); font-weight: 700; }`

### 7. Text Mask

Text used as a mask for a background image or video, showing the visual through the letterforms.

- **Approach:** Apply `background-image` to the text element with `background-clip: text` and `transparent` fill. The image shows through the letter shapes. Works best with large, bold type.
- **Best for:** Hero sections, brand identity moments, creative portfolios, seasonal campaigns.
- **CSS:** `.text-mask { font-size: clamp(4rem, 10vw, 8rem); font-weight: 900; background-image: url('hero.jpg'); background-size: cover; background-position: center; -webkit-background-clip: text; -webkit-text-fill-color: transparent; }`

---

## Signature Moment Catalog

Techniques for creating one memorable element per page that users remember and associate with the brand.

### Hero Parallax with Depth Layers

Three or more layers moving at different speeds to create a sense of physical depth in the hero section. Background moves slowest, foreground fastest. Works with illustrations, photography, or abstract shapes.

- **Components:** 3+ absolutely positioned layers, scroll event listener, `transform: translateY()` at different rates.
- **Effort:** Medium. Requires layered visual assets.
- **Impact:** High. Creates immediate visual distinction.

### Animated SVG Illustration

A custom SVG illustration with animated elements (floating, pulsing, rotating) that feel alive without being distracting.

- **Components:** Custom SVG with named groups, CSS `@keyframes` or SMIL animation, `prefers-reduced-motion` fallback to static state.
- **Effort:** High. Requires custom illustration work.
- **Impact:** High. Unique visual that no competitor has.

### Scroll-triggered Counter Animation

Numbers that count up from zero to their target value as they scroll into view, often used for statistics sections.

- **Components:** Intersection Observer to detect visibility, `requestAnimationFrame` loop with easing function, formatted number display (commas, units, suffixes).
- **Effort:** Low. Well-documented pattern.
- **Impact:** Medium. Effective but common; elevate with custom easing or unit reveals.

### Interactive Demo Embedded in Page

A working product demo (not a screenshot or video) embedded directly in the landing page, allowing users to interact with real functionality.

- **Components:** iframe or inline component, sandboxed environment, pre-populated demo data, clear "try it" affordance.
- **Effort:** High. Requires engineering investment in demo infrastructure.
- **Impact:** Very high. Nothing converts like actual product experience.

### Split-screen with Video

A hero section split between content and a looping background video that demonstrates the product or creates mood.

- **Components:** CSS Grid 50/50 split, `<video>` with `autoplay muted loop playsinline`, poster image fallback, text overlay with content and CTA.
- **Effort:** Medium. Requires video asset production.
- **Impact:** High. Video communicates faster than text for product demonstration.

### 3D Tilt Card on Hover

Cards that respond to cursor position with a subtle 3D tilt effect, creating a tangible, physical feel.

- **Components:** `mousemove` event listener on card, `transform: perspective(1000px) rotateX(${ry}deg) rotateY(${rx}deg)`, specular highlight overlay that follows cursor, `mouseleave` reset animation.
- **Effort:** Low-Medium. Pure front-end implementation.
- **Impact:** Medium-High. Creates premium feel, encourages interaction.

### Custom Cursor with Brand Element

A cursor replacement or augmentation that incorporates a brand element (dot, ring, logo fragment, color).

- **Components:** Hidden default cursor, fixed-position cursor element following `mousemove`, context-sensitive states (enlarge on hover, change on click), lerp smoothing for trailing effect.
- **Effort:** Medium. Requires careful state management and performance optimization.
- **Impact:** Medium. Memorable but can polarize users.

### Loading Animation That Becomes the Page

An initial loading animation that transitions seamlessly into the page content, so the animation is not discarded but transformed.

- **Components:** Initial loading state with animated elements, CSS transition or FLIP animation morphing loading elements into their final positions, `window.load` or custom trigger to begin transition.
- **Effort:** High. Requires coordination between loading and loaded states.
- **Impact:** Very high. Creates a cohesive, premium experience when executed well.

---

## Implementation Notes

When using any pattern from this reference:

1. **Start with purpose.** Every pattern must serve a user need or business goal. If you cannot articulate why a pattern is needed, do not use it.
2. **Respect motion preferences.** All animation patterns must have a `prefers-reduced-motion: reduce` fallback that either removes animation or replaces it with a simple opacity fade.
3. **Test on mobile.** Hover-dependent patterns (magnetic buttons, cursor effects, 3D tilt) need mobile alternatives or graceful degradation.
4. **Measure performance.** Every animation pattern should maintain 60fps. Use Chrome DevTools Performance panel to verify. If a pattern drops frames, simplify or remove it.
5. **One per page.** A signature moment loses its impact if competing with other visual effects. Pick one moment per page and let everything else support it.
