# Component Patterns Reference

> Each component has been split into its own file for targeted agent reading.
> See `foundations/components/`.

---

## Global Component Rules

- `foundations/components/` is canonical for component CSS, HTML, JS, and
  accessibility. If another foundation file conflicts, the component file wins.
- Use CSS logical properties by default: `padding-inline`, `margin-block`,
  `inset-inline-start`, `border-inline-start`, and `text-align: start`.
- Physical properties (`left`, `right`, `margin-left`, `padding-right`) are
  allowed only for geometry that is not language-directional, such as centered
  transforms or canvas/SVG positioning.
- Apply `components.character_rules` from the context packet before generic
  defaults. Component character is part of the brand, not polish.
- Apply `creative_brief` and `creative_brief.risk_dial` before choosing the final
  CSS treatment. `safe` keeps canonical defaults, `elevated` allows one visible
  brand move per component, `bold` allows stronger asymmetry or non-standard
  borders, and `experimental` may use advanced CSS with fallbacks.
- Apply `components.component_style_contract` exactly for shared components so
  buttons, cards, inputs, navigation, and sections match across dashboard,
  previews, and UI kits.
- Apply `signature_dna` or `signature_moment.systemic_effects` to at least three
  component categories. Signature behavior must reach ordinary UI states such as
  focus rings, active nav, card hover, table selection, chart focus, or dividers.
- `micro_interactions.md` describes behavioral patterns only; it should not
  override canonical component implementations.

## Brand Expression (risk_dial to CSS Mapping)

Every component must reflect the `creative_brief.risk_dial` value in its CSS
treatment. This is not optional polish — it is the primary mechanism by which
creative intent reaches the rendered interface.

### risk_dial Values and CSS Consequences

| risk_dial | Padding | Borders | Hover States | Layout |
|-----------|---------|---------|--------------|--------|
| `safe` | Standard symmetric padding from spacing scale | Subtle 1px borders in `--border` color; no decorative borders | Gentle: opacity shift, subtle background tint | Standard centered containers; no grid breaks |
| `elevated` | Slight asymmetry allowed (e.g., `padding-block: var(--space-5); padding-inline: var(--space-6)`) | Refined treatments: dual-tone borders, accent top-bar on featured elements | Considered transitions: 200ms multi-property (background + border-color + shadow) | Standard grid with one asymmetric section allowed |
| `bold` | Asymmetric padding encouraged (e.g., `padding: var(--space-6) var(--space-8)`) | Non-standard borders: mixed widths, accent segment borders, selective border removal | Animated: 200-300ms with transform (translateY, scale) + shadow elevation + color shift | Asymmetric grids in hero/proof sections; 7/5 or 8/4 splits |
| `experimental` | Intentionally uneven padding; variable inset per component state | Mixed border styles: dashed + solid segments, gradient borders, border-image | Complex multi-state animations: staggered property changes, clip-path reveals, backdrop-filter transitions | Broken grids; overlapping elements; non-rectangular containers |

### Applying Brand Expression

1. Read `creative_brief.risk_dial` from the context packet.
2. Read `creative_brief.statement` and `art_direction_keywords` for qualitative tone.
3. Apply the corresponding CSS column from the table above as baseline.
4. Override per-component with `components.character_rules` entries, which are
   more specific than the risk_dial defaults.
5. Propagate `signature_dna` into at least 3 component categories using the
   propagation rules from the context packet.

When `character_rules` and `risk_dial` conflict, `character_rules` wins for
that specific component; `risk_dial` governs components without specific rules.

---

## Minimum Component Spec

Every component file should provide:
- Semantic HTML structure and ARIA requirements
- CSS for default, hover/focus/active/disabled/error states where applicable
- At least three variants or sizes when the component naturally has variants
- Dark-mode and responsive notes
- Brand Expression hook mapping `creative_brief`, `character_rules`, risk level,
  and signature DNA into concrete CSS choices

---

## Component Files

| File | Description |
|------|-------------|
| `buttons.md` | Primary, secondary, ghost, and destructive button variants with size scales and accessibility rules |
| `cards.md` | Standard, elevated, interactive, feature, and pricing card variants with internal layout patterns |
| `forms.md` | Text inputs, labels, helper/error text, form groups, validation UX, and multi-step form patterns |
| `navigation.md` | Sticky top navigation bar with active indicators and mobile hamburger menu requirements |
| `badges.md` | Inline status badges (accent, success, error) with tinted backgrounds and label typography |
| `tables.md` | Data table with header styling, row hover states, and column alignment conventions |
| `modals.md` | Dialog overlay with backdrop blur, focus trap, and Escape-to-dismiss accessibility |
| `section_layouts.md` | Page-level section compositions: hero, feature grid, pricing table, and testimonials |
| `animations.md` | Page-load reveals, hover lifts, underline grows, and staggered entrance patterns |
| `dropdowns.md` | Custom select/combobox with full ARIA roles, keyboard navigation, and click-outside dismiss |
| `tabs.md` | Tab list and panel pattern with WAI-ARIA Tabs keyboard behavior and focus management |
| `accordion.md` | Expandable sections with CSS grid-row animation, single/multiple mode, and chevron rotation |
| `tooltip.md` | Positioned tooltip with delay, placement variants, arrow caret, and Escape dismiss |
| `toast.md` | Snackbar notifications with auto-dismiss timer, stacking, variant accent bars, and aria-live |
| `toggle.md` | Binary switch with native checkbox backing, loading pulse, and async state handling |
| `pagination.md` | Page navigation with ellipsis logic, prev/next arrows, and aria-current on active page |
| `file_upload.md` | Drag-and-drop upload zone with file validation, per-file progress bars, and remove buttons |
| `date_picker.md` | Calendar popup with month navigation, full grid keyboard control, and locale-aware formatting |
| `breadcrumbs.md` | Hierarchical path navigation with ordered list, aria-current, and mobile truncation |
| `compositions.md` | Composite patterns (Search Bar, Data Table, Form Section, Card Grid, Settings Panel) wiring atomic components together with data flow descriptions |

## Component Fallback Rules

When `components.character_rules` does not cover a specific component type, derive
its CSS treatment from these defaults in priority order:

1. **Nearest named category.** If `buttons` is defined but `badges` is not, badges
   inherit button border/radius logic with smaller padding.
2. **risk_dial baseline.** Apply the corresponding risk_dial column from the
   Brand Expression table above as the default treatment.
3. **Signature DNA propagation.** If `signature_dna` names the component's
   category, apply the propagation rule before generic defaults.

When a component is not covered by character_rules, signature_dna, or risk_dial,
use: 1px `--border` border, `--radius-md`, standard spacing, `--bg` surface.

Never invent novel component treatments for components not in the packet without
falling back through these three priority levels first.

## Known Expansion Targets

The current component library covers the common core but is not exhaustive.
For full application systems, add or synthesize these before claiming complete
coverage: avatar, command palette, stepper/wizard, divider/separator,
drawer/sheet, radio group, checkbox group, slider/range input, and popover.
