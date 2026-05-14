# Output Schemas

Templates and structures for every deliverable the design system produces.

---

## Framework Selection

All generated output starts with framework-agnostic `tokens.css`. By default, agents produce HTML output only. Framework-specific component output (React, Vue, Astro, Svelte) is generated only when `constraints.framework` in the context packet specifies a framework -- via the optional Phase 4b Framework Adaptation step (see `workflow/agent_decomposition.md`).

| Framework | Extension | Component Format | State Management |
|-----------|-----------|-----------------|------------------|
| HTML | `.html` | Single file with embedded CSS | Vanilla JS |
| React | `.tsx` | Functional component with props | useState/useRef |
| Vue | `.vue` | SFC with `<script setup>` | ref/reactive |
| Astro | `.astro` | Component with `---` script | Props only |
| Svelte | `.svelte` | Component with `<script>` | $state/$props |

**Framework-specific output is only generated when `constraints.framework` in the context packet specifies a framework.** If no framework is specified or `constraints.framework` is `"html"`, agents produce HTML output only.

### HTML Output (default -- always generated)
```
design-system/
├── tokens.css
├── design-system.html
├── preview/
│   ├── _card.css
│   ├── colors-primary.html
│   ├── colors-neutrals.html
│   └── ... (17 preview files)
├── ui_kits/
│   └── [surface]/
│       ├── README.md
│       └── index.html
├── README.md
├── DECISIONS.md
└── SKILL.md
```

### React Output (only when `constraints.framework: react`)
```
design-system/
├── tokens.css
├── design-system.html
├── preview/
│   └── ... (17 preview files -- always HTML)
├── src/
│   ├── components/
│   │   ├── Button.tsx
│   │   ├── Card.tsx
│   │   ├── Input.tsx
│   │   └── ...
│   ├── hooks/
│   │   └── useTheme.ts
│   └── index.ts
├── package.json
├── README.md
├── DECISIONS.md
└── SKILL.md
```

### Vue Output (only when `constraints.framework: vue`)
```
design-system/
├── tokens.css
├── design-system.html
├── preview/
│   └── ... (17 preview files -- always HTML)
├── src/
│   ├── components/
│   │   ├── Button.vue
│   │   ├── Card.vue
│   │   ├── Input.vue
│   │   └── ...
│   ├── composables/
│   │   └── useTheme.ts
│   └── index.ts
├── package.json
├── README.md
├── DECISIONS.md
└── SKILL.md
```

### Astro Output (only when `constraints.framework: astro`)
```
design-system/
├── tokens.css
├── design-system.html
├── preview/
│   └── ... (17 preview files -- always HTML)
├── src/
│   ├── components/
│   │   ├── Button.astro
│   │   ├── Card.astro
│   │   ├── Input.astro
│   │   └── ...
│   └── index.ts
├── package.json
├── README.md
├── DECISIONS.md
└── SKILL.md
```

### Svelte Output (only when `constraints.framework: svelte`)
```
design-system/
├── tokens.css
├── design-system.html
├── preview/
│   └── ... (17 preview files -- always HTML)
├── src/
│   ├── lib/
│   │   ├── components/
│   │   │   ├── Button.svelte
│   │   │   ├── Card.svelte
│   │   │   ├── Input.svelte
│   │   │   └── ...
│   │   └── index.ts
│   └── app.html
├── package.json
├── README.md
├── DECISIONS.md
└── SKILL.md
```

**Note:** Preview files are ALWAYS standalone HTML regardless of framework. They demonstrate the design system visually without requiring a build step. Framework-specific output trees (src/, package.json, .tsx/.vue/.astro/.svelte files) are only generated when Phase 4b Framework Adaptation is triggered by a `constraints.framework` setting in the context packet.

---

## README.md Template

```markdown
# [Brand] Design System

A design system for [Brand] ([domain]) — [one-line description].

## Sources
[Document what was provided vs inferred. Flag all substitutions.]

## Brand essence
- **Name:** [brand name]
- **Domain:** [domain]
- **Tagline:** [tagline]
- **What they sell:** [product/service description]
- **Who buys:** [target audience]
- **Voice in three words:** [word], [word], [word]

## Signature look
[3-5 visual rules that define the system's identity. The "do not break" rules.]

## Content fundamentals
### Tone
[Voice description with examples of good and bad copy.]
### Casing
[Document the casing choice: sentence, title, etc.]
### Length
[Length rules for each element type.]
### Example copy
[Good example vs bad example with explanation.]

## Visual foundations
### Color
[Token table with role, token name, value, and usage.]
[Color rules: how accent is used, gradient policy, temperature.]
### Typography
[Primary language/script, secondary language support, font choices with brand-value
rationale, expressive page-part roles, fallback stacks, and scale table with size,
weight, line-height, letter-spacing, and usage.]
### Spacing
[Base unit, scale values, application guide.]
### Radii
[Scale values with usage contexts.]
### Shadows / elevation
[Elevation levels with shadow values and use cases.]
### Borders
[Default border treatment for light and dark surfaces.]
### Imagery
[Color vibe, subject guidelines, treatment rules.]
### Motion
[Easing, durations, hover/press/focus states table.]
### Layout rules
[Container width, grid, nav height, section rhythm.]

## Iconography
[Icon library, style, sizing, color rules, spacing.]

## Project index
[File tree showing all generated files.]

## Caveats
[Substitution flags for all inferred assets.]
```

---

## Preview Dashboard Structure

`design-system.html` — a single-page browseable dashboard.

### Required sections (in sidebar order)

**Foundations:**
1. Overview — system metadata, fonts, icons, grid, version
2. Color — token cards showing swatches with hex values
3. Typography — live specimens for each type level
4. Spacing — visual bars showing the scale
5. Radii — boxes at each radius level
6. Shadows — elevation demonstrations

**Components:**
7. Buttons — all variants and states
8. Cards — card variants with hover states
9. Forms — inputs, labels, helpers, error states
10. Badges — badge variants
11. Navigation — nav bar preview

**Brand (if applicable):**
12. Logo — lockup variations

**UI Kits:**
13. Links to click-through prototypes

### Technical requirements
- Sticky top bar with brand name
- Scrollspy sidebar navigation
- Responsive layout (sidebar collapses on mobile)
- All styles reference the CSS variables file
- Chosen icon library initialized only when `constraints.icon_cdn` or local assets are provided

---

## UI Kit Structure

For each surface type (marketing site, dashboard, settings/admin, docs):

> By default, agents produce plain HTML output. Framework-specific component files (.tsx, .vue, .astro, .svelte) are only generated during the optional Phase 4b Framework Adaptation when `constraints.framework` specifies a framework. The default structure is:

```
ui_kits/[surface-type]/
├── README.md          <- What this kit covers, how to use it
└── index.html         <- Interactive click-through prototype (all components inline)
```

When Phase 4 is triggered with a framework target, individual component files are extracted into the appropriate framework structure (see Framework Selection trees above).

### Marketing site kit components
Standard set: Nav, Hero, Features/ServiceCards, Pricing, Testimonials,
ContactForm, Footer.

### Dashboard / admin kit components
Standard set: SidebarNav, HeaderBar, StatsCards, DataTable, Charts,
SettingsPanel, EmptyState, Filters, BulkActions, AuditTrail.

### Settings / admin surface guidance
When `surfaces` includes settings, admin, or CRUD-heavy tools, generate:
- Grouped form sections with labels, helpers, validation, and save/discard actions
- Toggle rows with consequence copy
- Destructive zone with confirmation pattern
- Permissions or roles table where relevant
- Audit trail or activity history
- Empty, loading, and error states

### Settings / admin kit components
Standard set: FormGroups (label + input + description + error), ToggleSections
with save/discard footer, CRUDTable (search + filter + pagination + inline actions),
PermissionMatrix (roles vs capabilities), AuditTrail (timestamped activity log),
BreadcrumbNav for nested pages, EmptyState, LoadingSkeleton, DestructiveZone.

When the surface is primarily CRUD-heavy, prioritize table patterns and form-group
consistency over decorative elements. Use the design system's spacing scale at
compact density. Labels in a fixed-width left column, inputs right-aligned.
Group settings into visually distinct sections with background alternation.

### Kit README template
```markdown
# [Surface Type] UI Kit

A click-through prototype for [surface type] built with the [Brand] design system.

## What's included
- [Component list with descriptions]

## How to use
Open `index.html` in a browser. Navigation links between sections work.
Form inputs accept input. All hover and focus states are live.

## Component map
| Section in index.html | Component | Description |
|------------------------|-----------|-------------|
| Primitives | Button, Input, Badge, Card | Base styled elements using tokens |
| Nav | Navigation | Sticky top nav with mobile collapse |
| ... | ... | ... |

When Phase 4b Framework Adaptation is triggered, this table maps to individual framework component files instead.
```

---

## Preview Files

Individual component previews go in `preview/`:

```
preview/
├── _card.css              <- Shared preview card styles
├── colors-primary.html    <- Accent color swatches
├── colors-neutrals.html   <- Neutral scale swatches
├── colors-semantic.html   <- Success/warning/error swatches
├── type-display.html      <- Display and heading specimens
├── type-body.html         <- Body and label specimens
├── type-mono.html         <- Monospace specimens
├── spacing-scale.html     <- Visual spacing scale
├── radii.html             <- Border radius examples
├── shadows.html           <- Elevation demonstrations
├── buttons.html           <- Button variants and states
├── buttons-on-dark.html   <- Buttons on dark background
├── form-inputs.html       <- Input states and types
├── cards.html             <- Card variants
├── badges.html            <- Badge/tag variants
├── nav-dark.html          <- Navigation on dark bg
├── logo-lockups.html      <- Logo on light/dark/accent
└── iconography.html       <- Icon grid and sizing
```

Each preview file:
- Is self-contained HTML (includes CSS variables inline or via link)
- Shows all states (default, hover, focus, disabled, error)
- Can be opened standalone in a browser
- Uses representative content (not "Lorem ipsum")

---

## Framework-Aware Generation

By default, all generation agents (A-F) produce framework-agnostic HTML output. Framework-specific component output is handled separately:

1. Agents A-G always produce the HTML output tree (tokens.css, .html files, preview/, ui_kits/).
2. If `constraints.framework` specifies a framework in the context packet, an optional Phase 4b Framework Adaptation agent reads the generated HTML + `schemas/framework_targets.md` and converts components to the target framework.
3. Preview files are ALWAYS standalone HTML regardless of framework selection.

If no framework is specified, only the default HTML output is generated. See `workflow/agent_decomposition.md` for Phase 4 details.

---

## DECISIONS.md Template

Generated alongside README.md. Captures every design decision with rationale for traceability.

```markdown
# Design Decisions — [Brand Name]

Every token, color, font, and layout choice in this system was made deliberately.
This file records why.

## Decision Record Format (required for every major decision)
| Field | Description |
|------|-------------|
| ID | Stable identifier (`D-001`, `D-002`, ...) |
| Decision | What was chosen |
| Rationale | Why it was chosen |
| User Context | Which interview/context signal required this choice |
| Principle | UX/design-system principle or foundation reference |
| Accessibility | WCAG/APCA implication (or `n/a`) |
| Research | Competitor/external reference used (or `n/a`) |

## Aesthetic Direction
- **Decision:** [preset name or custom description]
- **Rationale:** [why this aesthetic fits the brand/audience]
- **Source:** [interview response, user confirmation, or derived from X]
- **User Context:** [audience, trust level, domain, product type]
- **Principle:** [visual hierarchy, recognition-over-recall, consistency, etc.]
- **Accessibility:** [contrast, motion, readability implications]
- **Research:** [reference that influenced this choice, or `n/a`]

## Color System
- **Decision:** [accent hex, neutral temperature, palette approach]
- **Rationale:** [why these colors]
- **Source:** [where the color came from — brand guidelines, preset, user pick]
- **User Context:** [brand constraints, domain expectations]
- **Principle:** [semantic clarity, contrast safety, recognizability]
- **Accessibility:** [target contrast policy and implications]
- **Research:** [industry pattern or benchmark]

## Typography
- **Decision:** [display font, body font, scale ratio]
- **Rationale:** [why these fonts and scale]
- **Source:** [user-provided, preset, Google Fonts pairing]
- **User Context:** [audience reading needs, content density]
- **Principle:** [readability, hierarchy, rhythm]
- **Accessibility:** [minimum size/line-height/weight impact]
- **Research:** [reference typography precedent]

## Spacing & Layout
- **Decision:** [base unit, max-width, density tier]
- **Rationale:** [why this spacing]
- **Source:** [derived from content density / user preference]
- **User Context:** [scan behavior, device mix]
- **Principle:** [information hierarchy, chunking, cognitive load]
- **Accessibility:** [touch target and zoom/reflow implications]
- **Research:** [domain-specific layout precedent]

## Component Styling
- **Decision:** [roundness, elevation approach, signature look rules]
- **Rationale:** [why these component styles]
- **Source:** [aesthetic direction, competitor analysis, user preference]
- **User Context:** [trust level and tone]
- **Principle:** [affordance clarity, consistency]
- **Accessibility:** [state visibility and focus behavior]
- **Research:** [competitor pattern or external source]

## Substitutions (to replace)
- [List of every substitution flag from the context packet]
```

Agent G should generate this file alongside README.md and SKILL.md.

---

## Project-Level SKILL.md Template

```markdown
---
name: [brand]-design
description: "Generate branded interfaces and assets for [Brand]. WHEN: \"build with [Brand] design\", \"use [Brand] design system\", \"create [Brand] UI\", \"apply [Brand] tokens\"."
user-invocable: true
---

Read the README.md file within this skill, and explore the other available files.

If creating visual artifacts, copy assets out and create static HTML files.
If working on production code, copy assets and read the rules here.

If the user invokes this skill without guidance, ask what they want to build.

## Quick reference
- **Colors, type tokens:** [CSS file path]
- **Logos:** [Asset paths]
- **Icons:** [Icon library + CDN]
- **Fonts:** [Font names + loading method]
- **UI kit:** [Kit path]

## Signature look (do not break)
[3-5 bullet rules. The visual identity constraints.]
```

