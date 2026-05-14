# Generation Flow

How to translate interview answers into design tokens, documentation, and deliverables.
This is the synthesis and output pipeline.

---

## Step 0: Research Inputs (Required)

Before token derivation, confirm synthesis includes:
1. Interview outcomes (user context, constraints, surfaces)
2. Competitor/reference scan summary (or justified skip in quick mode)
3. Accessibility and UX constraints (WCAG target + interaction requirements)
4. At least one inspiration or adjacent-category reference when the system aims for strong visual distinction
5. Primary language, secondary languages, script system, and reading direction when
   typography, cultural fit, or multilingual support matters

Token and component decisions should reconcile all three inputs, not aesthetics alone.

For non-trivial systems, research is not only defensive. It should actively enrich the
output with:
- Domain conventions worth preserving
- Adjacent-category ideas worth borrowing
- One differentiation gap the design system can exploit

---

## Step 0.5: Creative Divergence (Required Before Locking Direction)

Before committing to one token system, create 2-3 micro-directions. Keep this brief and
comparative rather than fully generated.

Each micro-direction should vary at least three of these levers:
1. Display/body font pairing
2. Neutral temperature
3. Accent strategy
4. Radius/elevation attitude
5. Layout rhythm or composition pattern
6. Signature moment

For each option, write:
- A one-line creative thesis
- A philosophy statement: "This system believes X and refuses to Y"
- Why it fits the product and audience
- What makes it different from the competitors/references
- What visual risk it introduces

### Token Budget Limits

To prevent context overflow in agent prompts, enforce these word limits on micro-directions:
- **Max 120 words per micro-direction** — Keep each option concise
- **Max 400 words total** — All micro-directions combined must not exceed 400 words

If directions exceed these limits, trim the less-essential fields (risk, competitor differentiation) before proceeding. The thesis, philosophy, and fit are mandatory; risk and differentiation can be shortened or omitted if needed.

Then select one direction or synthesize a hybrid. Do not skip this step for premium,
brand-forward, or marketing-heavy systems unless the user supplied an explicit existing brand.

Encode the result in the context packet as:
- `creative_brief`: 3-5 evocative sentences that all agents must preserve.
- `aesthetic.secondary_influences`: 1-2 useful traits from runner-up directions.
- `tension_points`: deliberate scale, density, or grid contrasts.
- `components.character_rules`: how the visual idea changes buttons, cards, inputs, and navigation.

### Tension Point → CSS Pattern Mapping

| Tension type | What it means | CSS consequence |
|---|---|---|
| `scale` | Oversized vs. undersized elements in deliberate contrast | Display font 5-8× larger than body; micro-labels at --fs-label |
| `weight` | Bold structural elements vs. hairline details | --fw-bold for headlines; --fw-regular + generous tracking for captions |
| `density` | Tightly packed data vs. airy editorial breathing room | --space-2 row padding in tables; --space-9 section padding in hero |
| `color_temperature` | Warm accent against cool neutral ground | Warm accent (#FF/amber family) + cool neutral (Ink/Smoke family) |
| `texture` | Rough/organic vs. smooth/technical | CSS background-image patterns vs. flat fills |
| `geometry` | Circular/organic shapes vs. hard rectangles | --radius-full on avatars/badges; --radius-sm on data cells |

### Tension Point Implementation Requirements

Every non-empty `tension_points` entry must be translated into
`tension_points.implementation` before dispatch:

```yaml
tension_points:
  scale: "hero headline dwarfs metadata"
  density: "spacious hero followed by compact proof table"
  structure: "hero breaks grid, product surfaces use strict grid"
  implementation:
    scale_css: "--fs-display is 5-7x --fs-label; metadata stays --fs-label with uppercase tracking"
    density_css: "hero padding uses --space-10; table rows use --space-2 block padding"
    structure_css: "hero uses 10-col asymmetric grid; all following content uses 12-col centered grid"
```

Do not leave tension as poetry. Agents must receive exact CSS consequences:
specific type ratios, spacing token assignments, section padding ranges, grid
templates, or state behaviors.

### Tension Point CSS Patterns (Concrete Reference)

These are the specific CSS patterns agents should emit for each tension type:

#### Scale Tension

- Hero headline: 4-6x body size (e.g., `font-size: clamp(3rem, 8vw, 6rem)`)
- Metadata/labels: 0.75x body size (e.g., `font-size: calc(var(--fs-body) * 0.75)`)
- Size ratio between display and label must be at least 5:1 for dramatic tension, 3:1 for subtle tension
- Token mapping: `--fs-display` is derived from body size * scale_ratio^4; `--fs-label` is body size * scale_ratio^-1

#### Density Tension

- Spacious sections (hero, CTA): 2x base section padding (`padding-block: calc(var(--space-8) * 2)`)
- Standard sections (features, testimonials): 1x base section padding (`padding-block: var(--space-8)`)
- Compact sections (metrics, data tables): 0.75x base section padding (`padding-block: calc(var(--space-8) * 0.75)`)
- Token mapping: section padding uses `--space-8` / `--space-10` for spacious, `--space-6` for compact
- Intra-section spacing follows the same multiplier: generous gap in spacious, tight gap in compact

#### Structure Tension

- Hero section: asymmetric grid (`grid-template-columns: 7fr 5fr` or `8fr 4fr`)
- Content sections: standard 12-column centered grid (`max-width: var(--container-max); margin-inline: auto`)
- Proof/data sections: full-bleed or edge-to-edge treatment
- The grid break must be documented in `tension_points.structure` and limited to the named section
- At `safe` risk_dial, structure tension is limited to a 2-column asymmetric split (no broken grids)
- At `bold` or `experimental`, overlapping elements or non-rectangular containers are allowed

### risk_dial → Generation Behavior

The `risk_dial` field in `creative_brief` controls how much the generation departs from safe defaults.

| risk_dial | Behavior |
|---|---|
| `safe` | All tokens reference established scale values. No novel combinations. Signature moment uses standard CSS properties only. |
| `elevated` | One experimental pattern allowed: gradient borders, layered shadows, or mix-blend-mode hover effect. Must still pass contrast. |
| `bold` | Two novel patterns. Signature moment may use clip-path, CSS masks, or unconventional layout (e.g. overlapping elements). |
| `experimental` | Full creative latitude. Unusual properties encouraged. Evaluate for browser support. Document any properties requiring fallback. |

### risk_dial_permissions — CSS Property Mappings

Each risk level controls which CSS properties are permitted beyond safe defaults.
Include this as a `risk_dial_permissions` field in the context packet:

| CSS Property Category | `safe` | `elevated` | `bold` | `experimental` |
|---|---|---|---|---|
| `border-radius` (non-standard) | Standard scale only | One custom radius allowed | Multiple custom radii | Freeform |
| `clip-path` | Not allowed | Not allowed | Allowed | Allowed |
| `mix-blend-mode` | Not allowed | One usage allowed | Multiple usages | Freeform |
| `backdrop-filter` | Not allowed | Not allowed | Allowed | Allowed |
| `background-image` (gradients/patterns) | Solid colors only | Subtle gradients | Complex gradients, patterns | Any |
| `transform` (non-standard) | `translateY(-1px)` hover only | `scale(1.02)` hover | `rotate`, `skew` allowed | Any transform |
| `box-shadow` (colored/multiple) | Standard shadow scale | One colored shadow | Multiple shadows, colored | Any shadow |
| `filter` (blur, brightness) | Not allowed | Not allowed | `brightness(1.1)` hover | Any filter |
| `position` (overlapping) | Normal flow only | `relative` offset | `absolute` overlap | `sticky` layering |
| CSS Grid (non-standard) | 12-col standard | Asymmetric splits | Broken grids, overlaps | Named areas, subgrid experiments |
| `writing-mode` / `text-orientation` | Horizontal only | Horizontal only | Vertical labels allowed | Any |
| `animation` (keyframe) | Transitions only | One keyframe animation | Multiple keyframes | Any animation |

> **Default:** `elevated` when `creative_brief.risk_dial` is not specified. Agents must
> treat this as an explicit `elevated` value, not as "no constraint." See the Default
> Fallbacks table in `schemas/agent_context_packet.md`.

Agents should not receive raw preset tables as permission to reuse defaults verbatim.
They receive resolved token values plus explicit deviations from the origin direction.

### Rule-Breaking Budget

Consistency creates trust, but one or two intentional exceptions can create memory.
Before finalizing the packet, decide whether the system needs a rule-breaking budget:

```yaml
rule_breaking_budget:
  allowed:
    - "Hero headline may exceed --fs-display by 12% on desktop only"
    - "One proof-section divider may use a non-neutral accent texture"
  forbidden:
    - "Do not break form, table, focus, contrast, or touch-target rules"
  rationale: "The brand needs one memorable editorial gesture without weakening daily-use surfaces."
```

Allowed breaks must be rare, documented, and inaccessible-state-safe. Never use
rule-breaking to justify unreadable text, broken contrast, non-keyboard controls,
or layout overflow.

---

## Step 1: Token Derivation

### From aesthetic origin to tokens

Once the aesthetic direction is chosen (either from `references/aesthetic_directions.md`
or custom), derive the full token system:

#### Color tokens
1. Start with the chosen aesthetic origin and color narrative
2. Adjust accent color if the user specified a brand color
3. Select a named neutral family, not just warm/cool/neutral:
   - Sandstone: mineral beige, terracotta dust, desert editorial warmth
   - Concrete: industrial grey, slightly blue-green, precise and architectural
   - Ink: near-black blue/violet undertone, dark-first and premium
   - Moss: green-brown neutrals for grounded organic systems
   - Smoke: cool desaturated grey for technical or clinical systems
   - Brass: muted gold-brown neutrals for luxury or institutional systems
   - Volcanic: near-black with red-charcoal undertone, high-drama and energy
   - Alpine: crisp cool blue-grey, clean mountain light, precision without coldness
   - Clay: terracotta-warmed midtone, tactile and handcrafted, saturated warmth
   - Chalk: high-key near-white with a barely-there warm yellow undertone, editorial softness

### Neutral Family Selection Matrix

| If the brand is... | Prefer | Avoid |
|---|---|---|
| Warm + institutional | Brass | Smoke if it feels clinical |
| Warm + organic/craft | Sandstone, Moss, or Clay | Concrete unless the brand needs restraint |
| Cool + technical | Smoke, Concrete, or Alpine | Brass, unless the brand is premium/institutional |
| Dark-first, premium, technical, or dramatic | Ink or Volcanic | Pure black |
| Healthcare/accessibility-first | Smoke or Alpine | Ambiguous greens unless explicitly branded |
| Luxury/editorial | Ink, Brass, or Chalk | Flat neutral gray |
| High-energy / consumer / action | Volcanic | Sandstone (too soft) |
| Light-mode only / editorial / high-clarity | Chalk or Alpine | Ink |
| Uncertain | Start from aesthetic origin default, then adjust by audience/trust | Arbitrary gray |

### Custom Neutral Family — OKLCH Recipe

When no named family matches, generate a custom neutral using OKLCH:

```
oklch(L C H)
```

1. **Set the hue (H).** Derive from the brand accent hue ± 180°, or from the
   industry color temperature. Examples:
   - Accent hue 210° (blue) → neutral hue 20–30° (warm complement)
   - Accent hue 145° (green) → neutral hue 320–340° (warm purple-grey)
   - Neutral desired cool → H 230–260°; warm → H 20–50°; neutral → H 0° (achromatic)

2. **Set the chroma (C).** Keep it low for a refined neutral:
   - Subtle warmth/coolness: C 0.005–0.012
   - Clearly tinted: C 0.015–0.025
   - Strongly tinted (uncommon): C 0.030+

3. **Generate the scale.** Nine steps from near-black to near-white:
   ```
   Step 900 (near-black):  L 0.12, C [chosen], H [chosen]
   Step 800:               L 0.20, C [chosen], H [chosen]
   Step 700:               L 0.30, C [chosen], H [chosen]
   Step 600:               L 0.40, C [chosen], H [chosen]
   Step 500 (mid):         L 0.52, C [chosen], H [chosen]
   Step 400:               L 0.64, C [chosen], H [chosen]
   Step 300:               L 0.74, C [chosen], H [chosen]
   Step 200:               L 0.84, C [chosen], H [chosen]
   Step 100 (near-white):  L 0.94, C [chosen * 0.5], H [chosen]
   Step 50 (base white):   L 0.98, C [chosen * 0.3], H [chosen]
   ```

4. **Convert to hex.** Browsers support `oklch()` natively, but generate hex
   fallbacks for agents that inline values into HTML attributes.

5. **Name the family** with an evocative single word, then add to `colors.raw_names`
   in the context packet. Document the OKLCH recipe in `decision_log`.

4. Generate the neutral scale (6-10 shades from near-black to near-white) from that family
5. Generate accent variants: base, hover (+10-15% lightness), press (-15-20% darker),
   tint (10-15% opacity on light surface), deep (30-40% darker for text on light)
6. Add semantic colors: success (#16A34A), warning (#F59E0B), error (#DC2626)
7. Verify contrast ratios for every fg/bg combination (WCAG AA: 4.5:1 normal, 3:1 large)

#### Typography tokens
1. Identify typography context:
   - primary language and script
   - secondary languages and fallback requirements
   - writing direction (`ltr`/`rtl`)
   - brand values the type must express (luxury, comic, friendly, beauty, authority, technical, craft, calm, speed)
   - page parts that need expressive vs quiet typography
2. Read `references/typography_selection.md` for non-English, multilingual,
   cultural, luxury, comic/playful, brand-forward, or typography-led systems.
3. Select display font from the preset, user preference, or script-native candidates.
   The display font must express a named brand value and fit the primary script.
4. Select body font that complements the display font while preserving script
   legibility at small sizes.
5. Select mono font if the product shows code, IDs, logs, data, or developer docs.
6. Assign fonts to page roles: brand mark, hero, section headings, body, navigation,
   buttons, labels, data, docs, captions, and code.
7. Choose a scale ratio: 1.125-1.2 (dense/data tools), 1.25 (comfortable/SaaS),
   1.333+ (dramatic/editorial), then adjust per script.
8. Generate the full type scale from base (16px by default; 17-18px when the script
   needs more vertical room).
9. Set line heights, letter spacing, and weights per level. Avoid Latin negative
   tracking rules for Arabic, CJK, Devanagari, Thai, Hebrew, or scripts where it
   harms readability.
10. Preserve at least one typographic tension point in the pairing (contrast in
    tone, width, serif/sans, script tradition, weight, rhythm, or role assignment).
11. Write `typography.font_rationale` explaining why each font fits the language,
    brand essence, page role, and legibility constraints.

#### Spacing tokens
1. Choose base unit (4px or 8px — recommend 4px for flexibility)
2. Generate the scale: 4, 8, 12, 16, 24, 32, 48, 64, 96, 128
3. Set container max-width (1200px dense → 1440px spacious)
4. Set nav height (60px minimal → 80px feature-rich)

> **Variable base unit for distinctive output:** While 4px and 8px are standard base
> units, specific aesthetics may benefit from non-standard bases (5px for softer
> editorial rhythm, 6px for grounded organic feel, 7px for luxury spacing). When using
> a non-standard base, document it in the context packet under `spacing.base_unit` and
> regenerate the scale. Ensure touch targets still meet the 44px minimum.

#### Radii tokens
1. Determine roundness direction from the aesthetic (sharp = 0-4px, moderate = 4-12px, round = 12-20px)
2. Generate: sm, md, lg, xl, full (999px)
3. Avoid uniform roundness unless the chosen direction explicitly calls for it

#### Shadow tokens
1. Determine elevation approach (shadow-based or flat/border-based)
2. Generate 4-5 levels from none to heavy
3. Reserve one stronger treatment for the system's signature moment

#### Motion tokens
1. Set the default easing curve
2. Set duration tokens: micro (100ms), fast (200ms), base (300ms), slow (450ms)

---

## Step 2: CSS Variable File

Generate `tokens.css` following `schemas/token_schema.md`:

1. Do not use CSS `@import` for fonts. Use `<link rel="preconnect">` and
   `<link rel="stylesheet">` in HTML outputs, or `@font-face` / bundled fonts
   when production constraints require local assets.
2. `:root` block with all tokens organized by section:
   - Color (raw palette, then semantic mapping)
   - Typography (families, sizes, line heights, spacing, weights)
   - Spacing
   - Radii
   - Shadows
   - Motion
   - Layout
3. Semantic type classes (`.t-display`, `.t-h1`, `.t-body`, etc.)
4. Base element styles (`html`, `body`, `a`, `::selection`, `:focus-visible`)
5. Reduced motion media query

---

## Step 2.5: Visual Sketch (User Confirmation)

Before proceeding to full system generation, create a lightweight visual sketch for user confirmation.

### Purpose

This phase validates visual direction before committing to full token system and component generation. The user reviews and confirms or rejects the aesthetic before Phase 3 dispatch.

### Deliverable

Generate a single lightweight HTML file (~200 lines) containing:

1. **Color swatches** — 4-6 color chips showing the proposed palette
2. **Type specimens** — Display font and body font samples at key sizes
3. **Component sketches** — 3 rough component mocks:
   - Button (primary + secondary)
   - Card (basic layout)
   - Input (focus state)
4. **One section layout** — Hero or feature section structure

### Constraints

- No dark mode
- No responsive breakpoints
- No full token system — just visual direction confirmation
- Single HTML file, inline styles only
- ~200 lines maximum

### Flow

1. Generate the visual sketch HTML file
2. Present to user for review
3. User either:
   - **Confirms** → Proceed to Phase 3
   - **Rejects** → Loop back to Phase 2 adjustments, then regenerate sketch

### When to Skip

Skip Phase 2.5 in quick mode or when the user provides complete brand guidelines (exact hex codes, font files, signed-off tokens).

---

## Step 3: System Documentation

Generate `README.md` following `schemas/output_schemas.md`:

1. System overview (name, version, fonts used, icon system)
2. Brand essence (if applicable)
3. Signature look (the 3-5 visual rules that define the system)
4. Content fundamentals (voice, tone, length rules, casing, example copy)
5. Visual foundations (color, typography, spacing, radii, shadows, borders, imagery, motion)
6. Interaction states (hover/press/focus per component type)
7. Iconography rules
8. Layout rules
9. Project index (file structure)
10. Caveats and substitution flags
11. Signature moment explanation (what it is, where it appears, why it matters)

---

## Step 4: Decision Traceability + Project-Level Skill

Generate both `DECISIONS.md` and a project-specific `SKILL.md`:

1. `DECISIONS.md` logs major decisions and rationale:
   - aesthetic choice and why
   - color temperature/accent choices and why
   - typography pairings, language/script fit, expressive roles, and scale ratio rationale
   - spacing density decisions
   - signature moment choice and why
   - substitutions and inferred values
2. `SKILL.md` includes:
   - Front matter (name, description, user-invocable: true)
   - Quick reference (key tokens, fonts, assets, icons)
   - Signature look rules (the "do not break" constraints)
   - Router to other project files

This file should be ~30-50 lines — lean and actionable.

---

## Step 5: Visual Preview Dashboard

Generate `design-system.html` — a single browsable page:

1. Sticky top bar with brand name and version
2. Sidebar navigation organized by category
3. Main content with sections:
   - Overview (system metadata)
   - Color (token cards with swatches)
   - Typography (live type specimens)
   - Spacing (visual scale)
   - Radii (visual examples)
   - Shadows (elevation demos)
   - Iconography (icon grid with usage)
   - Components (buttons, cards, forms, badges)
   - Logo lockups (if applicable)
   - UI kits (links to prototypes)
4. Scrollspy for sidebar navigation
5. All tokens rendered from the CSS variables file

---

## Step 6: Component Previews

Generate individual HTML files in `preview/`:
- Each file is self-contained (includes the CSS variables)
- Focused on one component or foundation element
- Shows all states and variants
- Can be opened standalone in a browser

Standard previews: colors, type, spacing, radii, shadows, buttons, form-inputs,
cards, badges, nav, logo-lockups, iconography.

---

## Step 7: UI Kits

For each surface type (marketing site, dashboard, docs, etc.), generate:
1. A `README.md` explaining the kit
2. An `index.html` interactive click-through prototype
3. Individual component files (JSX or HTML)
4. A primitives file (Button, Input, Badge, Card base components)

Marketing site kits typically include: Nav, Hero, FeatureCards, Pricing,
Testimonials, ContactForm, Footer.

Every surface must include exactly one signature_anchor (the memorable moment)
plus signature_dna that propagates the anchor's visual idea into ordinary components.

Signature anchor examples:
- A hero composition with unusual spatial tension
- A distinctive framing or border treatment
- A purposeful motion motif
- A data-as-ornament pattern
- An art-directed image/texture treatment

The signature anchor must be brand-relevant and reusable, not decorative noise.
Signature DNA must propagate into at least 3 component categories. If the
signature anchor is "overlapping layers," cards, modals, and section dividers
should inherit controlled layering. If it is "calibrated grid glow," focus rings,
chart highlights, and selected nav states should carry the same visual DNA.

Apply `signature_dna` propagation rules from the context packet. Each rule names
a specific component category and the CSS property/value that carries the mark.

---

## Step 8: Substitution Flags

Throughout all generated files, mark inferred or substituted values:

```markdown
> **Substitution flag:** [Asset name] was not provided. [What was used instead].
> If the real [asset] exists, replace it and regenerate.
```

Common substitutions to flag:
- Fonts (when no brand fonts were provided)
- Icons (when no custom icon set was provided)
- Logo (when inferred from description)
- Imagery (placeholder or stock image references)
- Colors (when derived from aesthetic preset rather than brand guidelines)

---

## Step 9: Distinctiveness Verification (before Agent H evaluation)

Before dispatching Agent H, the orchestrator verifies:

1. **Signature anchor exists** — `signature_anchor` is non-empty and refers to a specific visual idea (not "clean and modern")
2. **DNA propagates** — `signature_dna` has ≥ 3 entries, each naming a specific component and how it carries the mark
3. **Typography has tension** — display font and body font are from different classification families (serif+sans, condensed+regular, etc.)
4. **Accent is not default** — accent color is not one of: #6366F1 (indigo-500), #3B82F6 (blue-500), #8B5CF6 (violet-500), #10B981 (green-500) unless user explicitly chose it
5. **Font is not Inter** — display font is not Inter unless user explicitly chose it

If checks 4 or 5 fail, regenerate with different accent/font before evaluation.

---

## Output Checklist

Before delivering, verify:
- [ ] All CSS variables are defined and referenced (no hardcoded values)
- [ ] Contrast ratios pass WCAG AA for all text/background combinations
- [ ] Reduced motion media query is included
- [ ] Touch targets meet 44x44px minimum
- [ ] Font loading follows the project strategy (`<link>`, bundled fonts, or system-fonts-only mode)
- [ ] Substitution flags are present for all inferred values
- [ ] README covers all sections from the schema
- [ ] DECISIONS.md captures all major design decisions and rationale
- [ ] One signature moment is explicitly defined and visible in the output
- [ ] At least one adjacent or external inspiration source influenced the system when distinctiveness was a goal
- [ ] Preview dashboard renders correctly
- [ ] No placeholder text left in deliverables
