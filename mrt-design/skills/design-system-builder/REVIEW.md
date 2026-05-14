# Relentless Review: Professional Design System Skill

**Date:** 2026-05-02
**Method:** 5 parallel deep-read agents covering creative engine, context/orchestration, cross-file contradictions, completeness gaps, and scaling excellence.

---

## Executive Verdict

This skill has the **best process architecture** I've seen for AI design-system generation. The anti-slop detection, three-layer token architecture, aesthetic synthesis algorithm, and wave-based evaluation are genuinely innovative. The typography selection and visual storytelling files are best-in-class.

But it has **structural gaps that prevent it from producing truly distinctive output at scale.** The creative engine controls tokens (colors, fonts, radii) but not spatial logic (how things are arranged). Cross-file contradictions will produce code that fails its own evaluation checklist. Context management is underestimated by ~2x. And 17 standard components are missing.

**The skill is "strongly nice-to-have" but not yet "must-have."** The gap between thorough and transformational is real and addressable.

---

## Critical Findings (Must Fix)

### C-01: Aesthetic presets produce token variation, not conceptual differentiation

**Files:** `references/aesthetic_directions.md` (entire file, 492 lines), `references/aesthetic_synthesis.md`

Each of the 14 presets defines colors, fonts, radii, and a "best for" list. Two presets with different hex values will generate identical page layouts. Warm Editorial and Swiss Precision share the same structural DNA — only the surface tokens differ. The "Hybrid Approach" (line 469) tells agents to mix temperature from one, accent from another, typography from a third, producing muddled aesthetics with no coherent philosophy.

**No preset includes spatial philosophy** — how each aesthetic thinks about space, grid discipline, whitespace attitude, or alignment preference. The creative engine can tell an agent what colors to use but not HOW to arrange elements distinctively.

**Fix:** Add to each preset a 2-3 sentence "spatial philosophy" and "component character" note. Example: *Warm Editorial: "Space tells the story. Margins are generous and slightly asymmetric. Headlines breathe; body text is contained. Cards have soft borders, not shadows. The grid is a suggestion, not a cage."* In `aesthetic_synthesis.md`, add Step 6.5 "Derive Spatial Logic" that encodes grid discipline, section rhythm variation, whitespace attitude, and alignment preference into the context packet as `spatial_logic`.

---

### C-02: The Soft SaaS preset IS the AI slop the skill warns against

**File:** `references/aesthetic_directions.md`, lines 72-100

Soft SaaS uses DM Sans, indigo accent (#6366F1), rounded corners (8/12/16), pastel surfaces. This is literally the "Generic SaaS Syndrome" anti-pattern from `creative_inspiration.md` lines 465-473. The skill's own anti-slop detector would flag this preset's output. Yet it is presented as a valid starting point with no warning.

**Fix:** Either rename to something more specific (e.g., "Friendly B2B" or "Approachable Enterprise") and change the accent to a non-indigo hue, or add an explicit warning: "Soft SaaS is the closest to AI-default output. Do not ship this preset without customization — change the accent, add a signature moment, and ensure asymmetric layouts."

---

### C-03: Signature Anchor/DNA framework is the best idea but not propagated into the context packet

**File:** `foundations/visual_storytelling.md`, lines 130-154

The Anchor (one hero moment per page) vs DNA (propagation into 3+ component categories) split solves the "signature moments are hero-only decorations" problem perfectly. But the context packet schema in `SKILL.md` line 209 only has `signature_moment` as a single field. The Anchor/DNA split is not encoded in the packet. Agents generating toast notifications or settings forms have no way to propagate DNA into mundane components.

**Fix:** Rename packet fields to `signature_anchor` (hero concept) and `signature_dna` (3+ propagation rules naming specific components). Update all agent templates to reference these. Add "Signature DNA for Mundane Components" template to visual_storytelling.md showing how DNA manifests in toasts, tables, settings forms.

---

### C-04: Key creative fields may be stripped during agent packet slicing

**File:** `SKILL.md`, lines 207-219; `schemas/agent_context_packet.md`

Fields `creative_brief`, `tension_points`, `neutral_family`, `imagery`, `character_rules` are listed as packet contents, but the slicing logic in `agent_context_packet.md` doesn't guarantee these reach all generation agents. If slicing strips `imagery` from Agent C (component previews) or `character_rules` from Agents D-F (UI kits), those agents produce components without brand-specific character.

**Fix:** Add a mandatory rule in Phase 3 dispatch: "Agents A through F MUST receive: creative_brief, signature_dna, character_rules, density_system, typography block, and full color block. These fields must NOT be sliced out."

---

### C-05: Dark mode selector pattern contradicts across 3 files

**Files:**
- `schemas/token_schema.md` lines 429-460: mandates `[data-theme="dark"]` ONLY
- `schemas/theming_schema.md` lines 193-204: presents `.dark` class as equally valid
- `foundations/visual_system.md` line 122: says "`.dark` class or `prefers-color-scheme`"

Agents reading theming_schema will generate `.dark` selectors that fail the evaluation checklist, which enforces `[data-theme]`.

**Fix:** Make `[data-theme="dark"]` canonical everywhere. Remove `.dark` class from theming_schema or mark as legacy. Update visual_system.md line 122.

---

### C-06: `--dur-micro` is simultaneously canonical and banned

**Files:**
- `schemas/token_schema.md` lines 131-132, 418, 475: defines `--dur-micro: 100ms` as canonical in three places
- `schemas/evaluation_checklist.md` line 119: bans `--dur-micro` as non-canonical

Agents generating tokens will include it (token_schema says to). Agents evaluating will reject it (evaluation_checklist says it's non-canonical). Deadlock.

**Fix:** Either add `--dur-micro` to the evaluation checklist's allowed list, or remove it from all three token_schema locations.

---

### C-07: Evaluation checklist references phantom tokens that don't exist

**File:** `schemas/evaluation_checklist.md`, lines 202-211

References `--fg-heading`, `--semantic-error`, `--semantic-success`, `--semantic-warning`, `--fg-on-accent`, `--fg-disabled`. The canonical token dictionary defines `--error` (no "semantic-" prefix), `--on-accent` (no "fg-" prefix), and does NOT define `--fg-heading` or `--fg-disabled` at all. The evaluation agent will check contrast ratios against tokens that don't exist.

**Fix:** Correct all token names to match token_schema: `--error`, `--success`, `--warning`, `--on-accent`. Remove `--fg-heading` and `--fg-disabled` or add them to the canonical dictionary.

---

### C-08: Framework trigger field name is wrong

**File:** `schemas/output_schemas.md`, line 19

Says "when `framework_targets` specifies a framework" but the actual context packet field is `constraints.framework`. `framework_targets.md` is a reference file, not a trigger field. Agents may look for the wrong field and never trigger framework output.

**Fix:** Change all references from `framework_targets` to `constraints.framework` in output_schemas.md.

---

### C-09: `reading_direction` duplicated in 5+ locations with inconsistent casing

**File:** `schemas/agent_context_packet.md`

`reading_direction` appears in: `typography.language_strategy.reading_direction`, `constraints.reading_direction`, `global_context.reading_direction`, top-level `reading_direction`. Example packet shows `"LTR"` (uppercase) in one place and `"ltr"` (lowercase) in another. If Agent A reads one and Agent D reads another, CSS logical properties won't match.

**Fix:** Canonicalize to ONE field: `global_context.reading_direction`. Enforce lowercase `"ltr"` / `"rtl"`. Delete or make other occurrences computed aliases.

---

### C-10: Agent validation references wrong field paths

**File:** `workflow/agent_decomposition.md`, lines 503-507

References `brand.accent`, `brand.display_font`, `brand.body_font`. These don't exist in the packet schema. Actual paths are `colors.raw_palette.accent`, `typography.families.display`, `typography.families.body`. The validation will always fail and block dispatch.

**Fix:** Correct all field paths to match `schemas/agent_context_packet.md`.

---

### C-11: `agent_context_packet.md` is 48KB — too large for practical use

**File:** `schemas/agent_context_packet.md` (1,074 lines)

This single file is a schema, example, validation checklist, slicing spec, contrast validation algorithm, and decision log guide all in one. Loading it during Phase 2 synthesis consumes ~12,000+ tokens.

**Fix:** Split into: (1) `packet_schema.md` — YAML template and field descriptions (~400 lines). (2) `packet_validation.md` — pre-dispatch checklist and fallbacks (~200 lines). (3) `packet_slicing.md` — per-agent slicing table and compact rules (~200 lines). Move example packets to `references/`.

---

### C-12: Quick mode packet omits `global_context` mandated for all agents

**File:** `workflow/quick_mode.md`, lines 82-150

The slicing rules state `global_context` is mandatory for ALL agents. The quick packet template doesn't include it. Quick-mode agents won't know reading direction, locale, date format, or icon strategy. RTL or non-English projects will produce broken output.

**Fix:** Add `global_context` to the quick packet template (only 7 fields, ~15 lines).

---

### C-13: No product-type adaptation document

**Missing file.** `layout_compositions.md` defines 4 shells, `section_layouts.md` has a 4-line rhythm section, `content_design.md` has density by surface type. But there is no single document telling agents how to adapt visual density, component selection, color usage, motion budget, or content strategy by product type (marketing vs dashboard vs docs vs settings).

**Fix:** Create `foundations/product_type_adaptation.md` with four sections (Marketing, Dashboard, Documentation, Settings/Admin), each specifying: default shell, visual density, component priority list, color usage rules, motion budget, content targets, navigation patterns, and imagery strategy.

---

### C-14: No visual preview before committing to full generation

**Files:** `workflow/generation_flow.md`, `SKILL.md`

The first time anyone sees visual output is after 6-8 agents generate thousands of lines. If the direction is wrong, the entire generation is wasted. This is the #1 reason a human with Figma is faster.

**Fix:** Add Phase 2.5: Visual Sketch. One agent generates a single lightweight HTML file (~200 lines) with color swatches, type specimens, 3 component sketches (button, card, input), and one section layout. User confirms before Phase 3 dispatch.

---

### C-15: No cross-project learning mechanism

**File:** `workflow/iteration.md`, lines 175-220

`LEARNINGS.md` is project-local. The skill itself never improves from any generation. If 50 users independently fix the same issue, the skill has no mechanism to discover this. No agent template reads LEARNINGS.md. The feedback loop is defined but never closed.

**Fix:** (1) Add Step 0 to Phase 1: "Read LEARNINGS.md if it exists and incorporate known preferences." (2) After every N generations, propose amendments to the evaluation checklist, aesthetic directions, or generation flow.

---

## Major Findings

### Creative Engine

| ID | File | Lines | Issue |
|----|------|-------|-------|
| M-01 | `ai_slop_detection.md` | 155-169 | Distinctiveness score is gameable — additive scoring doesn't test relationships between elements. An agent can pass with Syne + default purple + centered layout + no signature moment. Replace with a coherence check requiring ALL of: non-default font, brand-derived accent, at least one asymmetric section, creative_brief visible in 3+ component categories. |
| M-02 | `ai_slop_detection.md` | 137-153 | "Safe but generic" classification has no enforcement. Add to severity table: "Flag to user for approval. Do not auto-deliver. Suggest 2-3 specific changes." |
| M-03 | `aesthetic_synthesis.md` | 666-674 | Uniqueness check "80% similarity" is unmeasurable by agents. Replace with concrete criteria: if output shares accent hue (within 30 degrees), same font pairing, AND same radius base as any preset, it's too similar. |
| M-04 | `pattern_innovation.md` | 1-234 | All 10 "unusual" patterns are standard (bento grids, parallax, sticky sections). Rename to "Layout Pattern Library" or add genuinely novel patterns. |
| M-05 | `visual_system.md` | 255-268 | Radius aesthetic mapping is hardcoded values, not ranges. Change to ranges with rationale so agents can deviate when the brand warrants it. |
| M-06 | `token_schema.md` | 131-136 | Canonical motion durations lock out entire aesthetic territories (playful needs spring easing, brutalist needs 0ms snaps, luxury needs 600ms+). Allow aesthetic presets to override specific motion tokens when documented. |
| M-07 | `token_schema.md` | 138 | "No other duration values are permitted" prevents Candy Pop from using 80ms snaps and Brutalist from using 0ms instant changes. Change to: "These are defaults. Aesthetic presets may override when documented in context packet." |
| M-08 | Multiple | Various | `character_rules` and `component_style_contract` are referenced in SKILL.md and evaluation but never operationally defined with concrete generation rules. Add to aesthetic_synthesis.md: derive 3-5 character rules naming component categories with specific token references. |
| M-09 | `creative_inspiration.md` | 1-188 | Trend catalog has no decision framework connecting brand attributes to trend selection. Add a "Trend Selection Decision Tree" filtering by mood energy level and surface type. |

### Cross-File Contradictions

| ID | File 1 | File 2 | Issue |
|----|--------|--------|-------|
| M-10 | `generation_flow.md` line 236 | `token_schema.md` lines 131-136 | Motion duration values shifted: "fast" is 200ms in token_schema but 100-150ms in generation_flow. Align generation_flow to canonical values. |
| M-11 | `theming_schema.md` line 31 | `token_schema.md` line 45 | `--bg-alt` maps to `--neutral-50` (same as `--bg`) in theming_schema but `--neutral-100` (distinct surface) in token_schema. Fix theming_schema. |
| M-12 | `theming_schema.md` lines 47-48 | `token_schema.md` lines 56-57 | `--border` is `--neutral-200` in theming_schema but `--neutral-300` in token_schema. `--border-strong` differs similarly. Fix theming_schema. |
| M-13 | `theming_schema.md` line 96 | `token_schema.md` line 439 | Dark `--accent-tint` is `color-mix(accent)` in token_schema but `--neutral-800` (ignores accent) in theming_schema. Fix theming_schema. |
| M-14 | `visual_system.md` line 252 | `token_schema.md` line 147 | `--radius-xl` is 20px in token_schema but 16px in visual_system. Fix visual_system. |
| M-15 | `framework_targets.md` lines 69-79 | `output_schemas.md` lines 47-55 | Directory structure: `ui/` vs `src/components/`. Fix framework_targets to match output_schemas. |
| M-16 | `framework_targets.md` lines 96-152 | `token_schema.md` lines 262-289 | Framework targets use physical CSS properties (`padding: var(--space-1) var(--space-3)`) despite token_schema mandating logical properties (`padding-inline`, `padding-block`) for RTL support. Fix all framework component CSS. |
| M-17 | `responsive_system.md` lines 300-306 | `token_schema.md` lines 99-112 | `--space-14` and `--space-16` referenced but only `--space-1` through `--space-10` are defined. Replace with `calc(var(--space-10) * 1.3)` or add tokens to schema. |
| M-18 | `visual_system.md` lines 365-370 | `token_schema.md` lines 131-136 | Visual system suggests non-canonical durations (80-120ms, 150ms, 250ms). Remap to canonical tokens only. |
| M-19 | `templates/readme_skeleton.md` | `schemas/output_schemas.md` lines 132-192 | Skeleton is missing: Sources, Brand essence, Signature look, Content fundamentals, Imagery, Motion, Layout rules, Iconography, Caveats. Update skeleton to match output_schemas. |

### Context & Orchestration

| ID | File | Lines | Issue |
|----|------|-------|-------|
| M-20 | `generation_flow.md` | 28-55 | Step 0.5 divergence has no token budget gate. Each micro-direction can blow up to 500+ words. Add inline limit: max 120 words per direction, max 400 words total. |
| M-21 | `generation_flow.md` | 384-394 | Step 9 Distinctiveness Verification has no home — no agent or main-chat step is assigned to run it. Assign to H1's evaluation checklist. |
| M-22 | `agent_decomposition.md` | 260-276 | H3 requires grep-based preprocessing of generated output but no orchestrator step exists for this. Add explicit Phase 4 preprocessing step or create Agent H0. |
| M-23 | `agent_decomposition.md` | 151-154 | Task 3 bundles Agent G (full packet + generation) with H1/H2 (evaluation). Context profile mismatch — separate into Task 3 (G) and Task 4 (H). |
| M-24 | `agent_context_packet.md` | 990-1074 | Compact packet example omits `shadows`, full `radii`, and `motion` entirely. Agents receiving it must guess these values. Add all sections the slicing table marks required. |
| M-25 | `agent_context_packet.md` | 587 | `signature_moment` labeled "legacy" but all templates depend on it. Either update all templates to `signature_anchor`/`signature_dna`, or commit to `signature_moment`. |
| M-26 | `context_budget.md` | 9-18 | Token estimates are ~2x too low. Phase 2 estimated at 14-20K tokens; reality is 30-40K+. Total estimated 35-65K; reality is 55-85K+. Re-measure with actual tokenizer. |
| M-27 | `agent_context_packet.md` | 372, 378 | `character_rules` and `component_style_contract` can contradict. No cross-validation during packet generation. Add to pre-dispatch checklist. |
| M-28 | `existing_project_extraction.md` | 262, 395, 420, 433 | References `component_patterns.md` (singular file) but skill now uses `components/` directory. Update all paths. |
| M-29 | `agent_task_templates.md` | 227, 296-297 | Agent templates don't include structured instructions for reading `component_style_contract`. Add "copy these values verbatim" section listing exact contract fields. |
| M-30 | `interview_framework.md` | 179-181 | Competitor scan has no timeout. If research takes 8+ minutes, interview stalls. Add 90-second max wait with reference-gallery fallback. |
| M-31 | `agent_decomposition.md` | 485 | 5-minute timeout for generation agents may be too short for complex UI kits. Increase to 8 minutes. |
| M-32 | `agent_context_packet.md` | 527 | `density_tier` (legacy) contradicts `density_system`. Add explicit override rule: "density_system wins; density_tier is ignored." |

### Completeness

| ID | File | Issue |
|----|------|-------|
| M-33 | `foundations/components/animations.md` | Critically thin — 53 lines, no tokens, no states, no dark mode, no accessibility, no brand expression. Needs full rewrite. |
| M-34 | `foundations/components/forms.md` | Missing: textarea, select, radio group, checkbox group, slider/range. Only covers text input and validation. |
| M-35 | `foundations/components/compositions.md` | ASCII diagrams only — no CSS/HTML code. Needs actual component wiring examples. |
| M-36 | `foundations/data_visualization.md` | Missing: chart type specs (bar, line, pie, area, scatter), dark mode chart styling, chart interaction patterns, chart animation transitions, any chart library guidance. |
| M-37 | 8 component files | accordion, breadcrumbs, date_picker, file_upload, forms, pagination, tabs, tooltip all missing: dark mode CSS, component tokens, brand expression, responsive behavior. |
| M-38 | Missing files | No checkbox, radio group, slider, command palette, popover, divider, progress bar, chip/tag, or stepper component files. All are standard in Material/Ant/shadcn. |
| M-39 | Missing files | No dashboard-specific components (filter panel, chart wrapper), no docs components (callout/admonition, code block, TOC), no marketing components (CTA block, testimonial, pricing table). |

### Scaling & Excellence

| ID | File | Issue |
|----|------|-------|
| M-40 | `generation_flow.md` lines 218-221 | Fixed spacing scale (4-8-12-16-24-32-48-64-96-128) produces recognizable "tell" after repeated use. Make parametric: vary base unit (4/5/6/8px), allow geometric scaling. |
| M-41 | `generation_flow.md` (entire) | No variation seed or parametric noise. Deterministic given same inputs. Two similar brands get nearly identical output. Add `variation_seed` with controlled jitter on base unit, type ratio, shadow offsets. |
| M-42 | `validation/css_validation.md` | Grep commands assume human execution. Agent H cannot run bash. Add "For Agent H" section describing what to check when reading files. |
| M-43 | `validation/performance_budgets.md` | File size budgets specified but not enforced in evaluation checklist. Agent H won't flag a 120KB HTML file. Add file-size checkpoints. |
| M-44 | `evaluation_checklist.md` lines 179-227 | Contrast validation requires LLM arithmetic on hex colors — unreliable. Add lookup table of common token-pair contrast ratios per aesthetic preset. |
| M-45 | `schemas/framework_targets.md` | No design-tool integration (Figma/Penpot DTCG export). Add Phase 4c: generate `tokens.json` in Design Tokens Community Group format. |

---

## Minor Findings

| ID | File | Lines | Issue |
|----|------|-------|-------|
| m-01 | `ai_slop_detection.md` | systemic | Anti-slop rules could flag intentional aesthetic choices as slop (e.g., zero radius in Brutalist). Add: "Check creative_brief before flagging — documented intentional patterns are NOT slop." |
| m-02 | `token_schema.md` line 148 vs `visual_system.md` line 253 | `--radius-full`: 999px vs 9999px. Visually identical but inconsistent. Standardize to 999px per token_schema. |
| m-03 | `theming_schema.md` lines 32, 85, 133 | `--bg-inset` defined and used in theming but not in canonical token dictionary. Add to token_schema or remove. |
| m-04 | `agent_task_templates.md` lines 43-44 | Section references don't match actual section names in target files. Update to match. |
| m-05 | `evaluation_checklist.md` lines 65, 118 | `--no-brand-*` check duplicated. Keep one, cross-reference the other. |
| m-06 | `iconography.md` | No RTL guidance for directional icons. Add: "In RTL, flip arrows/chevrons with `transform: scaleX(-1)`. Non-directional icons should NOT be flipped." |
| m-07 | `creative_inspiration.md` lines 191-460 | Reference gallery "learn" entries are generic wisdom, not stealable techniques. Add specific CSS patterns per site. |
| m-08 | `embedded_example_analysis.md` lines 84-95 | Overfitting risks identified but no enforcement. Add to evaluation: "Neutrals temperature check — flag warm bias unless interview explicitly requested warm." |
| m-09 | `analysis.md` lines 105-113 | Self-critique lacks regression test protocol. Add: "Generate 3 test outputs (fintech dashboard, agency portfolio, children's edu platform). Verify all three are visually distinct." |
| m-10 | `iteration.md` lines 175-220 | LEARNINGS.md format defined but no agent template reads it. Add Step 0 to interview: "Read LEARNINGS.md if exists." |
| m-11 | `quick_mode.md` lines 19-20 | Quick mode boundary is fuzzy. Add decision rule: "If user mentions >2 of: aesthetic direction, signature moment, brand values, competitor differentiation — use medium build." |
| m-12 | `interview_framework.md` lines 242-254 | Decision helpers default uncertain users to Swiss Precision, reinforcing it as safe default (which is AI slop). Rotate recommendations or derive from industry+audience. |
| m-13 | `generation_flow.md` lines 117-127 | `risk_dial` defines behavior but agents only get the string "elevated" — no CSS-level specifics. Add `risk_dial_permissions` to packet with allowed CSS properties. |
| m-14 | `agent_context_packet.md` lines 513-515 | `intentional_gaps` section defined but no template consumes it. Add to Agent G or remove. |
| m-15 | `existing_project_extraction.md` lines 68-134 | Grep-based color extraction will produce massive noise (hex in URLs, SVG paths, data attributes). Add pre-filtering: weight CSS context 10x, frequency floor of 3+. |

---

## Strengths to Preserve

These are genuinely best-in-class and should not be weakened:

1. **Aesthetic synthesis algorithm** (`aesthetic_synthesis.md` lines 583-677) — 8-step process with mood matrix, 60/40 blending, uniqueness check. Operational and computable.

2. **Typography selection** (`typography_selection.md`) — Script-aware, language-aware, with emotional font roles and page-part matrix. Handles non-Latin scripts properly.

3. **Signature Anchor/DNA framework** (`visual_storytelling.md` lines 130-154) — Exactly the right split between hero moments and systemic propagation.

4. **Anti-slop detection** (`ai_slop_detection.md`) — Structured mechanism to detect and reject generic output. Most design skills just hope output is good.

5. **Rule-breaking budget** (`generation_flow.md` lines 134-149) — Explicitly carves space for intentional exceptions while protecting accessibility. Unusual and valuable.

6. **Three-layer token architecture** — Primitive → semantic → component with clear authority chain. Professional-grade infrastructure.

7. **Validation files** — Include actual grep commands, Playwright code, Lighthouse thresholds, and axe-core integration. Not vague checklists.

8. **Content design** (`content_design.md`) — Voice profiles, tone adaptation, copy length targets, and voice-transformed examples across profiles and intents.

9. **Imagery & illustration** (`imagery_and_illustration.md`) — Per-aesthetic photo treatments with CSS filter values, asset strategy matrix, and licensing rules.

10. **Motion choreography** (`motion_choreography.md`) — Entrance sequences, scroll-triggered system with IntersectionObserver code, performance rules (opacity + transform only).

---

## Priority Fix Order

1. **Fix all cross-file contradictions** (C-05 through C-08, M-10 through M-18) — These will produce code that fails its own evaluation. Fastest ROI.

2. **Add spatial philosophy to aesthetic presets** (C-01) — This is the single biggest creative engine gap. Without it, everything looks structurally the same.

3. **Propagate Signature DNA into packet and templates** (C-03, C-04) — The best idea in the skill needs to actually reach agents.

4. **Split agent_context_packet.md** (C-11) — 48KB file is the biggest context management risk.

5. **Add Phase 2.5 Visual Sketch** (C-14) — Highest-ROI new feature. Saves entire wasted generations.

6. **Create product_type_adaptation.md** (C-13) — Without it, marketing pages and dashboards get the same density and component selection.

7. **Fix context budget estimates** (M-26) — Current estimates are ~2x too low. Mode selection will be wrong.

8. **Expand thin component files** (M-33 through M-39) — 8 components missing dark mode/tokens, 17 components entirely absent.

9. **Make spacing/motion parametric** (M-40, M-41) — Eliminates the most recognizable "tell" from repeated use.

10. **Add cross-project learning** (C-15) — Long-term skill improvement mechanism.

---

## Stats

| Category | Critical | Major | Minor |
|----------|----------|-------|-------|
| Creative Engine | 4 | 9 | 2 |
| Cross-File Contradictions | 4 | 10 | 4 |
| Context & Orchestration | 4 | 13 | 3 |
| Completeness | 2 | 7 | 0 |
| Scaling & Excellence | 2 | 6 | 6 |
| **Total** | **16** | **45** | **15** |
