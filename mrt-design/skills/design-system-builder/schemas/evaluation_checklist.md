# Evaluation Checklist

Quality gates for every design system deliverable. Run through this before
handing off any output to the user.

---

## Severity Classification

Every checkpoint has a severity that determines the evaluation outcome:

| Severity | Meaning | Impact on Delivery |
|----------|---------|-------------------|
| **Critical** | Blocks delivery. Violates accessibility laws, produces broken output, or creates unusable interfaces. | Must fix before delivery. Agent output is rejected. |
| **Major** | Significant quality issue. Degrades user experience or contradicts design system principles. | Should fix. Delivery possible but flagged for user review. |
| **Minor** | Polish issue. Nice to fix but not blocking. | Informational. Included in report but does not block delivery. |

### Delivery Threshold

- **PASS**: Zero Critical failures, ≤ 2 Major failures
- **PASS WITH WARNINGS**: Zero Critical failures, 3-5 Major failures
- **FAIL**: Any Critical failure OR 6+ Major failures

Agent H returns one of these three verdicts with the report.

---

## Accessibility

- [ ] [Critical] **Contrast ratios** — Every fg/bg combination passes WCAG AA:
  - Normal text (<24px normal / <19px bold): 4.5:1 minimum
  - Large text (>=24px normal or >=19px bold): 3:1 minimum
  - UI components and graphical objects: 3:1 minimum
  - Focus indicators: 3:1 against adjacent colors
  - **Future:** When APCA reaches W3C Rec, validate with APCA Lc values instead.
    APCA Lc ≥ 60 for body text, ≥ 45 for large text, ≥ 15 for UI graphics.
- [ ] [Critical] **Focus visible** — Every interactive element has a visible focus ring that meets 3:1 contrast
- [ ] [Critical] **Touch targets** — All interactive elements are at least 44x44px (48x48px preferred on mobile)
- [ ] [Critical] **Reduced motion** — `@media (prefers-reduced-motion: reduce)` is included and functional
- [ ] [Critical] **Semantic HTML** — Proper elements used (`<button>`, `<a>`, `<nav>`, `<main>`, headings in order)
- [ ] [Critical] **ARIA attributes** — Present where HTML semantics fall short (modals, disclosures, live regions)
- [ ] [Critical] **Color not sole indicator** — Semantic colors paired with icons, text, or patterns
- [ ] [Critical] **Keyboard navigation** — All interactive elements reachable via Tab, activatable via Enter/Space
- [ ] [Critical] **Form labels** — Every input has an associated label (not placeholder-only)
- [ ] [Critical] **Image alt text** — All `<img>` elements have descriptive `alt` attributes (empty alt only for decorative images with `role="presentation"`)
- [ ] [Critical] **Page title** — `<title>` element is present and descriptive
- [ ] [Major] **Heading hierarchy** — Headings follow a logical order without skipping levels (h1 > h2 > h3, no h1 > h3)

## Token Consistency

- [ ] [Critical] **No hardcoded values** — All colors, fonts, sizes, spacing reference CSS variables
- [ ] [Critical] **Raw -> semantic -> component** — Three-layer architecture maintained
- [ ] [Critical] **All tokens defined** — No missing variables (check every semantic token has a raw source)
- [ ] [Major] **Dark mode tokens** — If the system supports dark mode, all semantic tokens are remapped
- [ ] [Critical] **Token naming** — Consistent naming convention throughout (no mixed conventions)
- [ ] [Critical] **No phantom tokens** — The following token names are NOT defined in `schemas/token_schema.md` and must not appear in generated output:
  - `--fg-heading` (not a canonical token; headings use `--fg`)
  - `--semantic-error` / `--semantic-success` / `--semantic-warning` (use `--error`, `--success`, `--warning`)
  - `--fg-on-accent` (use `--on-accent`)
  - `--fg-disabled` (not defined in the canonical schema; use `--fg-muted` with opacity)

## Theme Completeness

- [ ] [Critical] **Light theme complete** — All semantic tokens defined in `:root`
- [ ] [Critical] **Dark theme** — If `theme_modes` includes "dark", all semantic tokens remapped in `[data-theme="dark"]`
- [ ] [Critical] **System preference fallback** — `@media (prefers-color-scheme: dark)` block present with `:root:not([data-theme])` selector
- [ ] [Critical] **Requested theme modes present** — All requested `theme_modes` (light/dark/high-contrast) must be present in tokens.css. If the context packet specifies a theme mode and the corresponding `[data-theme="..."]` block is missing, that is a Critical failure. Verify each requested mode has a complete remapping of all semantic color tokens.
- [ ] [Major] **High-contrast theme** — If `theme_modes` includes "high-contrast", complete `[data-theme="high-contrast"]` block with WCAG AAA contrast targets
- [ ] [Major] **Dark shadow values** — Dark theme uses elevated opacity shadows (sm: 0.25+, md: 0.35+, lg: 0.50+)
- [ ] [Critical] **No --brand-* tokens** — No `--brand-*` variable names in generated output; use `--accent-*` naming. Cross-reference: also checked in Validation Tool Integration below.

## Dark Mode Component Behavior

These checks go beyond token completeness to verify that components actually work in dark mode — not just that the tokens are defined.

- [ ] [Critical] **Hover states visible in dark mode** — All hover effects (border-color, background-color, shadow) use semantic tokens (`var(--accent-hover)`, `var(--bg-alt)`, etc.) not hardcoded light-mode colors. Hardcoded `#F3F4F6`-style fills on dark surfaces are invisible.
- [ ] [Critical] **Borders visible in dark mode** — Component borders use `var(--border)` or `var(--border-strong)` which are remapped in dark theme. Borders hardcoded with `rgba(0,0,0,0.1)` disappear on dark backgrounds.
- [ ] [Major] **Focus rings visible in dark mode** — Focus rings use `var(--focus-ring)` not hardcoded light values. Verify the focus ring is visible on `var(--bg)` dark surfaces.
- [ ] [Major] **Dark mode shadows elevated** — Shadows use `var(--shadow-color)` which is remapped to higher opacity in dark theme, or use `var(--shadow-sm/md/lg)` directly. Shadows with hardcoded `rgba(0,0,0,0.05)` are invisible on dark surfaces.
- [ ] [Major] **Images have dark mode treatment** — If `imagery.implementation.dark_mode_adjustment` is non-empty, apply the specified CSS filter to image elements in the dark theme block. Missing filter means images appear unmodified in dark mode (often too bright or oversaturated).
- [ ] [Major] **Skeleton / loading states in dark mode** — Skeleton loaders use `var(--bg-alt)` not white/light-grey. On dark surfaces, white skeletons are the most visible dark mode regression.
- [ ] [Minor] **Chart/data colors work in dark mode** — Data visualization accent colors that work on light backgrounds may not have sufficient contrast on dark surfaces. Verify each chart color meets 3:1 against `var(--bg)` in dark mode.
- [ ] [Minor] **Illustrations / SVG assets** — Any SVG illustrations using `fill: #000` or `fill: #FFF` directly should be checked for legibility in both modes.

## Brand Consistency

- [ ] [Major] **Signature look preserved** — The 3-5 "do not break" rules are followed everywhere
- [ ] [Major] **Accent used sparingly** — One primary CTA per viewport, not competing accent elements
- [ ] [Major] **Typography hierarchy** — Correct font families used at each level, no mismatched weights
- [ ] [Major] **Typography expresses brand values** — Font rationale names the values each font expresses (luxury, comic/playful, friendly, beauty/editorial, technical, authority, craft, calm, etc.) and the rendered system reflects them
- [ ] [Major] **Language/script fit** — Primary language, script system, reading direction, and script-specific metric choices are documented and respected
- [ ] [Major] **Page-part type roles** — Hero, headings, body, labels, data, docs, and code use appropriate expressive or quiet type roles rather than one generic font treatment everywhere
- [ ] [Minor] **Spacing scale** — All spacing uses token values, no arbitrary pixel values
- [ ] [Minor] **Radii consistency** — Same roundness language throughout (no mixed sharp/rounded)

## Content Quality

- [ ] [Major] **Voice-appropriate** — Copy matches the documented voice profile
- [ ] [Minor] **Length rules followed** — Headlines, body, buttons at documented lengths
- [ ] [Major] **Casing consistent** — Chosen casing style applied everywhere
- [ ] [Major] **No placeholder text** — No "Lorem ipsum", "TODO", or placeholder copy remains
- [ ] [Minor] **No emoji in professional copy** — Unless the brand explicitly allows it
- [ ] [Major] **Microcopy patterns** — Button labels, error messages, and helpers follow documented patterns

## Responsiveness

- [ ] [Critical] **Mobile-first** — CSS written for smallest screen, enhanced with min-width queries
- [ ] [Critical] **Breakpoints tested** — Layout works at 375px, 768px, 1024px, 1280px
- [ ] [Minor] **Typography scales** — Display/h1 sizes reduce 20-30% below md breakpoint
- [ ] [Critical] **Navigation collapses** — Mobile menu works below lg breakpoint
- [ ] [Critical] **Touch targets** — 48px minimum on mobile views
- [ ] [Minor] **Content reflow** — Text stays within ~72ch, images maintain aspect ratio
- [ ] [Critical] **No horizontal scroll** — Page doesn't scroll horizontally at any breakpoint

## Visual Preview

- [ ] [Major] **Preview dashboard renders** — design-system.html loads and displays correctly
- [ ] [Major] **All sections populated** — No empty sections in the preview
- [ ] [Minor] **Scrollspy works** — Sidebar navigation highlights current section
- [ ] [Minor] **Component previews live** — Hover, focus, and active states are interactive
- [ ] [Major] **Rendered visual check** — Verify no text overflow, broken hierarchy, or broken media at 375px, 768px, and 1280px widths (if screenshots available). Check for: text clipped by containers, overlapping elements, images that do not load or are wrong aspect ratio, and layout collapse at narrow widths.

## Performance

- [ ] [Major] **Font loading** — Font source is consistent with project strategy (`<link>` in HTML, bundled `@font-face`, or system-fonts-only mode; no CSS `@import`)
- [ ] [Major] **No unnecessary dependencies** — Only required CDNs included
- [ ] [Minor] **CSS variables over duplicates** — No repeated identical values
- [ ] [Minor] **Image references** — Placeholder images use lightweight sources or CSS patterns

## Validation Tool Integration

- [ ] [Major] **CSS structure valid** — Run regex checks from validation/css_validation.md (no empty values, three-layer structure, no !important in component layer)
- [ ] [Major] **Accessibility smoke test** — Checks from validation/accessibility_smoke_test.md pass (contrast, focus, keyboard navigation, 200% zoom)
- [ ] [Minor] **Performance budgets met** — File sizes and metrics within limits from validation/performance_budgets.md
- [ ] [Minor] **Visual regression baseline** — If iterating on an existing system, compare against previous output per validation/visual_regression.md
- [ ] [Critical] **No --brand-* token names** — grep confirms no `--brand-` variables in any generated file. See also the Brand Consistency check above.
- [ ] [Major] **Canonical motion tokens** — Only `--dur-micro`, `--dur-fast`, `--dur-base`, `--dur-slow` present; no non-canonical duration names. Exception: if the context packet includes `motion.overrides`, those overridden values are allowed.
- [ ] [Critical] **character_rules is structured** — JSON/YAML has `buttons`, `cards`, `inputs`, `navigation`, `sections` keys (not flat array)
- [ ] [Major] **typography language strategy is structured** — `typography.language_strategy`, `typography.expressive_roles`, and `typography.font_rationale` are present for full/medium builds

## Documentation

- [ ] [Major] **README complete** — All sections from the output schema are covered
- [ ] [Major] **DECISIONS complete** — Major visual/token decisions include ID, rationale, source, user context, principle, accessibility impact, and research evidence (or explicit `n/a`)
- [ ] [Major] **Substitution flags** — All inferred assets flagged with replacement instructions
- [ ] [Minor] **File index** — Project tree matches actual generated files
- [ ] [Minor] **Project SKILL.md** — Concise, actionable, references other files correctly

## Beauty & Memorability

- [ ] [Critical] **Signature moment exists** — At least one immediately recognizable visual idea is present and intentional
- [ ] [Major] **Signature moment is reusable** — The distinctive move appears in more than one appropriate place, not just one decorative flourish
- [ ] [Major] **Signature moment systemic reach** — Signature moment systemic effects must appear in at least 3 distinct component categories (e.g., navigation active state, card hover, focus ring, section divider). Fewer than 3 is a Major failure.
- [ ] [Major] **Typography has personality** — Display/body pairing creates tension or voice, not just utility
- [ ] [Major] **Typography is culturally and linguistically specific** — Non-English or multilingual systems do not use Latin-default font logic, line-height, tracking, or fallback stacks without justification
- [ ] [Major] **Composition has spatial drama** — At least one section breaks cookie-cutter symmetry where appropriate
- [ ] [Minor] **Emotional tone is legible** — The interface feels like the intended brand within 3 seconds of viewing it
- [ ] [Major] **Art-director gate** — The design has at least one positive, memorable choice a senior art director would defend: emotional palette, strong type tension, meaningful asymmetry, or intentional rule-breaking
- [ ] [Major] **Component character** — Buttons, cards, inputs, or navigation have brand-specific character rules, not only safe default rectangles
- [ ] [Major] **Imagery direction** — Marketing-heavy outputs define photo/illustration treatment, crop rules, and placeholder behavior
- [ ] [Critical] **character_rules coverage** — `character_rules` object has entries for ≥ 4 of the 5 required keys (buttons, cards, inputs, navigation, sections); each entry is a specific CSS-level rule, not a mood description. Count and verify: list each key found and confirm the count.
- [ ] [Major] **Signature DNA reaches the system** — Signature systemic effects appear in at least 3 distinct component categories (for example: nav active state, card hover, focus ring, section divider, chart focus, table selection)
- [ ] [Major] **Tension points are implemented** — Each non-empty `tension_points` field has a corresponding concrete CSS/layout behavior from `tension_points.implementation`
- [ ] [Major] **Rule-breaking budget is controlled** — Any token-breaking gesture appears only in `rule_breaking_budget.allowed` locations and never in forms, tables, focus states, contrast, or touch targets
- [ ] [Major] **Component style contract consistency** — Shared buttons, cards, inputs, navigation, and section rhythm match across `design-system.html`, `preview/*`, and `ui_kits/*`
- [ ] [Major] **Spatial logic is reflected in layout** — `spatial_logic.grid_discipline`, `whitespace_attitude`, and `alignment` produce visible structural differences between presets (not just token swaps). Verify grid templates, section spacing, and alignment patterns in at least 2 generated surfaces.

## Creative Field Validation

These checks verify that the creative fields from the context packet are
operational — meaning they produce specific, verifiable consequences in the
generated output, not just prose.

- [ ] [Major] **Component-specific character rules >= 3** — At least 3 of the 5 required component categories (buttons, cards, inputs, navigation, sections) have character rules that produce visible CSS consequences. Verify by searching generated CSS for the specific treatment described in each rule. If character_rules uses Format A (structured), each rule must have at least 2 of 4 sub-fields filled (border_treatment, padding_asymmetry, hover_behavior, elevation).
- [ ] [Major] **Token-level consequences from creative_brief >= 2** — The creative_brief (statement, philosophy, art_direction_keywords, risk_dial) must produce at least 2 specific token-level deviations from safe defaults. Verify by comparing generated token values against the default scale and confirming at least 2 values were modified because of the creative brief. Examples: risk_dial=elevated causing asymmetric button padding; art_direction_keywords=["precise"] causing tighter letter-spacing on labels.
- [ ] [Major] **Signature DNA propagation rules >= 2** — At least 2 ordinary-component propagation rules from signature_dna must be verifiable in the generated CSS. Each rule names a component category and a CSS property/value. Verify by searching the CSS output for each named property/value pattern. Example: if signature_dna says "focus rings use cyan glow", search for `box-shadow` or `outline` declarations containing the accent color on focus-visible selectors.
- [ ] [Major] **Negative constraint exists** — The context packet or generated documentation contains at least one explicit "never do X" constraint that limits the design system. Verify by searching for "never", "must not", "avoid", or "do not" in the generated README or DECISIONS.md. A system with only positive rules and no boundaries is not a system — it is a suggestion. Examples: "Never use glow on body text", "Never break form contrast rules for stylistic effect".
- [ ] [Major] **risk_dial produces visible CSS consequences** — The risk_dial value (safe/elevated/bold/experimental) must produce at least one deviation from standard defaults in the component CSS. Verify by comparing component CSS against the risk_dial-to-CSS mapping in `foundations/component_patterns.md`. At `safe`, symmetric padding and subtle borders are expected. At `bold`, asymmetric padding or non-standard border treatments must be visible.
- [ ] [Major] **Tension point CSS is concrete** — Each non-empty entry in tension_points.implementation must contain specific numeric values or token names, not just prose. Verify: scale_css must name a size ratio (e.g., "5x"), density_css must name a spacing token (e.g., "--space-8"), structure_css must name a grid template (e.g., "7fr 5fr").
- [ ] [Major] **Risk-dial compliance** — If `creative_brief.risk_dial` is 'bold' or 'experimental', verify at least 2 components break from safe defaults (e.g., unusual border treatment, asymmetric layout, non-standard color use). Safe/elevated dials are exempt.
- [ ] [Critical] **signature_dna propagation** — signature visual idea appears in ≥ 3 different component categories in generated CSS (verifiable by searching for the relevant CSS property/value pattern)
- [ ] [Critical] **Typography tension** — display and body fonts are from different classification families (serif ≠ sans, condensed ≠ regular, etc.)
- [ ] [Critical] **Non-default accent** — accent color is not #6366F1, #3B82F6, #8B5CF6, or #10B981 unless user explicitly chose it
- [ ] [Critical] **Non-default display font** — display font is not Inter unless user explicitly chose it

## Anti-Slop Check

- [ ] [Critical] **Distinctive aesthetic** — Output doesn't look like a generic Bootstrap template
- [ ] [Critical] **No default Inter headings** — Unless explicitly chosen by the user
- [ ] [Critical] **No purple gradients on white** — Unless explicitly chosen by the user
- [ ] [Major] **No centered-everything layout** — Unless the aesthetic demands it
- [ ] [Major] **Font pairing has tension** — Display and body fonts create contrast, not sameness
- [ ] [Minor] **Visual hierarchy is obvious** — Squint test passes: primary content identifiable

---

## Quality Tier Classification

After all checklists pass, classify the output into a quality tier. This tier
determines delivery framing and whether a "beauty bonus" note is included.

The tier is determined by the presence of **affirmative creative achievements**,
not just the absence of failures. A system can pass all Critical/Major checks
and still be a Tier 1 (Foundation) output if it lacks positive distinction.

### Tier Criteria

**Tier 1 — Foundation**
Functionally correct. Meets all Critical requirements. No Critical failures.
May have Minor issues. Lacks positive creative distinction.

**Tier 2 — Professional**
Meets all Critical requirements + at least 4 of the following:
- [ ] Custom display font with documented personality rationale (not Inter/Roboto/system)
- [ ] Non-default accent color with brand-derived rationale
- [ ] Asymmetric hero or major section layout (not centered-column)
- [ ] Signature DNA verifiable in 3+ component categories
- [ ] Typographic tension (display + body from different classification families)
- [ ] Customized shadow treatment (brand-colored, ultra-flat, or dramatically elevated)
- [ ] Component character rules are CSS-specific, not mood descriptions

**Tier 3 — Exceptional**
Meets Professional requirements + at least 2 of the following "beauty bonus" criteria:
- [ ] **Irreplaceable signature** — The signature moment is derived from the product's
  function or user insight, not borrowed from a trend list. Passes the
  "brand name stripped" test (the visual element still communicates what the product does).
- [ ] **Coherent system tension** — At least one deliberate scale, density, or structural
  contrast from `tension_points` is concretely expressed in CSS and defensible on
  artistic grounds (not just described in prose).
- [ ] **Neutral family beyond safe grey** — The neutral palette carries a deliberate
  temperature and character (Volcanic, Chalk, Moss, etc.) that adds to the brand voice.
- [ ] **Surprising positive choice** — A rule was broken deliberately in one location,
  the break is documented in `rule_breaking_budget`, and the choice elevates the design
  without compromising usability.
- [ ] **Dense surface signature** — The signature DNA reaches a dashboard, table, or
  settings surface with an equally intentional treatment as the marketing surface.

### Reporting

Include in the evaluation report:

```
QUALITY TIER: [1 Foundation | 2 Professional | 3 Exceptional]
Beauty bonus criteria met: [list each criterion with PASS/FAIL]
Recommendation: [one sentence on the primary improvement opportunity]
```

A Tier 1 output should be flagged to the user for review before delivery.
A Tier 3 output may be highlighted to the user as a strong creative result.

---

## Automated Contrast Validation

### Method

The evaluation agent must programmatically verify color contrast using the WCAG relative luminance algorithm:

**Step 1:** Convert hex to sRGB (0-1 range): `sRGB = parseInt(hex, 16) / 255`

**Step 2:** Convert sRGB to linear RGB:
```
if sRGB <= 0.04045:
    linear = sRGB / 12.92
else:
    linear = ((sRGB + 0.055) / 1.055) ^ 2.4
```

**Step 3:** Calculate relative luminance: `L = 0.2126 * R + 0.7152 * G + 0.0722 * B`

**Step 4:** Calculate contrast ratio: `CR = (L_lighter + 0.05) / (L_darker + 0.05)`

### Required Checks

| Check | Elements | Minimum | Severity |
|-------|----------|---------|----------|
| Body text on background | --fg on --bg | 4.5:1 | Critical |
| Heading text on background | --fg on --bg (same token, different size) | 3:1 | Major |
| Muted/helper text on background | --fg-muted on --bg | 3:1 | Major |
| Accent color on background | --accent on --bg | 3:1 | Major |
| Button text on primary | --on-accent on --accent | 4.5:1 | Critical |
| Error text on background | --error on --bg | 4.5:1 | Critical |
| Success text on background | --success on --bg | 3:1 | Major |
| Warning text on background | --warning on --bg | 3:1 | Minor |
| Link text on background | --accent on --bg | 4.5:1 | Critical |
| Disabled text on background | --fg-muted on --bg | No minimum | N/A (exempt) |

### Reporting Format

For each check, report:
```
- [PASS/FAIL] <pair>: <calculated_ratio>:1 (required: <minimum>:1) [<severity>]
```

Example:
```
- [PASS] --fg on --bg: 8.23:1 (required: 4.5:1) [Critical]
- [FAIL] --accent on --bg: 2.1:1 (required: 3:1) [Major]
  → Fix: Adjust --accent from the current raw accent to an accessible tokenized variant (document exact value and ratio)
```

If a check fails, the evaluation agent MUST suggest a specific color value that passes. Never report a failure without a fix suggestion.

## Hardcoded Color Sweep

Evaluation must scan all generated files, not only CSS, for hardcoded color
values:

```bash
grep -RInE '#[0-9a-fA-F]{3,8}|rgba?\\(|hsla?\\(|oklch\\(' .
```

Allowed locations:
- Raw token definitions in `tokens.css`
- Documented swatch labels in preview/dashboard files
- Intentional SVG examples that reference existing raw token values

Flag any hardcoded color in component styles, inline styles, JavaScript strings,
or SVG fills/strokes when the value is not defined in the raw token layer.
