# Quick Mode

Lightweight workflow for small, focused requests. Use when the user wants a single
component, one page, a quick color palette, or any task that doesn't need the full
5-phase pipeline with 7+ agents.

---

## When to Use Quick Mode

- Single component: "style a button", "create a card component"
- Single page: "build a landing page", "create a pricing section"
- Quick palette: "give me a color scheme for a fintech app"
- Token-only: "generate CSS tokens for this brand"
- Rapid prototype: "mock up a dashboard layout"
- Any request where the user provides a complete brief in one message

## When NOT to Use Quick Mode

- Full design system (use greenfield workflow)
- Brand-forward system with multiple deliverables but moderate scope (use medium build)
- Multiple surfaces needed (use full workflow)
- Existing codebase extraction (use existing_project_extraction.md)
- Visual audit (use visual_audit.md)

---

## Quick Mode Flow

### Step 1: Assess the Request

Determine what the user needs:

| Request Type | Output | Agent Template |
|-------------|--------|----------------|
| Token-only | `tokens.css` | Agent A |
| Single component | HTML file with component + tokens | Agent C (single file) |
| Single page | Complete HTML page | Agent D/E/F (relevant surface) |
| Quick palette | Token definitions (no files) | None — generate inline |
| Component library | `preview/*.html` files | Agent C |

### Step 2: Gather Minimal Context

Skip the full interview. Collect only what's needed:

**Required (ask if not provided):**
- Brand/product name
- Aesthetic direction or reference
- Primary language/script when typography or copy is central

**Skip entirely:**
- Full competitor landscape research (2-3 site deep scan)
- Detailed audience profiling
- Multi-surface planning
- Content density discussion

**Infer carefully from context:**
Avoid industry-to-aesthetic stereotypes. If the user only gives an industry
("fintech", "developer tool", "SaaS"), ask or infer two micro-signals before
choosing a direction:
- First impression: trust, speed, warmth, luxury, play, precision, calm, or disruption
- Usage posture: first-time visitor, evaluator, daily power user, or internal operator
- Typographic spirit: luxurious, comic/playful, friendly, beautiful/editorial,
  technical, authoritative, calm, crafted, or another clear value

Map those signals to an aesthetic. Example: fintech + Gen-Z + playful evaluator
may be Candy Pop/Swiss hybrid, not automatic navy Swiss Precision. Developer
tool + designer audience may be Warm Editorial/Tech Blueprint, not automatic
Neon Dashboard.
- If they provide a URL → extract colors/fonts directly

**Maximum 2-3 questions.** If the user provided a clear brief, ask zero questions.

**Quick enrichment rule:** for user-facing page/component requests, run a lightweight
benchmark check (1 relevant reference or competitor pattern) before generation.

### Step 3: Generate a Quick Context Packet

Create a minimal context packet with only the fields needed for the task.

```yaml
# Quick context packet — trimmed for single-deliverable use
version: "1.1"

brand:
  name: ""
  industry: ""

aesthetic:
  origin: ""
  modifications: []
  mood_keywords: []

colors:
  temperature: ""
  raw_palette:
    neutral_dark: ""
    neutral_light: ""
    accent: ""
  semantic_mapping:
    bg: ""
    fg: ""
    accent: ""

typography:
  families:
    display: ""
    body: ""
    mono: ""
  scale_ratio: ""
  language_strategy:
    primary_language: ""
    script_system: ""
    reading_direction: ""
  expressive_roles:
    hero: ""
    body: ""
    ui_label: ""
    data: ""
  font_rationale:
    display: { expresses: [], script_fit: "" }
    body: { expresses: [], script_fit: "" }
  levels:
    h1: { size: "", weight: "" }
    h2: { size: "", weight: "" }
    body: { size: "", weight: "" }

spacing:
  base_unit: ""
  scale: { "1": "", "4": "", "6": "", "8": "" }

radii:
  md: ""

voice:
  profile: ""
  casing: ""

constraints:
  framework: ""
  icon_library: ""

global_context:
  reading_direction: ltr
  locale: en-US
  date_format: YYYY-MM-DD
  icon_strategy: lucide
  language: en
  currency: USD

# ── CREATIVE MINIMUM (required even in Quick Mode) ──
creative_minimum:
  thesis: ""          # One sentence: what makes this brand's visual identity non-generic
  refusal: ""         # One thing this brand would never do visually (e.g. "never use rounded pill buttons")
  tension: ""         # One deliberate contrast (e.g. "oversized serif headline + tiny monospace label")
  character_rule: ""  # One component-level rule (e.g. "cards: left accent border, no shadow")
  signature: ""       # The one visual idea that propagates everywhere (e.g. "diagonal slash dividers")
```

These 5 fields are the minimum viable creative brief. Without them, Quick Mode produces generic output. The thesis and refusal take 30 seconds to write and prevent the most common failure mode.

This is ~50 lines instead of the full ~200-line packet. Only fill sections relevant
to the task.

### Step 4: Dispatch Single Agent

Send one agent with the quick context packet + the relevant task template from
`schemas/agent_task_templates.md`.

For token-only requests, generate tokens inline without dispatching an agent.

### Step 5: Lightweight Validate + Deliver

Run a lightweight validation pass before returning output:
1. Validate token references (no hardcoded values in generated component/page styles)
2. Check minimum contrast for primary text/background pairs
3. Verify focus-visible styles exist for interactive elements
4. Confirm touch targets are at least 44px when the artifact is interactive

Return the output directly after this quick validation. No full assembly phase.

If the user wants to expand to a full design system later, transition to the
greenfield or medium-build workflow using the quick context packet as Phase 1 input.

---

## Quick Creative Minimum

Even in quick mode, the context packet must include five creative-minimum fields.
Without them, quick mode produces generic AI output indistinguishable from any
other generated system.

1. **Creative thesis** (1 sentence): What makes this brand's visual identity non-generic.
   Example: "An engineering tool that feels like a well-lit workshop, not a nightclub."
2. **Refusal statement** (1 sentence): What this brand would never do visually.
   Example: "Never use rounded pill buttons or gradient backgrounds."
3. **One tension point**: A deliberate contrast the system exploits.
   Example: "Oversized serif headline paired with tiny monospace metadata labels."
4. **One component character rule**: How the visual idea changes one component.
   Example: "Cards use a left accent border and no shadow."
5. **One signature/DNA rule**: The one visual idea that propagates everywhere.
   Example: "Diagonal slash dividers between sections, echoed in badge corners."

These five fields take under 30 seconds to write and prevent the most common failure
mode: generic, forgettable output that could belong to any brand.

---

## Quick Token Derivation

When generating tokens inline (no agent), use these shortcuts. Each row includes a
creative-minimum seed to prevent generic output.

| Aesthetic | Primary | Accent | Neutrals | Radii | Creative Seed |
|-----------|---------|--------|----------|-------|---------------|
| Warm Editorial | #1C1917 | #C2410C | Warm gray | 4-12px | Thesis: "A literary magazine for digital products." Refuses: rounded corners >12px. Tension: oversized serif + tiny sans label. Character: left-border cards, no shadow. DNA: warm paper texture on alternating sections. |
| Neon Dashboard | #E4E4E7 | #22D3EE | Cool gray | 6-14px | Thesis: "Cold precision with sudden warmth." Refuses: purple gradients. Tension: dense data + spacious hero. Character: zinc borders, cyan on hover only. DNA: calibrated cyan glow on active states. |
| Approachable Enterprise | #1E293B | ⚠ Choose a non-default accent — avoid #6366F1, #3B82F6, #8B5CF6 | Blue-gray | 8-16px | Thesis: "Approachable without being childish." Refuses: hard edges and neon. Tension: generous whitespace + dense feature grids. Character: pill badges, soft shadows. DNA: rounded containers with subtle accent tint backgrounds. |
| Brutalist Raw | #000000 | User choice | Pure gray | 0-4px | Thesis: "The interface is the content." Refuses: decoration, drop shadows. Tension: extreme weight contrast (900 vs 200). Character: 1px black borders, no radius. DNA: raw borders and monospaced data throughout. |
| Luxury Premium | #1A1A1A | #C9A96E | Warm dark | 2-8px | Thesis: "Restraint is the loudest signal." Refuses: saturated color, rounded shapes. Tension: minimal UI + opulent serif headlines. Character: thin gold borders, no fill. DNA: gilded dividers and refined serif contrast. |
| Retro Terminal | #33FF33 | #33FF33 | Dark green | 0-2px | Thesis: "The screen IS the brand." Refuses: modern rounded components. Tension: monospace everything + blinking cursor. Character: green-on-black, no radius. DNA: scanline texture and CRT glow on key elements. |
| Earth Organic | #292524 | #92400E | Warm brown | 8-16px | Thesis: "Grounded, alive, and tactile." Refuses: cold blues, clinical layouts. Tension: organic shapes + structured grid. Character: soft rounded corners, warm shadows. DNA: natural texture borders and earthy color washes. |
| Tech Blueprint | #0A2540 | #0A6ED1 | Blue-gray | 4-8px | Thesis: "Engineered clarity for complex systems." Refuses: decorative illustration. Tension: dense schematics + clean whitespace. Character: thin 1px borders, technical radius. DNA: blueprint grid lines and exploded-diagram spacing. |
| Candy Pop | #1A1A2E | #FF6B6B | Cool gray | 12-20px | Thesis: "Joy is a design decision." Refuses: muted palettes and minimalism. Tension: maximal color + structured layout. Character: pill buttons, saturated cards. DNA: rounded everything with punchy accent duotones. |
| Swiss Precision | #0A0A0A | #2563EB | Neutral gray | 4-8px | Thesis: "Order creates trust." Refuses: decoration without function. Tension: strict grid + one bold accent move. Character: flat rectangles, functional borders. DNA: mathematically consistent spacing and grid-aligned accent bars. |
| Wabi-Sabi Serene | #2C2418 | #8B7355 | Warm earth | 6-14px | Thesis: "Imperfect beauty in digital form." Refuses: symmetry and machine precision. Tension: organic irregularity + usable structure. Character: slightly varied radii, muted shadows. DNA: asymmetric layouts and natural-material color palette. |
| Rajwada Splendor | #1F1108 | #D4A853 | Warm gold | 6-16px | Thesis: "Heritage luxury, modern interface." Refuses: colonial-minimal cliches. Tension: ornate detail + clean function. Character: gold-accented borders, refined spacing. DNA: mandala-inspired pattern accents and jewel-tone highlights. |
| Islamic Geometry | #0D1B2A | #1B998B | Cool teal | 4-10px | Thesis: "Infinite pattern, finite screen." Refuses: figurative illustration. Tension: geometric complexity + minimal color. Character: patterned borders, teal accents. DNA: geometric tessellation as texture and decorative element. |
| Afrofuture Modern | #0A0A0A | #E63946 | Rich dark | 8-16px | Thesis: "The future has always been here." Refuses: Afro-kitsch and exoticism. Tension: bold geometric forms + warm community. Character: high-contrast panels, vibrant accents. DNA: bold pattern borders and saturated accent energy throughout. |

> **Display font note:** Choose a display font other than Inter for distinctiveness. Inter is a high-quality body/UI font but produces generic output when used as the display face. Any preset above that defaults to Inter should be overridden with a more expressive display font (e.g. a condensed grotesque, a slab serif, or a geometric display face) unless the user explicitly chose it.

Refer to `references/aesthetic_directions.md` for the full token sets.

---

## Quick Mode Quality Checks

Even in quick mode, verify:

- [ ] No hardcoded color values (use CSS variables)
- [ ] Contrast passes WCAG AA for text
- [ ] Font sizes follow a type scale
- [ ] Font choices fit the primary language/script and requested typographic spirit
- [ ] Spacing uses a consistent base unit
- [ ] If interactive: focus states exist, touch targets are 44px+
- [ ] If dark mode: all semantic tokens remap
