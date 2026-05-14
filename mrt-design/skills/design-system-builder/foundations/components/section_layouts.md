## Design Principles

- **Every section has a single visual priority.** A hero section leads with the headline; a pricing section leads with the recommended tier. Mixed priorities create visual noise.
- **Section spacing uses consistent vertical rhythm.** Large sections use `--space-16` to `--space-20` gaps between them; internal elements use the component-level spacing scale.
- **Feature grids auto-fit to avoid orphan columns.** `minmax(280px, 1fr)` ensures cards never shrink below readable width and the grid gracefully adapts from 3 to 2 to 1 column.
- **Social proof anchors trust below the CTA.** Logos, metrics, or testimonials placed directly under call-to-action buttons capitalize on the decision moment.

## Brand Expression

Sections are where `creative_brief`, `tension_points.implementation`, and
`rule_breaking_budget` become visible. Before choosing layout:

- Map `tension_points.scale` into type ratios and visual hierarchy.
- Map `tension_points.density` into section padding and component count.
- Map `tension_points.structure` into grid behavior and intentional grid breaks.
- Apply the `signature_anchor` once as the dominant section moment.
- Propagate signature DNA quietly through dividers, section backgrounds, or state rails.

`safe` sections stay on canonical grids. `elevated` sections may use one asymmetry.
`bold` sections may overlap or break grid. `experimental` sections may use masks,
scroll-linked effects, or kinetic layouts only with fallbacks.

### Hero Section
```
+------------------------------------------------+
|                                                 |
|         [Overline/Badge: "New Feature"]         |
|                                                 |
|         Main Headline Goes Here                 |
|                                                 |
|         Supporting text that adds context        |
|         and stays under 2 lines.                |
|                                                 |
|         [Primary CTA]    [Secondary CTA]        |
|                                                 |
|         [Social proof: logos or metrics]         |
|                                                 |
+------------------------------------------------+
```

### Feature Grid (3-column)
Use CSS Grid: `grid-template-columns: repeat(auto-fit, minmax(280px, 1fr))`

### Pricing Table
- 2-3 tiers side by side
- Highlighted/recommended tier: accent border, "Popular" badge, slightly scaled up
- Feature comparison with checkmarks
- CTA at bottom of each tier, primary CTA only on recommended tier

### Testimonial Section
- Quote text in larger, italic body font
- Attribution: name, role, company
- Optional avatar image
- Carousel or grid layout for multiple testimonials

### Required Rhythm
- Marketing surfaces should use at least three distinct section patterns.
- Dashboard/admin surfaces should prioritize scan -> filter -> inspect -> act -> confirm.
- Docs surfaces should prioritize orient -> navigate -> explain -> reference.
- Settings surfaces should group -> explain consequence -> edit -> save/discard.
