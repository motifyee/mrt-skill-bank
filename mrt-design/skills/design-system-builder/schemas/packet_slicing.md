# Packet Slicing

Per-agent slicing table, compact packet format, required fields per agent, and slicing rules.
This file was extracted from `agent_context_packet.md` for maintainability.

---

## Per-Agent Packet Slicing

Packet slicing is mandatory by default. Include only the sections each agent needs.
The `brand` and `constraints` sections are always included. Other sections are
included per the table below.

| Agent | Required Sections | Excluded Sections |
|-------|------------------|-------------------|
| A: CSS Tokens | brand, creative_brief, aesthetic, colors, typography, spacing, density_system, radii, shadows, motion, constraints (incl. reading_direction, locale), global_context, substitutions, signature_anchor, signature_dna, signature_moment, tension_points, components, decision_log | voice, surfaces |
| B: Preview Dashboard | brand, creative_brief, aesthetic, colors, typography, spacing, density_system, radii, shadows, motion, voice, constraints (incl. reading_direction, locale, theme_modes), global_context, imagery, components, signature_anchor, signature_dna, signature_moment, tension_points, decision_log | surfaces (components list only) |
| C: Component Previews | brand, creative_brief, colors, typography, spacing, density_system, radii, shadows, motion, voice, constraints, global_context, substitutions, components, signature_anchor, signature_dna, signature_moment, tension_points, decision_log | surfaces (component list only) |
| D: Marketing UI Kit | brand, creative_brief, aesthetic, colors, typography, spacing, density_system, radii, shadows, motion, voice, surfaces (marketing only), constraints (incl. locale, cultural_context), global_context, imagery, components, substitutions, signature_anchor, signature_dna, signature_moment, spatial_logic, tension_points, decision_log, research_evidence | -- |
| E: Dashboard UI Kit | brand, creative_brief, colors, typography, spacing, density_system, radii, shadows, motion, imagery, constraints (incl. locale, cultural_context, reading_direction), global_context, surfaces (dashboard only), components, signature_anchor, signature_dna, signature_moment, spatial_logic, tension_points, decision_log, research_evidence | voice, aesthetic |
| F: Docs UI Kit | brand, creative_brief, colors, typography, spacing, density_system, radii, shadows, motion, voice, constraints (incl. locale, cultural_context, reading_direction), global_context, surfaces (docs only), components, signature_anchor, signature_dna, signature_moment, spatial_logic, tension_points, decision_log | aesthetic |
| G: Documentation | brand, creative_brief, aesthetic, colors, typography, spacing, density_system, radii, shadows, motion, voice, surfaces, constraints (full -- all globalization fields), global_context, imagery, components, substitutions, signature_anchor, signature_dna, signature_moment, tension_points, intentional_gaps, decision_log, research_evidence | -- (needs full packet) |
| H: Evaluation | brand, constraints (for reference), global_context | (reads generated files directly) |

**Slicing method:** When constructing an agent prompt, include the full `brand`,
`constraints`, and `global_context` sections, then include only the sections listed
above. The following sections are mandatory for ALL agents and must never be sliced out:
`brand`, `constraints`, `global_context`, `creative_brief`, `signature_dna`,
`components.character_rules`, `density_system`, `typography`, `colors`, and `spatial_logic`. These fields
prevent cross-agent visual drift and ensure the signature DNA propagates consistently
across all outputs. Remove excluded sections entirely -- do not leave empty placeholders.
This typically reduces the packet from ~200 lines to ~120-150 lines per agent.

**Globalization note:** When `global_context.reading_direction` is `"rtl"`, Agent A must:
1. Add CSS logical properties (`margin-inline-start`, `padding-inline-end`, etc.) as the primary spacing approach.
2. Add a `[dir="rtl"]` override block at the bottom of `tokens.css` that flips any directional values that logical properties cannot cover (e.g., `text-align`, asymmetric padding overrides, SVG-based icons).
Agents D, E, F should use `constraints.locale` and `constraints.cultural_context` to localize example copy (date formats, number separators, placeholder text language).

Use full-packet fanout only when both conditions are true:
1. Packet line count is under 180.
2. Runtime has verified surplus context for all parallel agents.

### Compact Operational Packet

When packet pressure is high, generate a compact variant for A-F agents:
- Keep all numeric/token fields unchanged.
- **Typography levels must NEVER be compressed in compact packets.** All heading
  levels (display through caption: display, h1, h2, h3, h4, body_lg, body, body_sm,
  label, mono) must be preserved with their complete metrics (size, weight,
  line_height, letter_spacing). Surface agents cannot invent type sizes -- omitting
  any level forces agents to guess, causing cross-agent typographic drift.
- Compress `decision_log` to only IDs + short rationale fragments relevant to the agent.
- Reduce `research_evidence` to 1-2 findings with direct design impact.
- Preserve `signature_anchor` and `signature_dna` in full, because they are part of the aesthetic contract. `signature_moment` is a legacy field; new packets should use anchor + DNA.
- Preserve `components.component_style_contract` because it prevents shared
  components from drifting across dashboard, previews, and UI kits.
- Preserve `tension_points.implementation` and `rule_breaking_budget` because they
  are executable creative constraints, not rationale prose.
- Remove explanatory prose that does not change output decisions.

Use the full packet for Agent G and any traceability-heavy audit flows.

### Compact Packet Transformation Algorithm

Follow these steps in order when converting a full packet to compact format:

1. **Add format header.** Insert `format: compact` as the second line (after `version`).
2. **Preserve verbatim** (copy without modification):
   - `brand` (all fields)
   - `creative_brief` (all fields)
   - `signature_anchor`, `signature_dna`, `signature_moment`
   - `spatial_logic` (all fields)
   - `tension_points` (including `implementation`)
   - `rule_breaking_budget`
   - `components.character_rules`, `components.component_style_contract`
   - `global_context` (all fields)
   - `constraints` (all fields)
3. **Preserve all entries** (keep every item, do not omit steps):
   - `typography.levels` — all 10 levels with complete metrics
   - `spacing.scale` — all 10 steps
4. **Compress** (keep content, use inline YAML):
   - `typography.expressive_roles` — single-line map
   - `typography.font_rationale` — collapse to key fields only (font, use_for)
   - `density_system` — single-line map
5. **Remove entirely** (never include in compact):
   - `research_evidence` (except for Agent D)
   - `intentional_gaps`
   - `decision_log` (except for Agent A)
   - `typography.language_strategy.cultural_typographic_notes` if empty
   - `substitutions` (except for Agent A)
6. **Surface filter.** Keep only the surface relevant to the target agent (e.g., Agent D gets `surfaces[marketing]` only).
7. **Validate.** Before injection, confirm all mandatory fields from the required-fields table above are present. If any is missing, the compact packet is invalid — regenerate with that field included.

### Compact Packet Example

Concrete shape for A-F packet slices. Keep this shape stable so agents do not
invent incompatible compact formats.

```yaml
version: "1.1"
brand:
  name: "Velocity"
  industry: "developer tools"
  audience: "platform engineers"
creative_brief:
  statement: "Cold precision with sudden warmth. Dense data surfaces stay calm, while the hero gets one sharp atmospheric moment. Nothing decorative survives unless it helps orientation."
  philosophy: "This system believes deployment should feel controlled and refuses noisy cyberpunk."
  risk_dial: "elevated"
aesthetic:
  origin: "Neon Dashboard"
  modifications: ["Ink neutral family instead of default grey", "No purple secondary accent"]
  secondary_influences: ["Swiss grid discipline"]
colors:
  neutral_family: "Ink"
  dark_mode_strategy: "dark-first"
  raw_palette: { neutral_dark: "#08080C", neutral_dark_2: "#101018", neutral_light: "#EEEFF0", accent: "#22D3EE" }
  semantic_mapping: { bg: "#08080C", bg_alt: "#101018", fg: "#EEEFF0", fg_muted: "#A0A0B0", accent: "#22D3EE", on_accent: "#08080C", border: "#1E1E2A" }
typography:
  families: { display: "Space Grotesk", body: "IBM Plex Sans", mono: "Fira Code" }
  language_strategy: { primary_language: "English", secondary_languages: [], script_system: "Latin", reading_direction: "ltr" }
  expressive_roles: { hero: "technical display", section_heading: "display", body: "body", ui_label: "body medium", data: "mono/tabular", code: "mono" }
  font_rationale:
    display: { font: "Space Grotesk", expresses: ["precision", "speed"], script_fit: "Latin geometric forms support developer-tool sharpness", use_for: ["hero", "section headings"], avoid_for: ["long body"] }
    body: { font: "IBM Plex Sans", expresses: ["clarity", "trust"], script_fit: "Readable Latin UI face with technical tone", use_for: ["body", "labels"], avoid_for: ["brand mark"] }
  levels:
    display: { size: "72px", weight: "700", line_height: "1.05", letter_spacing: "-0.03em" }
    h1: { size: "56px", weight: "700", line_height: "1.1", letter_spacing: "-0.02em" }
    h2: { size: "32px", weight: "600", line_height: "1.15", letter_spacing: "-0.01em" }
    h3: { size: "24px", weight: "600", line_height: "1.2", letter_spacing: "0" }
    h4: { size: "20px", weight: "600", line_height: "1.3", letter_spacing: "0" }
    body_lg: { size: "18px", weight: "400", line_height: "1.6", letter_spacing: "0" }
    body: { size: "16px", weight: "400", line_height: "1.6", letter_spacing: "0" }
    body_sm: { size: "14px", weight: "400", line_height: "1.55", letter_spacing: "0" }
    label: { size: "12px", weight: "500", line_height: "1.3", letter_spacing: "0.08em" }
    mono: { size: "14px", weight: "400", line_height: "1.5", letter_spacing: "0" }
spacing:
  scale: { "1": "4px", "2": "8px", "3": "12px", "4": "16px", "5": "24px", "6": "32px", "7": "48px", "8": "64px", "9": "96px", "10": "128px" }
density_system:
  tier: "compact"
  spacing_multiplier: 0.85
  max_components_per_section: "7"
  whitespace_attitude: "compressed but breathable around primary data"
global_context:
  reading_direction: "ltr"
  lang: "en"
  locale: "en-US"
  date_format: "MM/DD/YYYY"
  number_format: "1,234.56"
  cultural_context: ""
  font_loading: "CDN"
  icon_strategy: "Lucide via CDN"
components:
  character_rules:
    buttons: "Square-ish corners (radius-md), sentence-case label, filled primary uses on_accent text"
    cards: "1px zinc border at rest; 1px cyan top border on hover/active -- no box shadow"
    inputs: "Full 1px zinc border; focus state swaps to 1px cyan border with subtle glow"
    navigation: "Monochrome zinc until active; active item gets 2px cyan bottom border, no bg fill"
    sections: "Dark void (#09090B) alternating with slightly lifted bg_alt (#18181B); no pure white sections"
  signature_propagation:
    - "Focus rings and selected nav states use the same cyan glow as the hero grid."
  component_style_contract:
    radius_language: "slightly-rounded"
    elevation_language: "flat"
    border_style: "full"
    density: "compact"
  rule_breaking_budget:
    allowed: ["Hero headline may use 72px display weight outside standard scale"]
    forbidden: ["forms", "tables", "accessibility states"]
signature_anchor: "Calibrated cyan grid-glow"
signature_dna:
  - "Hero -- subtle dot-grid with cyan glow overlay on the dark panel background"
  - "Navigation -- selected item receives 2px cyan underline plus 4px radial glow; all other items are flat zinc"
  - "Data/chart focus states -- active point emits cyan glow, binding infrastructure and analytics surfaces together"
signature_moment:
  concept: "calibrated grid glow"
  placements: ["hero", "selected nav", "chart focus states"]
  constraints: "Never use glow on body text or dense table rows."
  systemic_effects: ["cyan focus ring", "thin grid lines", "glow reserved for active states"]
tension_points:
  scale: "large technical display type against 12px metadata labels"
  density: "spacious hero followed by compact deployment table"
  structure: "break the grid only in the hero diagnostic panel"
  implementation:
    scale_css: "6x ratio -- display (72px) vs label (12px)"
    density_css: "hero --space-10 padding vs table --space-2 row gap"
    structure_css: "hero uses 7fr 5fr asymmetric grid; body uses 12-col"
intentional_gaps:
  - omitted: "Full competitor notes"
    recovery: "Read research_evidence only if copy or positioning is blocked"
```
