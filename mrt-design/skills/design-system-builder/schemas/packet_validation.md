# Packet Validation

Pre-dispatch checklist, fallback rules, cross-validation logic, and contrast validation.
This file was extracted from `agent_context_packet.md` for maintainability.

---

## Context Packet Validation

### Pre-Dispatch Checklist

Before any agent receives the packet, verify:

- [ ] `version` field present and matches expected version (`"1.1"`)
- [ ] `brand.name` is a non-empty string
- [ ] `colors.raw_palette.accent` is a valid hex color (#RRGGBB format)
- [ ] `colors.raw_palette` has at least 2 neutral stops (`neutral_dark` + `neutral_light`)
- [ ] `typography.families.display` and `typography.families.body` are non-empty font names
- [ ] `typography.language_strategy.primary_language` and `script_system` are present for full/medium builds
- [ ] `typography.font_rationale.display.expresses` names at least one brand value
- [ ] `typography.expressive_roles.hero`, `body`, `ui_label`, and `data` are filled when those surfaces exist
- [ ] `spacing.base_unit` is a positive number
- [ ] `density_system.spacing_multiplier` is the value assigned to CSS `--density-scale`
- [ ] `colors.raw_palette.info` and `info_tint` are present
- [ ] `components.character_rules` has at least 4 filled keys across buttons/cards/inputs/navigation/sections
- [ ] `components.component_style_contract` defines shared button/card/input/nav behavior for cross-agent consistency
- [ ] `components.component_style_contract` includes exact CSS values for button_primary, card_default, nav_active, and input_focus
- [ ] `tension_points.implementation` maps prose tension to concrete CSS consequences
- [ ] `signature_dna` or `signature_moment.systemic_effects` names at least 3 component categories
- [ ] `rule_breaking_budget.allowed` is explicit when the design needs intentional surprise
- [ ] `surfaces` array has at least one entry (if generating UI kits)
- [ ] `constraints.framework` is one of: html, react, vue, astro, svelte
- [ ] `constraints.theme_modes` is a non-empty array with at least "light"
- [ ] `constraints.default_mode` is set to one of the listed theme_modes
- [ ] Every mode in `constraints.theme_modes` has corresponding color overrides (Agent A must generate all)
- [ ] `global_context` is present with reading_direction, lang, locale, date_format, number_format, font_loading, and icon_strategy
- [ ] `spatial_logic` is present for full/medium builds with grid_discipline and whitespace_attitude filled

### Cross-Validation: character_rules vs component_style_contract

The `components.character_rules` (Format B string shorthand) and `components.component_style_contract` must not contradict each other. Before dispatch, verify:

1. **Radius consistency.** If `character_rules.buttons` mentions "pill" or "radius-full", then `component_style_contract.radius_language` must be `"pill"`. If `character_rules` says "sharp corners", `radius_language` must be `"sharp"`.
2. **Elevation consistency.** If `character_rules.cards` says "no shadow", then `component_style_contract.elevation_language` must be `"flat"`. If `character_rules` mentions "elevated" or "floating", `elevation_language` must match.
3. **Border consistency.** If `character_rules.inputs` says "underline-only", then `component_style_contract.border_style` must be `"underline"`. If "full border", then `"full"`.
4. **Density consistency.** `component_style_contract.density` must match `density_system.tier`.
5. **CSS value specificity.** Every sub-field in `component_style_contract` (button_primary, card_default, nav_active, input_focus) must use valid CSS values that resolve against the token system. Values referencing `var(--token)` must have corresponding tokens defined in `colors` or `spacing` sections.

If any cross-validation fails, the packet is invalid. Fix the contradiction before dispatch.

### Default Fallbacks

When a recommended field is missing, agents use these defaults:

| Field | Default | Notes |
|-------|---------|-------|
| colors.accent | Primary with +15deg hue rotation | Computed, not random |
| colors.semantic.success | #16A34A | Green-600 WCAG AA on white |
| colors.semantic.error | #DC2626 | Red-600 WCAG AA on white |
| colors.semantic.warning | #D97706 | Amber-600 WCAG AA on white |
| colors.semantic.info | #2563EB | Blue-600 WCAG AA on white |
| colors.raw_palette.info | #2563EB | Blue-600 -- fallback when not specified in palette |
| colors.raw_palette.info_tint | #EFF6FF | Blue-50 tint for info backgrounds |
| typography.mono | ui-monospace, "Cascadia Code", "Fira Code", monospace | System stack |
| motion.duration_micro | 100ms | Canonical `--dur-micro` |
| motion.duration_fast | 200ms | Canonical `--dur-fast` |
| motion.duration_base | 300ms | Canonical `--dur-base` |
| motion.duration_slow | 450ms | Canonical `--dur-slow` |
| radii.base | 8px | |
| voice.profile | "Direct/Technical" | |
| surfaces | ["marketing"] | |
| creative_brief.risk_dial | "elevated" | See generation_flow.md for behavior per level |

### Contrast Validation

After deriving all colors, verify these minimum contrast ratios:

| Pair | Minimum Ratio | Standard |
|------|--------------|----------|
| fg on bg (light mode) | 4.5:1 | WCAG AA normal text |
| fg on bg (dark mode) | 4.5:1 | WCAG AA normal text |
| accent on bg | 3:1 | WCAG AA large text / UI components |
| fg-muted on bg | 3:1 | WCAG AA large text |
| error on bg | 3:1 | WCAG AA UI components |
| success on bg | 3:1 | WCAG AA UI components |

If any pair fails, adjust the foreground color (not the background) until the ratio passes. Document the adjustment in the `substitutions` array with `reason: "contrast_adjustment"`.

**Contrast checking method:** Use the relative luminance formula:
- L = 0.2126 * R_lin + 0.7152 * G_lin + 0.0722 * B_lin
- For each channel: if sRGB <= 0.04045, linear = sRGB / 12.92; else linear = ((sRGB + 0.055) / 1.055) ^ 2.4
- Contrast ratio = (L_lighter + 0.05) / (L_darker + 0.05)

---

## Decision Log Guidelines

The decision_log provides traceability for every major design decision. It enables:
- Evaluation agents to verify decisions match the original rationale
- Iteration mode to understand why tokens were chosen
- Future sessions to reconstruct the reasoning

### What to log
Log decisions that:
- Select an aesthetic direction or custom adjustment
- Override a preset's default (e.g., "warmer neutrals despite cool preset")
- Choose between alternatives (e.g., "1.25 scale ratio over 1.2 or 1.333")
- Infer from context rather than explicit user input (e.g., "assumed dense layout from developer tool context")
- Make accessibility-related adjustments (e.g., "lightened fg-muted from #A1A1AA to #B4B4BA for 4.5:1 contrast on bg")

### What NOT to log
- Token values that follow directly from the preset with no override
- Standard defaults applied per the fallback table
- Routine field population (brand name, icon library, etc.)

### Example entries

```yaml
decision_log:
  - id: "D-001"
    decision: "Aesthetic: Approachable Enterprise"
    rationale: "Non-technical audience, healthcare context. Approachable Enterprise reduces intimidation through round shapes and warm neutrals."
    source: "Interview Phase B -- user selected Approachable Enterprise from 4 options"
    user_context_ref: "audience=non-technical; trust_level=high; industry=healthcare"
    principle_ref: "Approachability + visual hierarchy"
    accessibility_ref: "Readable body text at 16px, line-height >= 1.5"
    research_ref: "design_references.md#Healthcare"
  - id: "D-002"
    decision: "Scale ratio: 1.25"
    rationale: "Comfortable hierarchy for consumer-facing portal. Not dense (1.2) and not dramatic (1.333). Standard for Approachable Enterprise."
    source: "Derived from aesthetic preset"
    user_context_ref: "content_density=medium"
    principle_ref: "Information hierarchy and scan rhythm"
    accessibility_ref: "Maintain minimum body size and line-height"
    research_ref: "n/a"
  - id: "D-003"
    decision: "Neutral temperature adjusted to warm"
    rationale: "Damietta region has warm cultural associations. Cool neutrals would feel clinical for a hospital serving families."
    source: "Domain research phase -- Middle Eastern cultural context"
    user_context_ref: "cultural_context.region=MENA"
    principle_ref: "Cultural resonance + trust signaling"
    accessibility_ref: "Preserve contrast targets across neutral backgrounds"
    research_ref: "External domain research notes"
```
