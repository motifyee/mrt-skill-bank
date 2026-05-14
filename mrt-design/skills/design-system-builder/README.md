# Professional Design System Skill

A modular AI skill for building complete, professional design systems and branded
web interfaces — from discovery through delivery.

## What This Skill Does

Given a product brief, brand direction, existing codebase, or even a vague idea,
this skill produces:

1. **CSS token file** (`tokens.css`) — complete design tokens with light/dark themes (high-contrast generated only when `theme_modes` specifies it)
2. **System documentation** (`README.md`) — brand, voice, visual rules, component specs
3. **Decision log** (`DECISIONS.md`) — explicit rationale for key design decisions
4. **Brand AI skill** (`SKILL.md`) — makes the system reusable by AI
5. **Visual dashboard** (`design-system.html`) — browseable system explorer with scrollspy
6. **Component previews** (`preview/`) — isolated component demonstrations
7. **UI kit prototypes** (`ui_kits/`) — click-through assembled pages for multiple surfaces

## Six Workflows

**Greenfield** — Build a design system from scratch through structured interview,
token synthesis, and generation. Phases 1–5.

**Medium build** — The default practical mode for most projects: short interview,
compact packet, 2-3 grouped tasks, and lightweight validation. See
`workflow/agent_decomposition.md`.

**Quick/Lite** — Single component, single page, or quick palette. Minimal interview,
single agent. See `workflow/quick_mode.md`.

**Existing project** — Extract tokens from a living codebase, normalize them,
audit gaps, generate the design system, and plan the migration. See `workflow/existing_project_extraction.md`.

**Visual audit** — Score existing UIs across 10 dimensions with specific fixes.
See `workflow/visual_audit.md`.

**Iteration** — Modify tokens or components in a previously generated design system.
See `workflow/iteration.md`.

## Architecture

```
SKILL.md                              Router (delegates to reference files)
|
+-- foundations/                       Professional knowledge base (22 files)
|   +-- ux_and_accessibility.md       WCAG, states, responsive, cognitive accessibility
|   +-- visual_system.md              Color, type, spacing, hierarchy, motion, elevation
|   +-- content_design.md             Voice, copy, microcopy, errors, tone
|   +-- components/                   Deep component docs (20 focused files)
|   +-- iconography.md                Icon selection, sizing, accessibility
|   +-- layout_compositions.md        Page-level layout patterns, section templates
|   +-- responsive_system.md          Breakpoints, grid behavior, mobile-first rules
|   +-- imagery_and_illustration.md   Illustration styles, photo treatment
|   +-- motion_choreography.md        Entrance sequences, scroll systems
|   +-- micro_interactions.md         Loading states, skeletons, toasts, empty states
|   +-- data_visualization.md         Chart colors, dashboard grids, KPI cards
|   +-- visual_storytelling.md        Narrative flow, section rhythm, signature moments
|
+-- workflow/                          Process guidance (7 files)
|   +-- interview_framework.md        Discovery questions + decision helpers
|   +-- generation_flow.md            Token synthesis + output pipeline
|   +-- agent_decomposition.md        Parallel agent task breakdown + portability
|   +-- existing_project_extraction.md  Reverse-engineer from living codebase
|   +-- visual_audit.md               10-dimension scoring rubric
|   +-- quick_mode.md                 Lightweight workflow for single deliverables
|   +-- iteration.md                  Modify existing design systems
|
+-- schemas/                           Output structure definitions (8 files)
|   +-- token_schema.md               CSS variable architecture, naming, DTCG
|   +-- output_schemas.md             README, dashboard, preview, kit templates
|   +-- evaluation_checklist.md       Quality gates per deliverable
|   +-- theming_schema.md             Light/dark/high-contrast theme switching
|   +-- framework_targets.md          React, Vue, Astro, Svelte component mapping
|   +-- agent_context_packet.md       Serialized token+brand state format + per-agent slicing
|   +-- agent_task_templates.md       Prompt templates for logical roles + grouped task execution
|   +-- context_budget.md             Context-cost accounting and execution mode gates
|
+-- references/                        Supplementary material (8 files)
    +-- embedded_example_analysis.md   Reference benchmark analysis
    +-- aesthetic_directions.md        14 curated aesthetic starting points
    +-- aesthetic_synthesis.md         Mood matrix, font pairing engine, color narratives
    +-- typography_selection.md        Language/script-aware expressive type selection
    +-- creative_inspiration.md        Design trends, reference gallery, anti-patterns
    +-- pattern_innovation.md          Unusual layouts, novel interactions, signature moments
    +-- design_references.md           Case studies + industry benchmarks (combined)
    +-- ai_slop_detection.md           Checklist for identifying generic AI design patterns
    +-- analysis.md                    Skill self-critique and overfitting safeguards
```

## Design Principles

- **Usability over aesthetics.** Accessibility is a floor, not a feature.
- **Tokens are the source of truth.** No hardcoded values in components.
- **Aesthetic direction is user-driven.** No hardcoded defaults.
- **Typography is language-aware.** Font choices reflect primary language, script,
  brand values, surface role, and legibility constraints.
- **Content is design.** Voice and copy rules are part of the system.
- **Flag inferred values.** Honesty about what's real vs. generated.
- **Anti-slop.** Every output needs a distinctive aesthetic point of view.
- **Operate lean.** Prefer grouped tasks and compact packets unless the project truly needs full fanout.

## Framework Support

By default, generates plain HTML output. When `framework_targets` specifies a
framework (React, Vue, Astro, or Svelte), an optional Phase 4 agent converts the
HTML output to framework-specific components with TypeScript types, barrel exports,
and a `package.json`. Includes Tailwind config and styled-components theme object
mappings in `schemas/framework_targets.md` for developer reference.
All frameworks share the same `tokens.css` file.

## How to Use

Invoke this skill when building or refining any design system, branded interface,
or web artifact that requires coherent design foundations. The skill's structured
workflow produces dramatically better output than raw code generation.
