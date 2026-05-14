---
name: design-system-builder
description: "Build, extract, audit, or iterate professional design systems and branded web interfaces. WHEN: \"design system\", \"brand guidelines\", \"style guide\", \"UI kit\", \"extract design system\", \"visual audit\", \"AI slop\", \"iterate design system\", \"create a design system\"."
---

# Design System Builder

A modular skill for building complete, professional design systems and branded interfaces.
Grounded in UX research, accessibility standards, and design-system best practice — not
aesthetic opinion alone.

## Architecture

This skill separates **reusable professional knowledge** from **brand-specific output**.
The SKILL.md you are reading is a router and orchestrator. It coordinates parallel
agents that do the heavy lifting — reading foundations, generating files, and running
quality checks. The main chat session stays lean: interview, synthesize, dispatch, assemble.

**CRITICAL: Agent execution is mandatory, not optional.** This skill's reference library
is too large to load into a single context window. Dispatching agents with the context
packet is the only way to produce complete, high-quality output without exhausting
context. See `workflow/agent_decomposition.md` for the full agent architecture.

```
design-system-skill/
├── SKILL.md                            <- you are here (workflow router)
├── foundations/
│   ├── ux_and_accessibility.md         <- WCAG, states, feedback, cognitive accessibility, i18n/RTL
│   ├── visual_system.md                <- color, type, spacing, hierarchy, motion, elevation
│   ├── content_design.md               <- voice, copy, microcopy, error messages
│   ├── components/                     <- 20 component deep-dives (replaces single component_patterns.md)
│   │   ├── buttons.md
│   │   ├── cards.md
│   │   ├── forms.md
│   │   ├── navigation.md
│   │   ├── badges.md
│   │   ├── tables.md
│   │   ├── modals.md
│   │   ├── dropdowns.md
│   │   ├── tabs.md
│   │   ├── accordion.md
│   │   ├── tooltip.md
│   │   ├── toast.md
│   │   ├── toggle.md
│   │   ├── pagination.md
│   │   ├── file_upload.md
│   │   ├── date_picker.md
│   │   ├── breadcrumbs.md
│   │   ├── compositions.md
│   │   ├── section_layouts.md
│   │   └── animations.md
│   ├── iconography.md                  <- icon selection, sizing, usage
│   ├── layout_compositions.md          <- page-level layout patterns, section templates
│   ├── responsive_system.md            <- breakpoints, grid, page grid system, mobile-first, i18n/RTL
│   ├── imagery_and_illustration.md     <- illustration styles, photo treatment, art direction
│   ├── motion_choreography.md          <- entrance sequences, scroll systems, keyframe library
│   ├── micro_interactions.md           <- loading states, skeletons, toasts, empty states
│   ├── data_visualization.md           <- chart colors, dashboard grids, KPI cards
│   └── visual_storytelling.md          <- narrative flow, section rhythm, signature moments
├── workflow/
│   ├── interview_framework.md          <- discovery questions, competitor scan, decision helpers
│   ├── generation_flow.md              <- synthesis, token generation, output pipeline
│   ├── agent_decomposition.md          <- parallel agent tasks, batched/sequential modes, portability
│   ├── existing_project_extraction.md  <- reverse-engineer design system from living codebase
│   ├── visual_audit.md                 <- 10-dimension scoring rubric for existing UIs
│   ├── quick_mode.md                   <- lightweight workflow for single deliverables
│   └── iteration.md                    <- modify existing design systems with targeted diffs
├── schemas/
│   ├── token_schema.md                 <- CSS variable architecture, naming, DTCG
│   ├── output_schemas.md               <- README template, preview page, UI kit structure
│   ├── evaluation_checklist.md         <- quality gates for every deliverable
│   ├── theming_schema.md               <- light/dark/high-contrast theme switching
│   ├── framework_targets.md            <- React, Vue, Astro, Svelte component mapping
│   ├── agent_context_packet.md         <- serialized token+brand state, per-agent slicing
│   ├── agent_task_templates.md         <- prompt templates for agents A-L
│   └── context_budget.md              <- context cost estimation, mode selection guidance
├── references/
│   ├── embedded_example_analysis.md    <- Bitrails reference benchmark analysis
│   ├── aesthetic_directions.md         <- 14 starting aesthetics with full tokens
│   ├── aesthetic_synthesis.md          <- mood matrix, font pairing engine, color narratives
│   ├── typography_selection.md         <- language/script-aware expressive type selection
│   ├── creative_inspiration.md         <- design trends, reference gallery, anti-patterns
│   ├── pattern_innovation.md           <- unusual layouts, novel interactions, signature moments
│   ├── design_references.md            <- case studies + industry benchmarks (combined)
│   ├── ai_slop_detection.md            <- checklist for identifying generic AI design patterns
│   └── analysis.md                     <- skill self-critique and overfitting safeguards
├── templates/
│   ├── tokens_skeleton.css             <- CSS file with section headers, systematic naming
│   ├── dashboard_skeleton.html         <- Dashboard HTML structure with theme toggle
│   └── readme_skeleton.md              <- README with section headers and changelog
└── validation/                         <- quality-gate checks for generated output
    ├── accessibility_smoke_test.md
    ├── css_validation.md
    ├── visual_regression.md
    └── performance_budgets.md
```

## Workflow Modes

### Mode 1: Full Greenfield (Phases 1–5)

For new design systems from scratch. Follow Phases 1–5 below.

### Mode 2: Medium Build (2-3 Grouped Tasks)

For most real projects: short interview, compact packet, grouped generation tasks,
and lightweight validation. This is the default recommendation when the user needs a
full branded output but not the maximum orchestration depth. See
`workflow/agent_decomposition.md` runtime Mode 2.

### Mode 3: Quick/Lite (Single Agent)

For small requests: one component, one page, a quick color palette, or a styled element.
See `workflow/quick_mode.md`.

### Mode 4: Existing Project (Extract & Refactor)

For extracting and upgrading a living codebase. Dispatch an agent using the template
from `workflow/existing_project_extraction.md`.

### Mode 5: Visual Audit (Score & Fix)

For scoring existing UIs and detecting AI slop. Dispatch an agent using the Visual Audit
template from `schemas/agent_task_templates.md`.

### Mode 6: Iteration (Modify Existing System)

For changing tokens or components in a previously generated design system.
See `workflow/iteration.md`.

---

### Full Workflow — Execution Model

```
Main Chat (Orchestrator) — stays lean, never reads foundation files
│
├── Phase 1: Discover (main chat — conversational, lightweight)
│   └── [Background] Competitor Research Agent
│
├── Phase 2: Synthesize (main chat — compile context packet from interview answers)
│   └── Read: interview_framework.md, generation_flow.md, aesthetic_directions.md
│       (Primary synthesis inputs. The main chat may also read lightweight orchestration
│        schemas in Phase 2/3: agent_context_packet.md, agent_task_templates.md,
│        agent_decomposition.md, and context_budget.md.)
│
├── Phase 2.5: Visual Sketch (main chat — lightweight HTML sketch for user confirmation)
│   └── Generate ~200-line HTML sketch with color swatches, type specimens, 3 component sketches, one section layout
│
├── Phase 3: Dispatch (main chat — send agents with context packet)
│   └── [Background] All generation agents run in PARALLEL
│       ├── Agent A: CSS Tokens
│       ├── Agent B: Preview Dashboard
│       ├── Agent C: Component Previews
│       ├── Agent D-F: UI Kits (one per surface)
│       └── Agent G: Documentation + DECISIONS.md
│
├── Phase 4: Evaluate (background wave execution after generation completes)
│   └── [Background] Agent H1 + H2 (parallel), then H3 cross-reference
│
└── Phase 5: Assemble (main chat — lightweight merge and delivery)
```

**Rules:**
1. The main chat should avoid deep foundation reads when the task can be solved by
   synthesis + orchestration docs. If blocked, read only the smallest relevant subset.
2. The main chat should prefer delegating CSS, HTML, and UI-kit generation to agents.
   Direct generation is acceptable in quick or repair flows.
3. The main chat primarily handles: interview, synthesis, context packet, agent dispatch, assembly.
4. All generation agents are independent and consume the same decision source (the context packet),
   but packet slicing is mandatory by default (see `schemas/agent_context_packet.md`).
5. Single-artifact requests use Quick Mode (`workflow/quick_mode.md`) instead of the
   full decomposition.
6. **Portability:** If your runtime does not support parallel agent dispatch, use the
   single-agent fallback described in `workflow/agent_decomposition.md` under
   "Single-Agent Fallback."
7. **Context gates are mandatory:** Follow `schemas/context_budget.md` before dispatch.
   If the packet or expected outputs exceed the selected mode budget, downgrade to a safer mode.
8. **Distinctiveness is mandatory:** Every full system needs a documented signature moment
   unless the user explicitly asks for utilitarian minimalism.

---

### Phase 1 — Discover (Main Chat)

Conduct the interview. This is the ONLY phase that runs entirely in the main chat.

1. Read `workflow/interview_framework.md` (~7KB).
2. Conduct an adaptive interview covering: product identity, audience, trust level,
   content density, voice, aesthetic direction, UI surfaces, technical constraints.
3. **Simultaneously**, dispatch a background agent to research 2-3 competitor sites
   using the Competitor Research template from `schemas/agent_task_templates.md`.
4. When the interview concludes, collect the competitor research results.

### Phase 2 — Synthesize (Main Chat)

Translate interview answers into a context packet. This stays in the main chat because
it requires conversational context from Phase 1.

1. Read `workflow/generation_flow.md` (~6KB) for token synthesis logic.
2. Read `references/aesthetic_directions.md` (~10KB) to map the chosen aesthetic to
    concrete token values.
3. Read `references/typography_selection.md` when the system has a non-English,
   multilingual, brand-forward, editorial, luxury, playful, cultural, or typography-led
   requirement. Use it to make typography language-aware instead of Latin-default.
4. Run a brief divergence pass: generate 2-3 micro-directions, select one, then lock the system.
5. Derive all design tokens: colors, typography, spacing, radii, shadows, motion, voice.
6. Serialize everything into the context packet format defined in
    `schemas/agent_context_packet.md`. The packet includes:
    - **creative_brief:** 3-5 sentence art direction that keeps agents aligned.
    - **decision_log:** captures every key design decision and its rationale for traceability.
    - **signature_anchor:** the one immediately recognizable visual idea (single phrase).
    - **signature_dna:** 3+ propagation rules naming specific component categories that carry the mark.
    - **signature_moment.systemic_effects:** legacy field -- explains how the distinctive idea affects ordinary components.
    - **density_system:** density tier plus spacing, section, and whitespace behavior.
    - **typography.language_strategy:** primary language, secondary languages, script system,
      reading direction, and cultural typographic notes.
    - **typography.expressive_roles:** which font voice is used for hero, headings, body,
      UI labels, data, and code.
    - **typography.font_rationale:** why each font fits the brand values, language,
      page role, and legibility requirements.
    - **components.character_rules:** brand-specific treatment for buttons, cards, inputs, and navigation.
    - **imagery:** photo, illustration, color treatment, crop, and subject rules.
    - **reading_direction:** `ltr` or `rtl` — triggers RTL-aware token generation (logical properties, mirrored layouts).
    - **cultural_context:** locale and cultural markers that influence color symbolism, date formats, and iconography choices.
7. Prefer the compact operational packet for A-F generation agents when context pressure is non-trivial.

Avoid loading large foundation/reference files in the main chat unless needed to unblock
the run. Prefer synthesis + orchestration docs in the main thread, and let generation
agents read the deeper references they need.

### Phase 3 — Dispatch (Main Chat → Agents)

Read `workflow/agent_decomposition.md` for the full agent architecture, then dispatch
the grouped tasks or full logical roster appropriate to the runtime mode.

1. For each required grouped task or logical agent, grab the relevant prompt template(s)
   from `schemas/agent_task_templates.md`.
2. Inject the context packet into the `{{CONTEXT_PACKET}}` placeholder.
3. Dispatch grouped tasks using the runtime's real task/subagent tool.
4. Default grouped recommendation:

   - Task 1: A+B+C (`tokens.css`, `design-system.html`, `preview/*`)
   - Task 2: requested D/E/F surfaces only (`ui_kits/*`)
   - Task 3: G + lightweight H1/H2 (`README.md`, `DECISIONS.md`, `SKILL.md`, first-pass validation)

5. Full logical roster when needed:

| Agent | Always? | Condition |
|-------|---------|-----------|
| A: CSS Tokens | Yes | — |
| B: Preview Dashboard | Yes | — |
| C: Component Previews | Yes | — |
| D: Marketing UI Kit | If surface needed | `surfaces` includes "marketing" |
| E: Dashboard UI Kit | If surface needed | `surfaces` includes "dashboard" |
| F: Docs UI Kit | If surface needed | `surfaces` includes "docs" |
| G: Documentation + DECISIONS.md | Yes | — |

6. For single-artifact requests, use Quick Mode (`workflow/quick_mode.md`).

### Phase 4 — Evaluate (Background Waves)

After all generation agents complete, dispatch the Evaluation Agent. Agent H runs in
three sequential waves (see `workflow/agent_decomposition.md`):

1. **Agent H1 (Core):** Structural checks — reads `schemas/evaluation_checklist.md` and
   all generated output files. Returns a pass/fail report on token completeness,
   naming consistency, and schema compliance.
2. **Agent H2 (Kits):** UI-kit-specific checks — validates previews and kits against
   quality gates in `schemas/evaluation_checklist.md` and anti-slop controls.
3. **Agent H3 (Cross-refs):** Cross-reference validation — verifies that token names,
   file index entries, and internal links are consistent across all deliverables.
4. If any wave reports failures, fix them in the main chat (targeted edits, not re-generation).

### Phase 5 — Assemble (Main Chat)

Gather agent outputs and deliver.

1. Collect all generated files from agents.
2. Verify cross-references: token names consistent, file index correct, links valid.
3. Deliver the complete design system to the user.

## Context Budget Enforcement

Before dispatch, run these hard gates:

1. Build the packet and estimate size using `schemas/context_budget.md`.
2. If packet > 250 lines, switch to Mode 2 or Mode 3.
3. If packet > 320 lines after prose trimming, block Mode 1 and use Mode 3.
4. If expected validation inputs exceed a single agent window, use H1/H2/H3 wave evaluation.
5. If main-chat working set approaches 65K tokens, stop loading additional docs and downgrade mode.

Dispatch is blocked until all gates pass.

## Key Principles

1. **Agents are mandatory.** This skill's reference library exceeds 500KB.
   Loading it into a single context window produces worse output than dispatching
   specialized agents that read only what they need. Always use the agent architecture.

2. **Usability is not negotiable.** Aesthetic choices must never degrade usability.
   Accessibility is a floor, not a feature. Generation agents read `foundations/ux_and_accessibility.md`.

3. **Tokens are the source of truth.** Every color, font, size, and spacing value
   lives in CSS custom properties. Components reference tokens, never raw values.
   Neutral scale construction uses **OKLCH interpolation** for perceptually even
   spacing between steps.

4. **Aesthetic direction is a user input, not a default.** Never assume warm/cool,
   light/dark, serif/sans, rounded/sharp. Derive from the interview.

5. **Typography is language-aware.** Font choices must fit the primary language,
   script, brand essence, and page role. A luxurious Arabic system, friendly Japanese
   app, comic Latin page, and technical Devanagari dashboard need different type
   logic, scale metrics, and fallbacks.

6. **Content is design.** Voice, tone, copy length, microcopy patterns, and error
   messages are part of the system. Generation agents read `foundations/content_design.md`.

7. **Flag inferred values.** When assets are generated rather than provided (logos,
   fonts, imagery), mark them with a substitution flag so the user knows what to
   replace when real assets become available.

8. **Anti-slop.** Generated interfaces must not look generically AI-made. No Inter
   as default heading font, no purple gradients, no centered-everything layouts,
   no cookie-cutter component styling. Every output needs a distinctive,
   intentional aesthetic point of view. The evaluation agent runs the full
   slop detection checklist from `references/ai_slop_detection.md`.

9. **Iterate, don't regenerate.** When the user wants to change tokens or
   components after initial generation, use the iteration workflow
   (`workflow/iteration.md`) to produce targeted diffs, not a full rebuild.

10. **Density-aware systems.** The context packet's `density_system` field
   controls spacing multiplier, section padding, component count, and whitespace
   attitude. Agents must respect this setting when generating layouts and component spacing.

11. **RTL-aware generation.** When the context packet's `reading_direction` is `rtl`,
    agents must emit CSS logical properties (`margin-inline-start` not `margin-left`),
    mirrored flex/grid directions, and bidirectional icon replacements.

## Composing with Other Skills

This skill works with:

- **frontend-design** — Handoff protocol:
  1. Generate design system first (produces `tokens.css`)
  2. Invoke frontend-design with the token file path as context
  3. frontend-design reads `tokens.css` for all color/spacing/typography values
  4. The context packet can be shared directly — frontend-design uses the same
     brand/colors/typography fields for its component generation
  5. Interface: `tokens.css` path + context packet YAML = complete handoff
- **web-artifacts-builder** — For complex multi-component React apps, use its
  bundling pipeline with this skill's design tokens.
- **pptx / pdf / docx** — Apply the generated brand tokens to document outputs.

## Runtime Portability

This skill supports three execution modes (full parallel, batched, sequential) depending
on runtime capability. See `workflow/agent_decomposition.md` (Runtime Modes section)
for mode selection. The context packet
architecture works identically in both modes — the only difference is parallelism.
