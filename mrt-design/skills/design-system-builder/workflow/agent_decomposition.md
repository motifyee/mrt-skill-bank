# Agent Decomposition

**MANDATORY execution strategy.** This is not optional — it is the primary way
this skill produces output. The main chat session is a thin orchestrator that
conducts the interview, synthesizes the context packet, dispatches agents, and
assembles results. All heavy work (reading foundations, generating files, running
evaluations) happens in agents.

---

## Why Agents Are Mandatory

This skill's reference library exceeds 500KB across 30+ files. Loading it into
a single context window:
- Consumes the entire context budget before generation begins
- Produces worse output because the model can't attend to all details equally
- Cannot parallelize — everything is sequential

Agent-based execution:
- Each agent reads only the 2-3 files it needs (~20-30KB)
- Agents run in parallel — 6-8x faster for full systems
- The main chat stays under the active mode budget from `schemas/context_budget.md`
- Quality is higher because each agent specializes in one deliverable

---

## Agent Dispatch Protocol

### How to Dispatch

Use your runtime's real task/subagent tool, not abstract `Agent({ ... })` syntax.
In task-based runtimes, dispatch grouped workstreams in a single turn whenever possible.
The logical A-L roster remains the design contract, but the recommended operational
execution usually collapses into 2-3 task calls.

```
// Recommended grouped dispatch in task/subagent runtimes

Task(
  subagent_type="general",
  prompt="<Grouped Task 1: tokens + dashboard + component previews>"
)

Task(
  subagent_type="general",
  prompt="<Grouped Task 2: requested UI kits only>"
)

Task(
  subagent_type="general",
  prompt="<Grouped Task 3: README + DECISIONS + project SKILL + lightweight validation>"
)
```

Use the full per-agent A-L split only when:
1. The runtime supports many parallel tasks cleanly.
2. The project is large enough to justify the extra orchestration.
3. File ownership remains disjoint.

### Agent Prompt Construction

Each task prompt is constructed by:
1. Taking the template from `schemas/agent_task_templates.md`
2. Replacing `{{CONTEXT_PACKET}}` with the serialized YAML context packet
3. Prepending the skill's base directory path so the agent can read reference files

Example preamble for every agent:
```
You are generating part of a design system. Your context is below.

The skill's reference files are at: [base directory path]
Read the files mentioned in your instructions as needed.

--- CONTEXT PACKET ---
[yaml packet here]
--- END CONTEXT PACKET ---
```

### Receiving Results

Background agents send results back automatically. The orchestrator:
1. Waits for all generation tasks to complete
2. Verifies each task produced its expected output file(s)
3. Dispatches evaluation in waves (H1 + H2 parallel, then H3 cross-reference)
4. Fixes any issues flagged by the evaluation waves
5. Assembles and delivers

---

## Complete Agent Roster

### Research Agents (Phase 1 — parallel with interview)

| Agent | Trigger | Reads | Returns |
|-------|---------|-------|---------|
| Competitor Research | Always (greenfield) | User-provided URLs or web search | 2-3 competitor analyses with palette, typography, layout, and differentiation notes |

### Generation Agents (Phase 3 — logical role model)

```
Phase 1 (Sequential — must finish first):
  └── Interview + Token Synthesis
        Reads: interview_framework.md, generation_flow.md, aesthetic_directions.md
        Outputs: context packet (schemas/agent_context_packet.md)

Phase 3 (Parallel — all independent, all consume the context packet):
  ├── Agent A: CSS Token File
  │     tokens.css + dark theme @media block
  │
  ├── Agent B: Visual Preview Dashboard
  │     design-system.html
  │
  ├── Agent C: Component Preview Files
  │     preview/*.html — one batch of previews per invocation
  │
  ├── Agent D: UI Kit — Marketing Surface
  │     ui_kits/marketing/ (README.md, index.html, components)
  │
  ├── Agent E: UI Kit — Dashboard Surface
  │     ui_kits/dashboard/ (README.md, index.html, components)
  │
  ├── Agent F: UI Kit — Docs Surface
  │     ui_kits/docs/ (README.md, index.html, components)
  │
  └── Agent G: Documentation
        README.md + DECISIONS.md + project-level SKILL.md

Phase 4 (Wave-Based — after all generation agents complete):
  └── Agent H1 + H2 (parallel), then H3
        Runs evaluation_checklist.md + ai_slop_detection.md against all outputs

Phase 4 Preprocessing (Before H3 runs):
  └── Agent H0 (or orchestrator): Token name validation
        Grep all generated output for token names, validate against canonical dictionary from tokens.css
        Flag unknown tokens, undefined references, or inconsistent naming before H3 cross-reference verification

Phase 4b: Framework Adaptation (Optional — only when framework_targets specifies a framework):
  └── Agent M: Framework Converter
        Reads generated HTML output + schemas/framework_targets.md
        Converts HTML components to target framework format (React, Vue, Astro, or Svelte)
        Produces src/ directory with component files, package.json, and barrel exports
        Triggered only when context packet has constraints.framework set to
        react, vue, astro, or svelte (not "html" or empty)
        Does NOT run when framework is html or unspecified (default HTML output is
        already complete after Phase 3)

Phase 5 (Sequential — orchestrator assembles):
  └── Gather results, verify cross-references, deliver
```

### Recommended Operational Grouping

For most real executions, group the logical roles into these workstreams:

| Grouped Task | Logical Roles Covered | Output Scope |
|--------------|-----------------------|--------------|
| Task 1: Core System | A + B + C | `tokens.css`, `design-system.html`, `preview/*` |
| Task 2: Surfaces | D/E/F as needed | Requested `ui_kits/*` only |
| Task 3 (G): Documentation | G | `README.md`, `DECISIONS.md`, `SKILL.md` |
| Task 4 (H1, H2): Evaluation | H1 + H2 (parallel) | First-pass validation reports |

Run H3 cross-reference verification separately only when needed.

Optional Task 5 (Framework Adaptation -- only when `constraints.framework` specifies a non-HTML target):
  M
  Converts generated HTML to React/Vue/Astro/Svelte components.

This grouped mode is the best default for typical runs because it preserves
specialization while avoiding excessive orchestration overhead.

### Audit Agents (standalone mode — not part of greenfield flow)

| Agent | Trigger | Reads | Returns |
|-------|---------|-------|---------|
| Visual Audit | User requests audit | workflow/visual_audit.md, target URL or codebase | 10-dimension scorecard with file:line fixes |
| AI Slop Check | User requests slop check | references/ai_slop_detection.md, target files | Slop findings with severity and antidotes |
| Existing Project Extraction | User has existing codebase | workflow/existing_project_extraction.md, codebase | Extracted token set, gap analysis, design system |

---

## Agent Input Specifications

### Agent A: CSS Tokens
| Field            | Value                                                        |
|------------------|--------------------------------------------------------------|
| Required inputs  | Context packet: brand, colors, typography, spacing, radii, shadows, motion, constraints |
| Files to read    | schemas/token_schema.md (sections 1–3: Raw Palette, Semantic Mapping, Dark Mode), schemas/theming_schema.md (sections 1–2: Variable Format, Three-Layer Architecture) |
| Output files     | tokens.css                                          |
| Quality checks   | All tokens defined, no hardcoded values, contrast ratios pass WCAG AA, dark mode remaps complete |

### Agent B: Preview Dashboard
| Field            | Value                                                        |
|------------------|--------------------------------------------------------------|
| Required inputs  | Context packet: all sections                                 |
| Files to read    | schemas/token_schema.md, schemas/output_schemas.md (Preview Dashboard Structure) |
| Output files     | design-system.html                                           |
| Quality checks   | All sections populated, scrollspy works, responsive sidebar, interactive states |

### Agent C: Component Previews
| Field            | Value                                                        |
|------------------|--------------------------------------------------------------|
| Required inputs  | Context packet: brand, colors, typography, spacing, radii, shadows, iconography |
| Files to read    | schemas/token_schema.md, schemas/output_schemas.md (Preview Files) |
| Output files     | preview/*.html (one file per component/foundation element)    |
| Quality checks   | Each file standalone-browsable, all states rendered, no hardcoded values |

### Agent D: Marketing UI Kit
| Field            | Value                                                        |
|------------------|--------------------------------------------------------------|
| Required inputs  | Context packet: brand, colors, typography, voice, surfaces.marketing, aesthetic |
| Files to read    | schemas/token_schema.md, schemas/output_schemas.md (UI Kit Structure), foundations/components/buttons.md, foundations/components/cards.md, foundations/components/navigation.md, foundations/components/section_layouts.md, foundations/layout_compositions.md |
| Output files     | ui_kits/marketing/README.md, ui_kits/marketing/index.html    |
| Quality checks   | Voice-appropriate copy, signature look preserved, interactive states, responsive |

### Agent E: Dashboard UI Kit
| Field            | Value                                                        |
|------------------|--------------------------------------------------------------|
| Required inputs  | Context packet: brand, colors, typography, surfaces.dashboard, constraints |
| Files to read    | schemas/token_schema.md, schemas/output_schemas.md (UI Kit Structure), foundations/data_visualization.md, foundations/components/buttons.md, foundations/components/cards.md, foundations/components/tables.md, foundations/components/modals.md |
| Output files     | ui_kits/dashboard/README.md, ui_kits/dashboard/index.html    |
| Quality checks   | Dense but scannable layout, chart colors accessible, data-dense patterns correct |

### Agent F: Docs UI Kit
| Field            | Value                                                        |
|------------------|--------------------------------------------------------------|
| Required inputs  | Context packet: brand, colors, typography, voice, surfaces.docs, constraints |
| Files to read    | schemas/token_schema.md, schemas/output_schemas.md (UI Kit Structure), foundations/content_design.md, foundations/components/buttons.md, foundations/components/cards.md, foundations/components/navigation.md, foundations/components/breadcrumbs.md |
| Output files     | ui_kits/docs/README.md, ui_kits/docs/index.html              |
| Quality checks   | Readable long-form, code blocks styled, nav works, search present |

### Agent G: Documentation
| Field            | Value                                                        |
|------------------|--------------------------------------------------------------|
| Required inputs  | Context packet: brand, aesthetic, colors, typography, spacing, voice, surfaces, constraints, substitutions |
| Files to read    | schemas/output_schemas.md (README.md Template, DECISIONS.md Template, Project-Level SKILL.md Template) |
| Output files     | README.md, DECISIONS.md, SKILL.md                            |
| Quality checks   | All sections covered, substitution flags present, file index accurate |

### Agent H: Evaluation Audit (Wave-Based)

Agent H is the most context-expensive agent because it must read all generated outputs (45,000–85,000 tokens) plus reference files. To prevent context overflow, evaluate in waves:

#### Wave H1: Core Deliverables
| Field            | Value                                                        |
|------------------|--------------------------------------------------------------|
| Required inputs  | Context packet (for reference), tokens.css, design-system.html, README.md, DECISIONS.md, SKILL.md |
| Files to read    | schemas/evaluation_checklist.md (sections 1–5), references/ai_slop_detection.md |
| Evaluates        | Token consistency, contrast verification, dashboard completeness, documentation quality |
| Additional task  | **Distinctiveness Verification** — Run the coherence check from generation_flow.md Step 9: verify non-default font, brand-derived accent, asymmetric section, and creative_brief propagation (4 requirements, all must pass) |
| Output           | Pass/fail report for core deliverables + distinctiveness verification results |

#### Wave H2: Previews + UI Kits
| Field            | Value                                                        |
|------------------|--------------------------------------------------------------|
| Required inputs  | Context packet (for reference), all preview/*.html files, all ui_kits/ files |
| Files to read    | schemas/evaluation_checklist.md (sections 6–9), references/ai_slop_detection.md |
| Evaluates        | Component accessibility, brand consistency, responsiveness, anti-slop |
| Output           | Pass/fail report for previews and UI kits |

#### Wave H3: Cross-Reference Verification (Diff-Based)
| Field            | Value                                                        |
|------------------|--------------------------------------------------------------|
| Required inputs  | Context packet + pre-extracted token diffs (NOT full generated files) |
| Files to read    | validation/css_validation.md, validation/accessibility_smoke_test.md |
| Evaluates        | Token cross-references, file index accuracy, substitution flag completeness |
| Output           | Final integration report |

H3 does NOT read all generated files. Instead, the **orchestrator must perform grep
extraction BEFORE dispatching H3** and pass the structured results:

1. **`DEFINED_TOKENS`**: The orchestrator extracts all custom property declarations
   from `tokens.css`:
   ```
   grep -oP '^\s*--([a-z][a-z0-9-]+)\s*:' tokens.css | sort -u
   ```
2. **`REFERENCED_TOKENS`**: The orchestrator extracts all `var(--*)` references from
   generated HTML files:
   ```
   grep -roh 'var(--[a-z][a-z0-9-]*)' design-system.html preview/ ui_kits/ | sort -u
   ```
3. **`MISSING_TOKEN_DIFF`**: Compute `REFERENCED_TOKENS - DEFINED_TOKENS` — these are
   undefined references (Critical failures).
4. **`UNUSED_TOKEN_SAMPLE`**: Optional sample of `DEFINED_TOKENS - REFERENCED_TOKENS`
   for cleanup suggestions.
5. **`FILE_INDEX_DIFF`**: Compare README.md's file tree against actual generated paths:
   ```
   grep -oP '`[^`]+`' README.md | tr -d '`' | sort -u
   ```
   Then diff against the output of `find . -type f | sort`.

H3 receives only these diffs plus small surrounding code snippets when diagnosis
is needed. H3 diagnoses the diff, proposes exact fixes, and verifies substitution
flag completeness — all without reading the full file set.

This prevents context overflow (H3 reads ~2KB of diffs instead of ~80KB of generated
files) while ensuring complete coverage.

Dispatch H1 and H2 in parallel (they evaluate independent file sets). Dispatch H3
after both complete. The orchestrator performs grep extraction between H1/H2 completion
and H3 dispatch.

### Agent M: Framework Adaptation (Optional Phase 4b)

Triggered only when `constraints.framework` in the context packet specifies react, vue, astro, or svelte. Does NOT run for the default html output.

| Field            | Value                                                        |
|------------------|--------------------------------------------------------------|
| Required inputs  | Context packet (framework field), all generated HTML files from Phase 3 |
| Files to read    | schemas/framework_targets.md, generated design-system.html, preview/*.html, ui_kits/*/index.html |
| Output files     | src/components/*.(tsx\|vue\|astro\|svelte), src/index.ts, src/hooks/useTheme.ts (React) or src/composables/useTheme.ts (Vue), package.json, components.css |
| Quality checks   | All HTML components have a framework equivalent, tokens.css shared correctly, TypeScript types present, barrel exports complete |

**Prompt template for Agent M:**

```
You are converting a generated HTML design system to [FRAMEWORK] components.

Read schemas/framework_targets.md for the target framework conventions.
Read the following generated files to extract components:
- design-system.html (all component sections)
- preview/*.html (isolated component markup)
- ui_kits/*/index.html (surface prototypes)

For each distinct component (Button, Card, Input, Badge, Modal, Nav, etc.),
create a [FRAMEWAY_EXTENSION] file following the patterns in framework_targets.md.

Generate:
1. src/components/ — one file per component with typed props
2. src/index.ts — barrel export
3. components.css — shared component styles using CSS variables
4. package.json — package configuration

All components must:
- Use CSS custom properties from tokens.css (import, not copy)
- Have TypeScript-typed props with documented defaults
- Follow framework conventions (className for React, :class for Vue, class:list for Astro, $props for Svelte)
- Preserve the signature moment and character_rules from the HTML originals

--- CONTEXT PACKET ---
{{CONTEXT_PACKET}}
--- END CONTEXT PACKET ---
```

---

## Dependency Graph

```
Phase 1 (Interview + Synthesis)
        │
        ▼
  [Context Packet]
        │
        ├──────────┬──────────┬──────────┬──────────┬──────────┬──────────┐
        ▼          ▼          ▼          ▼          ▼          ▼          ▼
   Agent A     Agent B     Agent C     Agent D     Agent E     Agent F     Agent G
   (CSS)       (Dashboard) (Previews)  (Marketing) (Dashboard) (Docs)     (Docs)
                                                                              │
                                                                              │
        ┌─────────────────────────────────────────────────────────────────────┘
        │ (all generation agents complete)
        ▼
   ┌─────┴─────┐
   │ H1 (Core) │     ┐
   └───────────┘     │ (parallel)
   ┌───────────┐     │
   │ H2 (Kits) │     ┘
   └───────────┘
        │
        ▼
   ┌───────────┐
   │ H3 (XRef) │
   └───────────┘
        │
        ▼
   ┌───────────────────────┐
   │ Phase 4b: Agent M     │  (optional — only when framework_targets set)
   │ Framework Adaptation  │
   └───────────────────────┘
        │
        ▼
   Phase 5 (Assemble)
```

All generation agents (A-G) are independent. They share one input: the context packet.
No agent reads another agent's output during generation.

Evaluation waves (H1/H2/H3) run after all generation agents complete and read their outputs.

---

## Context Packet Distribution

The context packet (defined in `schemas/agent_context_packet.md`) is the sole
serialization mechanism between the orchestrator and agents.

**Distribution rules:**

1. The context packet is self-contained. No agent needs to read the full skill
   or any foundation file beyond what its task template specifies.

2. The packet contains raw values (hex colors, pixel sizes, font names), not
   CSS variable references. Agents need the actual data to generate files.

3. By default, each agent receives a sliced packet plus a task-specific prompt
   template from `schemas/agent_task_templates.md`. Use per-agent slicing from
   `schemas/agent_context_packet.md`; only Agent G receives the full packet.
   The template tells the agent which fields matter most and which schema files
   to read for output format.

4. The packet replaces the `{{CONTEXT_PACKET}}` placeholder in each task template
   before dispatch.

5. **All agents generating UI (A, B, C, D, E, F) must follow the
   `components.component_style_contract` exactly.** This contract specifies exact
   CSS values for shared components (button hover/transition, card border/elevation,
   nav active indicator, input focus ring). It is more prescriptive than
   `character_rules` and takes precedence when the two conflict. The contract
   prevents cross-agent visual drift — without it, independent agents invent
   different hover colors, focus ring widths, and card borders for the same
   component type, producing a system that looks inconsistent across surfaces.

6. **Mandatory DNA propagation fields.** Before Phase 3 dispatch, verify that
   every agent prompt (A through F) includes: `creative_brief`, `signature_dna`,
   `components.character_rules`, `density_system`, the full `typography` block,
   and the full `colors` block. These fields MUST NOT be sliced out of any agent's
   packet. Without them, agents cannot propagate the system's visual identity into
   their deliverables, producing generic output that lacks brand coherence.

**Packet generation happens in Phase 2, after synthesis.** The orchestrator
serializes all interview answers, derived tokens, and design decisions into
the packet format. If the packet exceeds ~300 lines, trim prose descriptions
but keep all numeric values and token definitions.

---

## Merge Strategy

When assembling outputs in Phase 5, follow these rules:

### File naming conventions
- CSS tokens: `tokens.css` (root)
- Preview dashboard: `design-system.html` (root)
- Previews: `preview/{element}.html` (e.g., `preview/buttons.html`)
- UI kits: `ui_kits/{surface}/index.html`, `ui_kits/{surface}/README.md`
- Documentation: `README.md` (root), `DECISIONS.md` (root), `SKILL.md` (root)
- No two agents write to the same file path

### Token reference consistency
- All agents work from the same decision source (context packet + packet slices),
  so token names are consistent by construction
- Phase 5 verifies token references with extraction, not by loading every file
  into an agent. The orchestrator extracts:
  - `DEFINED_TOKENS`: declarations in `tokens.css`
  - `REFERENCED_TOKENS`: every unique `var(--*)` in generated files
  - `MISSING_TOKEN_DIFF`: referenced tokens missing from `tokens.css`
  - `FILE_INDEX_DIFF`: generated paths not represented in README, and README paths that do not exist
  H3 receives only these diffs plus small surrounding snippets when diagnosis is needed.

### Conflict resolution
- If two agents produce conflicting content, the context packet wins
- If shared components differ across `design-system.html`, `preview/*`, and
  `ui_kits/*`, `components.component_style_contract` wins. Patch the divergent
  output rather than reinterpreting `character_rules`.
- If Agent H flags an accessibility or consistency issue, fix it in the
  offending file and re-verify dependent cross-references

### Substitution flags
- Agent G collects substitution flags from the context packet and documents
  them in README.md
- Phase 5 verifies every flagged substitution is documented and no unflagged
  inferred values exist

---

## Error Recovery and Robustness

### Agent Failure Handling

When an agent fails (timeout, truncated output, missing fields), follow this protocol:

| Failure Type | Detection | Recovery |
|-------------|-----------|----------|
| Timeout (>5 min) | Agent returns no output | Retry once with same context packet. If second failure, log error and continue with remaining agents. |
| Truncated output | Output file missing expected sections or ends mid-tag | Retry once with explicit instruction to complete. If still truncated, mark output as partial and flag in assembly phase. |
| Missing context packet field | Agent output references undefined tokens | Block dispatch for required fields. For recommended fields, infer from the selected aesthetic origin or color narrative and record the inference in `substitutions`; never fall back to generic blue/green/system-ui defaults. |
| Validation failure | Output fails quality checks | Agent H (Evaluation) reports specific file:line failures. Fix in main chat with targeted edits — never re-run entire agent. |
| File write conflict | Two agents write same path | This should not happen (architecture prevents it). If detected, rename with suffix and flag for manual merge. |

### Partial Delivery Protocol

If 1-2 agents fail after retry:
1. Deliver all successful outputs immediately
2. Clearly list which outputs are missing
3. Offer to retry failed agents individually
4. Never block delivery of complete outputs on partial failures

If 3+ agents fail:
1. Halt delivery
2. Report the common failure mode (likely a context packet issue)
3. Ask the user whether to retry with a simplified packet or debug

### Agent Timeout Defaults

| Agent Type | Timeout | Rationale |
|-----------|---------|-----------|
| Research (I) | 8 min | Web fetching may be slow |
| Generation (A-G) | 8 min | Generation of complete design system deliverables; large output requires additional time |
| Evaluation (H) | 5 min | Reading + checking existing files |
| Audit (J, K) | 5 min | Reading + scoring |
| Framework Adaptation (M) | 8 min | Reading all HTML + converting to framework format |

### Context Packet Validation

Before dispatching agents, the orchestrator must validate the context packet:

```
Required fields (block dispatch if missing):
  - brand.name
  - colors.raw_palette.accent (valid hex)
  - colors.raw_palette.neutral_dark (valid hex)
  - typography.families.display (non-empty font name)
  - typography.families.body (non-empty font name)
  - spacing.base_unit

Required fields for generation dispatch:
  - brand.name (non-empty)
  - colors.raw_palette.accent (valid hex or token reference)
  - typography.families.display (named font)
  - typography.families.body (named font, different family from display)
  - spacing.base_unit (number, px or rem)
  - creative_brief (object, non-empty)
  - creative_brief.risk_dial (one of: safe, elevated, bold, experimental)
  - signature_anchor (non-empty string, must not be "clean and modern", "minimal", or other generic phrases)
  - character_rules.buttons (non-empty string)
  - character_rules.cards (non-empty string)
  - character_rules.inputs (non-empty string)
  - character_rules.navigation (non-empty string)
  - character_rules.sections (non-empty string)
  - component_style_contract.buttons.primary_hover
  - component_style_contract.cards.default
  - component_style_contract.inputs.focus
  - component_style_contract.navigation.active
  - tension_points.implementation populated when tension_points has prose
  - signature_dna has >= 3 entries OR signature_moment.systemic_effects names >= 3 component categories

If any creative field is missing or generic, the orchestrator must return to the interview/synthesis step rather than proceeding to generation. Generic signature_anchor phrases that fail validation: "clean", "modern", "minimal", "professional", "elegant" (without specifics).

Recommended fields (warn but continue if missing):
  - brand.audience
  - colors.accent
  - typography.mono
  - motion.duration
  - radii.base

Optional fields (no warning if missing):
  - surfaces (defaults to "marketing")
  - constraints.framework (defaults to "html")
  - voice.profile (defaults to "Direct/Technical")
```

If required fields are missing, return to Phase 1 interview to gather them. Never dispatch agents with incomplete required fields.
If creative fields (`creative_brief`, `signature_anchor`, `signature_dna`, `components.character_rules`,
or `imagery`) are missing in a full/medium build, pause synthesis and derive them
from the divergence step before dispatch. These fields prevent cross-agent visual drift.

---

## Runtime Modes

Choose the execution mode that matches your runtime's capabilities.
These modes target the same quality bar, but differ in orchestration overhead and
parallelism. For most projects, Mode 2 is the best default.

### Mode 1: Full Parallel (logical fanout)

When your runtime supports parallel subagent dispatch (Claude Code Task tool, Cursor
composer, etc.), dispatch all independent agents in a single message with multiple
tool calls. Each agent reads only its required files and returns its output.

**Best for:** Very large systems, or runtimes where many parallel tasks are cheap.

### Mode 2: Medium / Grouped Tasks (recommended default)

When your runtime supports task/subagent dispatch and you want strong output with less
orchestration overhead, group the system into 2-3 workstreams:

```
Task 1 (Core System):
  A + B + C
  tokens.css + design-system.html + preview/*

Task 2 (Surfaces):
  D/E/F as needed
  requested ui_kits/*

Task 3 (Docs + Light Validate):
  G + H1/H2-lite
  README.md + DECISIONS.md + SKILL.md + first-pass validation notes

Optional follow-up:
  H3 cross-reference verification if integration risk is high
```

This mode keeps the logical role specialization but dramatically cuts prompt-management
overhead. It covers the majority of real design-system requests well.

**Best for:** Most real projects.

### Mode 3: Sequential (one task, all deliverables)

When your runtime does NOT support parallel agents, or when context is very tight,
use a single agent that processes deliverables sequentially:

1. **Dispatch one agent** with the context packet + a combined prompt listing all
   deliverables in priority order.
2. The agent generates files one at a time: CSS tokens first, then dashboard,
   then previews, then UI kits, then documentation.
3. After each file, the agent briefly confirms completion before moving to the next.

**Combined prompt template:**

```
You are generating a complete design system sequentially. Produce each deliverable
in order. After completing each file, confirm and proceed to the next.

Priority order:
1. tokens.css (read schemas/token_schema.md + schemas/theming_schema.md)
2. design-system.html (read schemas/token_schema.md + schemas/output_schemas.md)
3. preview/*.html files (read schemas/token_schema.md + schemas/output_schemas.md)
4. UI kits per surface (read relevant foundation files + output_schemas.md)
5. README.md + DECISIONS.md + SKILL.md (read schemas/output_schemas.md)

After all files are generated:

Step 6 — Self-evaluate using schemas/evaluation_checklist.md and
references/ai_slop_detection.md. Report any failures.

Step 7 — H3-lite inline token cross-reference (no orchestrator in sequential mode):
  a. Collect every custom property declaration from tokens.css:
     DEFINED = all `--*:` lines in tokens.css
  b. Collect every `var(--*)` reference across all generated HTML files.
     REFERENCED = all unique var(--name) tokens found
  c. Compute MISSING = REFERENCED − DEFINED
  d. If MISSING is non-empty: add the missing token definitions to tokens.css
     before reporting complete. Never leave undefined references in the output.
  e. Report MISSING tokens fixed (or "none missing") as the final step.

--- CONTEXT PACKET ---
{{CONTEXT_PACKET}}
--- END CONTEXT PACKET ---
```

**Best for:** Windsurf, Cline, or any sequential-only runtime.

### Context Budget Guidance

Before choosing a mode, estimate context cost:

| Signal | Use Mode |
|--------|----------|
| Context packet > 250 lines | Mode 2 or 3 |
| Runtime limits to 2-3 concurrent agents | Mode 2 |
| No parallel agent support | Mode 3 |
| All agents available, packet < 200 lines | Mode 1 |

If the packet exceeds ~300 lines, trim prose descriptions but keep all numeric
values and token definitions.

### Runtime-Specific Notes

| Runtime | Recommended Mode | Notes |
|---------|-----------------|-------|
| Claude Code | Mode 2 | Grouped tasks are usually the best tradeoff |
| Cursor | Mode 2 | Grouped parallel blocks reduce orchestration overhead |
| Windsurf | Mode 3 | Cascade is sequential |
| Cline | Mode 3 | Sequential only |
| Generic | Mode 2 or 3 | Start with 2, fall back to 3 |
