# Skill Architecture Analysis

Self-critique of this skill's design: what's reusable knowledge vs. example-specific
patterns, identified gaps, and safeguards against overfitting to any single aesthetic.

---

## 1. Reusable Architectural Patterns

These patterns are generic and correct for virtually any design system.

### Token Architecture
- **Raw -> Semantic separation**: Raw palette (`--ink`, `--flame`) mapped to semantic roles (`--bg`, `--accent`). The indirection layer enables theming without touching component code.
- **Three-tier font stack**: display / body / mono. Holds for every product.
- **4px base grid**: Industry standard (Material uses 4/8px, Carbon uses 8px).
- **Modular type scale with explicit tokens**: Named sizes (display through label) with explicit line-height, weight, and letter-spacing per level. Better than "just pick a size."
- **Motion tokens**: Named easing + duration tiers (fast/base/slow). Prevents ad hoc animation values.
- **Shadow tiers**: none -> sm -> md -> lg with an accent shadow variant. Clean elevation system.
- **Semantic type classes** (.t-display, .t-h1, etc.): Reusable pattern for any system.
- **Focus-visible ring with accent color**: Accessibility-first interaction pattern.

### Design System Dashboard
- **Single browseable overview page**: Topbar + sidebar + main content. Makes the system inspectable by humans.
- **Token swatches with name + CSS var + hex**: Visual proof, not just documentation.
- **Typography specimens using representative content**: Shows type scale in realistic context.
- **Spacing visualization bars**: Physical representation of the spacing scale.
- **Component preview via iframes**: Isolated component views.
- **Scrollspy navigation**: Active sidebar state tracking scroll position.

### Content System
- **Voice definition** (direct, technical, warm): Every brand needs a voice definition.
- **Good/bad copy examples**: Concrete examples prevent misinterpretation.
- **Button microcopy patterns**: Actionable, not abstract.
- **Length constraints** (H1: 5-9 words, subhead: 1 clause): Prevents copy bloat.
- **Explicit anti-patterns**: Guardrails specific to the brand.
- **Substitution/certainty notes**: Flagging inferred vs. provided assets.

### Skill File
- **Quick reference block**: Fast lookup for the most-needed values.
- **"Signature look" immutable rules**: The 3-5 visual rules that define brand recognition.
- **Dual output mode** (artifacts vs. production code): Context-aware generation.

### Parallel Agent Architecture
- **Context packet as sole serialization**: Clean handoff between interview and generation.
- **Dependency graph with clear phases**: Sequential -> parallel -> sequential assembly.
- **Merge strategy with conflict resolution**: Token consistency across agents.
- **Evaluation agent gates delivery**: Quality check before assembly.

---

## 2. Example-Specific Patterns (do NOT treat as universal defaults)

These are decisions from the Bitrails reference benchmark, not universal rules:

| Pattern | Why it's example-specific |
|---|---|
| Single accent color model | Many brands need multi-accent palettes (primary, secondary, category) |
| Dark header + white wordmark as "signature" | Not every brand leads with dark chrome |
| Alternating ink/paper section rhythm | Some products are all-light or all-dark |
| Specific font pairings (Space Grotesk + Inter) | Font selection is the most brand-specific decision |
| "No parallax, no scroll-jacking" | Gaming and entertainment benefit from dramatic scroll |
| Warm gray (#F5F2EE) instead of cool gray | Color temperature is a brand choice |
| Sentence case everywhere | Some brands use title case or all-caps headings |
| "Declarative over promotional" tone | Correct for a studio, wrong for children's apps |
| Lucide icons specifically | Icon set choice depends on product context |
| 1280px max container | Depends on content density and device mix |
| "No gradients by default" | Many legitimate design systems use gradients |
| Flat/bordered elevation model | Many products need layered shadow elevation |

---

## 3. Risks of Overfitting

### Risk: Warm aesthetic becomes the default
The reference example is warm and restrained. If the skill internalizes this as "good design," it will produce warm-gray-on-charcoal for every project. **Mitigation**: Aesthetic direction is always a user input. The 14 presets in `aesthetic_directions.md` and the synthesis engine in `aesthetic_synthesis.md` enforce variety.

### Risk: Marketing website becomes the assumed output
If the skill optimizes for landing pages, it underperforms for dashboards, documentation, or app screens. **Mitigation**: Multiple surface types are supported with distinct layout patterns, content density guidance, and dedicated UI kit templates.

### Risk: Single-accent color model assumed
Many real systems need 2-3 accent colors (primary action, secondary action, informational) plus category colors. **Mitigation**: Token schema supports multi-accent palettes. The accent derivation in `generation_flow.md` handles primary and secondary accents.

### Risk: "Flat, no shadow" becomes default elevation
Some products need layered elevation (Gmail, Notion, dashboards). **Mitigation**: Elevation system is a choice derived from aesthetic direction, not a default.

### Risk: Token naming locked to example vocabulary
"Ink," "Paper," "Flame" are evocative example names. **Mitigation**: Token schema uses generic semantic names (bg, fg, accent, surface) as the base layer, with brand-specific aliases as optional overlay.

### Risk: Content rules assumed for all audiences
"No emoji" and "declarative tone" are correct for some brands but wrong for consumer products, education, or creative tools. **Mitigation**: Content rules derive from the interview's voice/tone dimension.

---

## 4. Structural Safeguards

- **SKILL.md stays under 200 lines**: It routes to reference files, never contains deep content.
- **Progressive disclosure**: Read foundational files only when needed for the current task.
- **Separation of concerns**: Usability knowledge never mixes with aesthetic preferences.
- **Brand-agnostic knowledge base**: All reference files avoid hardcoded aesthetic defaults.
- **Decision helpers, not decisions**: When the user is unsure, the skill offers tradeoffs.
- **Substitution flags**: Every inferred value is marked, with guidance to replace with real assets.

---

## 5. Quality Checks for This Skill Itself

When updating this skill, verify:
- [ ] No new file introduces a hardcoded aesthetic default
- [ ] No new foundation file contradicts an existing one
- [ ] The interview framework can still reach a complete synthesis
- [ ] The agent decomposition covers all new output types
- [ ] The evaluation checklist covers all quality dimensions
- [ ] File count stays manageable (currently 64 files across 6 directories)
- [ ] All token names in foundation files exist in the canonical `token_schema.md` dictionary
- [ ] Dark-mode CSS in component files uses `[data-theme="dark"]` selector, not `@media (prefers-color-scheme: dark)`
- [ ] New packet schema fields are reflected in `packet_slicing.md`, `packet_validation.md`, and the evaluation checklist
- [ ] Compact packet example in `packet_slicing.md` includes all mandatory fields

---

## 6. Cross-File Consistency Risks

These risks are structural — they exist because multiple files define overlapping concerns.

### Risk: Token naming drift across foundation and schema files
Foundation component files define CSS with specific token names (`--dur-normal`, `--bg-elevated`). The canonical dictionary in `token_schema.md` may use different names (`--dur-base`, `--bg-alt`). When either file changes, the other must be updated in sync.
**Structural enforcement:** `validation/css_validation.md` runs regex checks against generated output. `schemas/packet_validation.md` checks the packet. Neither checks the foundation source files themselves. Add a grep sweep to the quality checks: `rg 'var\(--[a-z-]+\)' foundations/` should not produce any token names absent from `token_schema.md`.

### Risk: Packet schema fields not reflected in slicing/validation/evaluation
When a new field is added to `packet_schema.md` (e.g., `spatial_logic`), it must be added to: (1) the slicing table in `packet_slicing.md`, (2) the mandatory fields list, (3) the validation checklist in `packet_validation.md`, and (4) the evaluation checklist. Missing any one means the field exists but is not enforced.
**Structural enforcement:** Each new field addition to `packet_schema.md` should be a single commit that touches all four files. The quality check list above includes this verification.

### Risk: Theming schema uses non-canonical raw token names
`theming_schema.md` maps semantic tokens to raw palette tokens. If the raw token names differ from `token_schema.md`'s canonical names (e.g., `--accent-raw` vs `--accent-500`), generated output will be inconsistent.
**Structural enforcement:** `theming_schema.md` must use the exact raw palette names from `token_schema.md`'s Canonical Token Dictionary. Verify on every theming_schema update.

### Risk: Compact packet format diverges from full format
The compact packet example in `packet_slicing.md` is maintained manually. If fields are added to the full schema but the compact example is not updated, agents receiving compact packets will miss new mandatory fields.
**Structural enforcement:** The compact packet transformation algorithm in `packet_slicing.md` defines which fields are preserved, compressed, or removed. Follow it mechanically — never hand-edit the compact example independently.

---

## 7. Creative Enforcement Gaps

These risks exist because creative mechanisms are defined but not structurally enforced.

### Risk: spatial_logic is defined but agents ignore it
`spatial_logic` (grid_discipline, whitespace_attitude, alignment) is defined in the packet schema and should produce visible structural differences. But if agents treat it as optional prose, all presets produce identical layouts.
**Structural enforcement:** The evaluation checklist includes a check: "Spatial logic is reflected in layout." If this check fails, the output should be rejected.

### Risk: creative_brief has no traceable CSS consequences
The `creative_brief.statement` is the primary creative control, but there is no mechanism to verify it actually influenced specific CSS decisions.
**Structural enforcement:** Add a `creative_trace` verification step to the evaluation: "Cite at least 2 CSS decisions that deviate from safe defaults specifically because of the creative_brief statement."

### Risk: Anti-slop rules produce second-order blandness
Banning Inter, purple gradients, and centered layouts pushes agents toward the second-most-common patterns (Space Grotesk + teal accent + flat cards + asymmetric hero). The Velocity example packet is literally this pattern.
**Structural enforcement:** Add "Positive Differentiation Guidance" to `ai_slop_detection.md`: affirmative creative moves to try, not just patterns to avoid. Add a "Second-Order Slop" section listing the patterns that emerge when first-order slop is avoided.

---

*Last audited: 2026-05-02. Re-audit when any file in schemas/ or theming_schema.md changes.*
