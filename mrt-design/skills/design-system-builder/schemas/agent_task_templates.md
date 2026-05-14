# Agent Task Templates

Prompt templates for each parallel agent role. Each template is a complete,
copy-paste-ready prompt. Replace `{{CONTEXT_PACKET}}` with the serialized
context packet before dispatching the agent.

## Output Persistence Contract

Unless your runtime specifies another mechanism, every generation template should follow
this file-output rule:

1. Write files directly to the exact paths named in the template if the runtime permits file writes.
2. If the runtime cannot write files, return the complete file contents grouped by path:
   - `FILE: path/to/file`
   - full file contents
3. Never return partial fragments when a full file is expected.
4. Do not invent alternate filenames or directories.

---

## Component Style Contract -- Copy Verbatim

Every UI-generating agent (A, B, C, D, E, F) MUST copy the following
`component_style_contract` fields verbatim from the context packet into their output CSS.
Do not reinterpret, approximate, or invent values for these fields:

```
components.component_style_contract:
  radius_language          # pill | rounded | sharp | mixed
  elevation_language       # flat | floating | layered
  border_style             # full | underline | none
  density                  # compact | comfortable | spacious
  button_primary.hover_color
  button_primary.border_treatment
  button_primary.transition
  button_focus.ring_color
  button_focus.ring_width
  button_focus.ring_offset
  button_disabled.opacity
  button_disabled.cursor
  button_disabled.pointer_events
  card_default.border
  card_default.hover_effect
  card_default.elevation
  card_featured.border
  card_featured.hover_effect
  card_featured.elevation
  input_default.border
  input_default.background
  input_default.radius
  input_focus.ring_color
  input_focus.ring_width
  input_focus.ring_offset
  input_error.border_color
  input_error.ring_color
  nav_active.indicator_style
  nav_active.color
  nav_active.background
  nav_mobile.breakpoint
  nav_mobile.treatment
  sections.rhythm
  sections.divider
```

If any field is empty in the packet, use the defaults from `schemas/packet_validation.md`
rather than inventing a value. These shared values prevent cross-agent visual drift.

---

## Context Discipline

Read only the files explicitly listed in the task template unless you are
blocked by a missing definition. Extra reference reads usually reduce output
quality by blowing the context budget. If blocked, read the smallest relevant
section and state which gap required it.

---

## Agent A: CSS Token Agent

**Role:** Generates the CSS custom properties file and dark theme variant.
**Input:** Context packet + token_schema.md + theming_schema.md
**Output:** `tokens.css`
**Schema:** schemas/token_schema.md

### Prompt Template

```
You are generating the CSS token file for a design system.
Write the result to `tokens.css`. If direct file writing is unavailable, return the
complete contents for `tokens.css` and nothing else.

Read schemas/token_schema.md (Canonical Token Dictionary: Raw Palette, Semantic Mapping,
Typography, Spacing, Shadows, Motion, Radius, Density — plus CSS Variable Architecture
for the three-layer naming system), schemas/theming_schema.md
(Theme Architecture, Light Theme, Dark Theme, High Contrast Theme — for multi-mode
token overrides), and
references/typography_selection.md if `typography.language_strategy` is present
or the packet is multilingual/non-Latin/typography-led.

Use the design decisions below to produce tokens.css. The file must:
1. Define tokens only. Do not use CSS `@import` for fonts; HTML deliverables load fonts via `<link>` or the project font strategy.
2. Define a :root block with ALL tokens organized by section:
   - Color (raw palette using evocative names from raw_names, then semantic mapping)
   - Typography (families, language/script strategy, expressive roles, all scale levels with size/weight/line-height/letter-spacing, weights)
   - Spacing (all 10 scale values, container max, nav heights)
   - Radii (sm through full)
   - Shadows (sm, md, lg, accent)
   - Motion (easing, durations)
   - Layout (container max, nav heights)
3. Add semantic type classes (.t-display, .t-h1, .t-h2, .t-h3, .t-h4, .t-body-lg, .t-body, .t-body-sm, .t-label, .t-mono)
4. Add base element styles (html, body, a, ::selection, :focus-visible)
5. Add reduced motion media query
6. If dark mode is supported, use the canonical pattern only: `[data-theme="dark"]`
   for explicit theme selection plus `@media (prefers-color-scheme: dark)` with
   `:root:not([data-theme])` as the OS fallback. Do not use `.dark`.
7. **Theme mode enforcement:** The packet's `constraints.theme_modes` lists every
   mode that MUST be generated (e.g., `["light", "dark"]` or
   `["light", "dark", "high-contrast"]`). You MUST generate ALL listed modes.
   Missing any requested mode is a generation failure — do not skip dark mode or
   high-contrast "if supported" or "when convenient." Every mode in the array
   must have a complete set of semantic token overrides. If `high-contrast` is
   listed, it must remap all semantic colors to meet WCAG AAA (7:1 for normal text).

Every value must come from the packet below. No hardcoded values. No placeholders.

--- CONTEXT PACKET ---
{{CONTEXT_PACKET}}
--- END CONTEXT PACKET ---

Quality checks before returning:
- Every semantic token has a raw source
- Contrast ratios pass WCAG AA for all fg/bg combinations
- Dark mode remaps all semantic color tokens
- Every mode listed in `constraints.theme_modes` has a complete token override block (FAIL if any requested mode is missing)
- High-contrast mode (if listed) achieves WCAG AAA 7:1 contrast for all text pairs
- No CSS variable is undefined or missing
- Signature-moment treatments are encoded where relevant
- Type scale and line-height respect `typography.language_strategy.script_system`
- Font choices and semantic type classes reflect `typography.expressive_roles`
- `components.component_style_contract` is reflected in shared component tokens/classes
- `tension_points.implementation` is represented as concrete CSS/token consequences
- `creative_brief`, `tension_points`, dark-mode strategy, and component character rules are reflected in token choices
```

---

## Agent B: Preview Dashboard Agent

**Role:** Generates the visual preview dashboard — a single browsable HTML page.
**Input:** Context packet + token_schema.md + output_schemas.md (Preview Dashboard Structure)
**Output:** `design-system.html`
**Schema:** schemas/output_schemas.md (Preview Dashboard Structure section)

### Prompt Template

```
You are generating the visual preview dashboard for a design system.
Write the result to `design-system.html`. If direct file writing is unavailable,
return the complete contents for `design-system.html`.

Read schemas/token_schema.md (sections 1–2: Raw Palette, Semantic Mapping)
for variable naming and schemas/output_schemas.md (Preview Dashboard Structure
section only) for required sections and technical requirements.

Produce design-system.html — a single browsable page with:
1. Sticky top bar with brand name and version
2. Sidebar navigation (organized by category, with scrollspy)
3. Main content sections in this order:
   - Overview (system metadata, fonts, icons, grid, version)
   - Color (swatch cards with token name and hex value for every color token)
    - Typography (live specimens for every type level with metrics, language/script rationale, and page-part usage)
   - Spacing (visual bars showing each scale step)
   - Radii (boxes at each radius level)
   - Shadows (elevation demonstrations)
   - Buttons (all variants: primary, secondary, outline, ghost, on-dark; all states: default, hover, focus, disabled)
   - Cards (card variants with hover states)
   - Forms (inputs, labels, helpers, error states)
   - Badges (badge/tag variants)
   - Navigation (nav bar preview)
   - Links to preview/ files and ui_kits/ prototypes

All styles must use the CSS custom properties from the token file. Inline the
token definitions in a <style> block so the file is standalone-browsable.
Load fonts and icons using the URLs/strategy in constraints only when provided.

The sidebar must collapse on mobile. The scrollspy must highlight the current
section. All interactive states (hover, focus, active) must be live.
Show the system's signature moment in at least one high-visibility section without
turning every section into the same effect.
Use `creative_brief`, `tension_points`, and `components.character_rules` to keep
the dashboard visually aligned with UI-kit agents.
In the typography section, show how the font system expresses the brand values:
display/body/mono rationale, language/script fit, page-part roles, and multilingual
fallbacks when present.

--- CONTEXT PACKET ---
{{CONTEXT_PACKET}}
--- END CONTEXT PACKET ---

Quality checks before returning:
- All sections from the schema are present and populated
- Scrollspy works
- Responsive sidebar
- No empty sections
- All tokens rendered from CSS variables
- Signature moment is visible and recognizable
- Typography specimens demonstrate script-appropriate metrics and expressive roles
```

---

## Agent C: Component Preview Agent

**Role:** Generates isolated component preview HTML files.
**Input:** Context packet + token_schema.md + output_schemas.md (Preview Files)
**Output:** `preview/*.html` (one file per component)
**Schema:** schemas/output_schemas.md (Preview Files section)

### Prompt Template

```
You are generating isolated component preview files for a design system.
Write each result to its exact path in `preview/`. If direct file writing is unavailable,
return complete file contents grouped by output path.

Read schemas/token_schema.md for variable naming and schemas/output_schemas.md
(the Preview Files section) for the file list and requirements.

Generate these preview files in preview/:
_card.css (shared preview card styles), colors-primary.html, colors-neutrals.html,
colors-semantic.html, type-display.html, type-body.html, type-mono.html,
spacing-scale.html, radii.html, shadows.html, buttons.html, buttons-on-dark.html,
form-inputs.html, cards.html, badges.html, nav-dark.html, logo-lockups.html,
iconography.html.

Each file must:
- Be self-contained HTML (inline the CSS variable definitions in a <style> block)
- Show ALL states: default, hover, focus, active, disabled, error (where applicable)
- Use representative content matching the brand voice (not "Lorem ipsum")
- Load fonts and icons using the URLs/strategy in constraints only when provided
- Be openable standalone in a browser
- Express the system's signature moment where appropriate for that component
- Apply `components.character_rules` instead of generic rounded-card defaults

--- CONTEXT PACKET ---
{{CONTEXT_PACKET}}
--- END CONTEXT PACKET ---

Quality checks before returning:
- Every file is standalone-browsable
- All states are shown for interactive elements
- No hardcoded values — all styles reference CSS variables
- Content matches the voice profile
- Signature moment is not absent from all previews
```

---

## Agent D: Marketing UI Kit Agent

**Role:** Generates the marketing surface UI kit — a standalone HTML prototype demonstrating the design system. Framework components (React, Vue, Svelte, etc.) are NOT generated here — they are defined in `schemas/framework_targets.md` for developers to implement.
**Input:** Context packet + token_schema.md + output_schemas.md + relevant component files + layout_compositions.md
**Output:** `ui_kits/marketing/` (README.md, index.html) — plain HTML/CSS files only
**Schema:** schemas/output_schemas.md (UI Kit Structure section)

### Prompt Template

```
You are generating the marketing UI kit for a design system.
Write outputs directly into `ui_kits/marketing/` using the exact filenames listed below.
If direct file writing is unavailable, return complete file contents grouped by path.

Read schemas/token_schema.md (sections 1–2), schemas/output_schemas.md
(UI Kit Structure section), foundations/components/buttons.md +
foundations/components/cards.md + foundations/components/navigation.md +
foundations/components/section_layouts.md for component specs, and
foundations/layout_compositions.md for page-level layout patterns.

Generate the marketing kit in ui_kits/marketing/:
- README.md (kit description, component map, usage instructions)
- index.html (interactive click-through prototype with all components)
- Individual component sections within the prototype: Nav, Hero, Features, Pricing,
  Testimonials, ContactForm, Footer (and any others listed in surfaces.marketing.components)

The prototype must:
- Be a single browsable HTML file with anchor-linked sections
- Use brand-appropriate copy matching the voice profile (no placeholder text)
- Preserve the signature look and aesthetic mood
- Include one deliberate signature moment tied to the packet's `signature_anchor`
- Propagate `signature_dna` rules into ordinary cards, buttons, dividers, and navigation
- Follow `components.component_style_contract` exactly for shared buttons, cards,
  inputs, navigation, and section rhythm so this kit matches dashboard/previews.
- Apply `tension_points.implementation` as CSS, not just visual language.
- Apply `rule_breaking_budget.allowed` only in the named locations and never in
  forms, tables, focus states, or accessibility-critical UI.
- Apply `imagery` rules to all placeholder or art-directed media areas
- Include interactive states: hover, focus, form inputs that accept input, mobile nav collapse
- Load fonts and icons using the URLs/strategy in constraints only when provided
- Inline CSS variable definitions for standalone browsing

--- CONTEXT PACKET ---
{{CONTEXT_PACKET}}
--- END CONTEXT PACKET ---

Quality checks before returning:
- Voice-appropriate copy throughout (no "Lorem ipsum")
- Signature look preserved
- Signature moment is present and not reduced to decoration
- All interactive states are live
- Responsive layout with mobile nav
- Component map in README matches actual files
- Shared components match `components.component_style_contract`
- Signature systemic effects appear in at least 3 component categories
```

---

## Agent E: Dashboard & Settings UI Kit Agent

**Role:** Generates the dashboard and settings/admin surface UI kits — click-through prototypes.
**Input:** Context packet + token_schema.md + output_schemas.md + data_visualization.md + relevant component files
**Output:** `ui_kits/dashboard/` and/or `ui_kits/settings/` (README.md, index.html, component files)
**Schema:** schemas/output_schemas.md (UI Kit Structure section)

### Supported Surface Types

| Surface | Directory | Typical Components |
|---------|-----------|-------------------|
| Dashboard | `ui_kits/dashboard/` | SidebarNav, HeaderBar, StatsCards, DataTable, Charts, ActivityFeed |
| Settings / Admin | `ui_kits/settings/` | FormGroups, ToggleSections, CRUDTable, PermissionMatrix, AuditTrail |

When `surfaces` includes "settings" or "admin", generate a settings UI kit (or
include settings patterns in the dashboard kit if the surfaces overlap).

### Prompt Template

```
You are generating the dashboard and/or settings UI kit for a design system.
Write outputs directly into `ui_kits/dashboard/` and/or `ui_kits/settings/`
using the exact filenames listed below. If direct file writing is unavailable,
return complete file contents grouped by path.

Read schemas/token_schema.md (sections 1–2), schemas/output_schemas.md
(UI Kit Structure section), foundations/data_visualization.md for chart colors
and KPI patterns, and foundations/components/buttons.md +
foundations/components/cards.md + foundations/components/tables.md +
foundations/components/modals.md for component specs.

Generate the dashboard kit in ui_kits/dashboard/ (when "dashboard" is in surfaces):
- README.md (kit description, component map, usage instructions)
- index.html (interactive click-through prototype)
- Component sections: SidebarNav, HeaderBar, StatsCards, DataTable, Charts,
  DeployHistory/ActivityFeed, SettingsPanel, EmptyState (per surfaces.dashboard.components)

Generate the settings/admin kit in ui_kits/settings/ (when "settings" or "admin" is in surfaces):
- README.md (kit description, component map, usage instructions)
- index.html (interactive click-through prototype)
- Component sections per surfaces.settings.components, including these patterns:

  **Form-group pattern:** Each setting uses a consistent structure:
  label + description text + input control + validation/error message.
  Groups are visually separated by borders or background alternation.
  Example: [Account Name] [text input] [Your public display name] [error: required]

  **Toggle section with save/discard:** Toggle switches, dropdowns, and inputs
  are grouped into sections. Each section has a footer with "Save" (primary)
  and "Discard" (ghost) buttons. Discard is disabled when no changes are pending.
  Changes should be tracked and the save button visually activated on modification.

  **Table-heavy CRUD interfaces:** Dense data tables with inline actions (edit,
  delete, duplicate), bulk selection checkboxes, and row-hover highlights.
  Include search/filter bar, pagination, and empty-state message.
  Column headers should be sortable (visual indicator).

  **Dense information layouts:** Multi-column settings pages using cards or
  fieldset-style groups. Use the design system's spacing scale to maintain
  readability at higher density. Labels left-aligned in a fixed-width column,
  inputs right-aligned.

  **Breadcrumb navigation:** Nested settings pages (e.g., Settings > Team >
  Permissions) use breadcrumb navigation at the top of the content area.
  Breadcrumbs link to parent levels. Current page is plain text.

  **Additional patterns to include:**
  - Destructive action zones (red-tinted section for delete account, reset data)
  - Permission/role matrix (table with checkboxes per role)
  - Audit trail / activity log (timestamped list with actor, action, target)
  - Empty state for new accounts with no settings configured
  - Loading skeleton for async settings fetch

The prototype must:
- Be a single browsable HTML file with a dashboard layout (sidebar + main content)
- Use realistic data matching the product domain (not generic placeholder data)
- Show dense but scannable information layouts
- Include chart visualizations styled with the brand's color tokens (dashboard only)
- Apply `density_system`, `tension_points`, and `components.character_rules` to keep dense UI distinctive but scannable
- Follow `components.component_style_contract` exactly for buttons, cards, inputs,
  navigation, table row hover/focus, and settings panels.
- Include settings/admin patterns when requested: grouped form sections, save/discard
  actions, destructive zones, audit trail, filters, bulk actions, and permission states.
- Load fonts and icons using the URLs/strategy in constraints only when provided
- Inline CSS variable definitions for standalone browsing

--- CONTEXT PACKET ---
{{CONTEXT_PACKET}}
--- END CONTEXT PACKET ---

Quality checks before returning:
- Chart colors are accessible (3:1 contrast against backgrounds)
- Data-dense patterns are correct (tables, stats, activity feeds)
- Interactive states on all controls (filters, toggles, search)
- Sidebar navigation works
- Responsive: sidebar collapses on smaller screens
- Shared components match `components.component_style_contract`
- Settings forms have consistent label-input-error structure
- Save/discard patterns are present and functional
- Breadcrumb navigation works for nested settings
- Destructive actions have confirmation pattern
```

---

## Agent F: Docs UI Kit Agent

**Role:** Generates the documentation surface UI kit — a click-through prototype.
**Input:** Context packet + token_schema.md + output_schemas.md + content_design.md + relevant component files
**Output:** `ui_kits/docs/` (README.md, index.html, component files)
**Schema:** schemas/output_schemas.md (UI Kit Structure section)

### Prompt Template

```
You are generating the documentation UI kit for a design system.
Write outputs directly into `ui_kits/docs/` using the exact filenames listed below.
If direct file writing is unavailable, return complete file contents grouped by path.

Read schemas/token_schema.md (sections 1–2), schemas/output_schemas.md
(UI Kit Structure section), foundations/content_design.md for voice and copy
patterns, and foundations/components/buttons.md + foundations/components/cards.md +
foundations/components/navigation.md + foundations/components/breadcrumbs.md
for component specs.

Generate the docs kit in ui_kits/docs/:
- README.md (kit description, component map, usage instructions)
- index.html (interactive click-through prototype)
- Component sections: SidebarNav, HeaderBar with search, ContentArea (prose + code blocks),
  CodeBlocks with syntax highlighting, APIReference, Callouts/Admonitions,
  TableOfContents, Breadcrumbs (per surfaces.docs.components, or standard set)

The prototype must:
- Be a single browsable HTML file with a docs layout (sidebar nav + content area)
- Use readable long-form typography with the body font at proper scale
- Style code blocks with the mono font and appropriate background
- Include working sidebar navigation with active section highlighting
- Show callout variants (note, warning, tip, danger)
- Match docs copy to the voice examples and `creative_brief`, not generic documentation prose
- Load fonts and icons using the URLs/strategy in constraints only when provided
- Inline CSS variable definitions for standalone browsing

--- CONTEXT PACKET ---
{{CONTEXT_PACKET}}
--- END CONTEXT PACKET ---

Quality checks before returning:
- Long-form content is readable (line length ~65-72ch, proper line height)
- Code blocks are styled and readable
- Sidebar navigation works with active highlighting
- Search input is present and styled
- Voice-appropriate documentation copy
```

---

## Agent G: Documentation Agent

**Role:** Generates README.md, DECISIONS.md, and project-level SKILL.md.
**Input:** Context packet + output_schemas.md
**Output:** `README.md`, `DECISIONS.md`, `SKILL.md`
**Schema:** schemas/output_schemas.md (README.md Template + DECISIONS.md Template + Project-Level SKILL.md Template)

### Prompt Template

```
You are generating the documentation files for a design system.
Write outputs directly to `README.md`, `DECISIONS.md`, and `SKILL.md`. If direct file
writing is unavailable, return complete file contents grouped by path.

Read schemas/output_schemas.md for the README.md Template, DECISIONS.md Template,
and Project-Level SKILL.md Template. Follow those structures exactly.

Generate three files:

1. README.md — Full system documentation with ALL sections:
   System overview (name, version, fonts, icon system)
   Brand essence (name, domain, tagline, audience, voice)
   Signature look (3-5 visual rules, the "do not break" constraints)
   Content fundamentals (tone, casing, length rules, example copy with good/bad)
   Visual foundations (color token table, typography language strategy, font rationale, expressive page-part roles, typography scale table, spacing, radii, shadows, borders, imagery, motion, layout rules)
   Iconography rules
   Project index (file tree showing all expected generated files)
   Caveats (substitution flags for ALL inferred values from the substitutions section)

2. DECISIONS.md — Design decision traceability log:
   Aesthetic direction (preset name, rationale, source)
   Color system (accent, neutrals, temperature — why each)
   Typography (fonts, primary language/script fit, expressive roles, brand values expressed, scale ratio — why these choices)
   Spacing & layout (base unit, density — why)
   Component styling (roundness, elevation, signature rules)
   Signature moment (concept, placements, constraints, why it improves memorability)
   Substitutions (every inferred value that needs replacement)
   For each major decision, include: ID, User Context, Principle, Accessibility, Research

3. SKILL.md — Project-level skill file (~30-50 lines, lean and actionable):
   YAML front matter with name, description, user-invocable: true
   Quick reference (CSS file, logos, icons, fonts, UI kit paths)
   Signature look rules (3-5 bullets)
   Router instructions (which files to read for what)

Use the brand name in headings. Write in the brand's voice profile.
Document every substitution flag from the substitutions section.

--- CONTEXT PACKET ---
{{CONTEXT_PACKET}}
--- END CONTEXT PACKET ---

Quality checks before returning:
- All README sections from the schema are covered
- Every substitution is flagged in the Caveats section
- File index lists all expected deliverables
- SKILL.md is under 50 lines
- Voice is consistent with the profile
```

---

## Agent H: Evaluation Agent (Wave-Based)

**Role:** Audits generated outputs in waves to prevent context overflow.
**Input:** Context packet + evaluation references + specific output files per wave
**Output:** Pass/fail report with specific fix instructions per wave
**Schema:** schemas/evaluation_checklist.md

### Wave H1: Core Deliverables

```
You are evaluating the core deliverables of a design system.

Read schemas/evaluation_checklist.md (sections 1–6: Accessibility, Token
Consistency, Theme Completeness, Brand Consistency, Content Quality, Responsiveness)
and references/ai_slop_detection.md. Also run the Distinctiveness Verification
(generation_flow.md Step 9): verify non-default font, brand-derived accent,
asymmetric section, and creative_brief propagation (4 requirements, all must pass).

Then evaluate these files:
- tokens.css
- design-system.html
- README.md
- DECISIONS.md
- SKILL.md

For each checkpoint, report:
- PASS: [brief note]
- FAIL: [specific file(s) and what is wrong]
- SKIP: [reason]

If any checkpoint fails, provide specific fix instructions (which file, what
the current value is, what it should be).

--- CONTEXT PACKET ---
{{CONTEXT_PACKET}}
--- END CONTEXT PACKET ---
```

### Wave H2: Previews + UI Kits

```
You are evaluating preview files and UI kits for a design system.

Read schemas/evaluation_checklist.md (sections 7–8, 10–11: Visual Preview,
Performance, Documentation, Beauty & Memorability) and
references/ai_slop_detection.md. Also run Anti-Slop Check (section 13).

Then evaluate these files:
- All files in preview/
- All files in ui_kits/marketing/ (if generated)
- All files in ui_kits/dashboard/ (if generated)
- All files in ui_kits/docs/ (if generated)

For each checkpoint, report:
- PASS: [brief note]
- FAIL: [specific file(s) and what is wrong]
- SKIP: [reason, e.g., surface not requested]

Focus on: component accessibility, brand consistency, responsive behavior,
anti-slop quality.

--- CONTEXT PACKET ---
{{CONTEXT_PACKET}}
--- END CONTEXT PACKET ---
```

### Wave H3: Cross-Reference Verification

```
You are verifying cross-references across all design system deliverables.

Read validation/css_validation.md and validation/accessibility_smoke_test.md
for checklists.

Do not read every generated file in full. The orchestrator must first provide:
- `DEFINED_TOKENS`: unique custom properties extracted from `tokens.css` declarations
- `REFERENCED_TOKENS`: unique `var(--*)` references extracted from generated files
- `MISSING_TOKEN_DIFF`: referenced tokens not present in `DEFINED_TOKENS`
- `UNUSED_TOKEN_SAMPLE`: optional sample of defined tokens never referenced
- `FILE_INDEX_DIFF`: README file-tree mismatches, if any

Then verify:
1. Token cross-references: diagnose `MISSING_TOKEN_DIFF` and propose exact fixes
2. File index accuracy: README.md's file tree matches actual generated files
3. Substitution flag completeness: every inferred value in the context packet
   has a corresponding flag in README.md
4. Decision traceability completeness: every major token/aesthetic decision has
   an entry in DECISIONS.md with ID, decision, rationale, source, user context,
   principle, accessibility, and research evidence (or explicit n/a)
5. CSS three-layer structure validation (raw → semantic → component)
6. Accessibility smoke test: contrast, focus states, ARIA, keyboard nav
7. Signature moment consistency: the documented signature moment appears in output files

Report any inconsistencies with specific file paths and fix instructions.

--- CONTEXT PACKET ---
{{CONTEXT_PACKET}}
--- END CONTEXT PACKET ---
```

---

## Agent I: Competitor Research Agent

**Role:** Researches 2-3 competitor/reference sites and extracts visual design patterns.
**Input:** Brand info (name, industry, audience) — lightweight, no full context packet needed.
**Output:** Structured competitor analysis with differentiation notes.

### Prompt Template

```
You are researching competitor websites for a design system project.

Brand: {{BRAND_NAME}}
Industry: {{INDUSTRY}}
Audience: {{AUDIENCE}}
Known competitors (if any): {{COMPETITORS}}

Tasks:
1. Identify 2-3 competitor or reference products in this space (use web search if not provided)
2. For each competitor, fetch their website and analyze:
   - Color palette (primary, accent, neutrals, temperature)
   - Typography (display font, body font, type scale approach)
   - Layout patterns (hero style, section rhythm, grid approach)
   - Component styling (button shape, card treatment, form styling)
   - Signature visual moments (what makes them recognizable)
   - Overall density and tone
3. Summarize findings as competitive positioning:
   - What patterns are common across competitors (table stakes)
   - What each competitor does uniquely
   - Where differentiation opportunities exist (gaps, underserved aesthetics)
   - One adjacent-category inspiration worth adapting
   - One candidate signature moment for this brand

**Fallback when web fetching fails:**
If you cannot fetch competitor websites (blocked, paywalled, or no web access):
1. Use the reference gallery in references/creative_inspiration.md — pick 2-3 sites
   from the same industry category and analyze their documented patterns instead.
2. If no industry match exists, skip to the mood matrix in references/aesthetic_synthesis.md
   and derive positioning from the brand's chosen aesthetic direction.
3. Never block Phase 2 synthesis on failed competitor research. Always return
   some form of competitive context, even if it's reference-gallery-based.
4. Focus on techniques that can be transformed into brand-specific output rather than copied literally.

Format as a structured analysis per competitor, then a differentiation summary.
Be specific — cite actual colors (hex), font names, and layout choices. Not vague impressions.
```

---

## Agent J: Visual Audit Agent

**Role:** Scores an existing UI across 10 dimensions with specific file:line evidence.
**Input:** Target URL or codebase path, or screenshot files.
**Output:** 10-dimension report card with scores, evidence, and prioritized fixes.

### Prompt Template

```
You are performing a visual audit of an existing interface.

Target: {{TARGET_URL_OR_PATH}}

Read workflow/visual_audit.md for the full 10-dimension scoring rubric and report format.

Steps:
1. Access the target (navigate to URL, or read codebase files)
2. Take screenshots at 375px, 768px, 1280px if possible
3. Score each of the 10 dimensions (0-10):
   - Color Consistency
   - Typography Hierarchy
   - Spacing Rhythm
   - Component Consistency
   - Responsive Behavior
   - Dark Mode / Theme Completeness
   - Motion & Animation
   - Accessibility
   - Information Density
   - Polish & Edge States

4. For each dimension:
   - Record the score
   - List 2-4 specific observations with file:line references where available
   - Provide fixes for each issue

5. Compile as a report card table, then list:
   - Critical issues (score ≤ 4)
   - Quick wins (easy fixes, high impact)
   - Strengths (what's working well)

6. Prioritize all fixes as P0 (fix now), P1 (next sprint), P2 (backlog), P3 (nice to have)

--- CONTEXT PACKET ---
{{CONTEXT_PACKET}}
--- END CONTEXT PACKET ---
```

---

## Agent K: AI Slop Detection Agent

**Role:** Scans files or a URL for generic AI-generated design patterns.
**Input:** Target files or URL.
**Output:** Slop findings with severity, specific evidence, and antidotes.

### Prompt Template

```
You are detecting generic AI-generated design patterns ("AI slop") in an interface.

Target: {{TARGET_URL_OR_PATH}}

Read references/ai_slop_detection.md for the full detection checklist.

Steps:
1. Access the target and examine: HTML structure, CSS, color choices, typography,
   layout patterns, component styling, motion, content copy
2. Run through every item in the detection checklist
3. For each detected pattern, report:
   - Anti-pattern name (from the checklist)
   - Severity: Critical / Major / Minor
   - Specific evidence (exact CSS values, HTML patterns, copy examples)
   - Antidote: what to change, referencing the relevant foundation or reference file

4. Summarize:
   - Overall slop severity: Clean / Moderate / Heavy
   - Top 3 most impactful fixes
   - Whether the design would pass as human-made to a design-literate audience

--- CONTEXT PACKET ---
{{CONTEXT_PACKET}}
--- END CONTEXT PACKET ---
```

---

## Agent L: Quick Mode Agent

**Role:** Handles single-component, single-page, or focused requests without the full pipeline.
**Input:** Quick context packet (trimmed) + task description
**Output:** Single deliverable (component HTML, token file, or page)
**Schema:** schemas/token_schema.md + schemas/output_schemas.md (relevant section)

### Prompt Template

```
You are generating a focused deliverable for a design system.
Write the requested deliverable directly to its target path when the task defines one.
If direct file writing is unavailable, return the complete file contents grouped by path.

Read schemas/token_schema.md for variable naming. Read the relevant section of
schemas/output_schemas.md for the output format.

Task: {{TASK_DESCRIPTION}}

This is a quick/single-deliverable request. Produce ONLY what is asked for.
Do not generate a full design system. Use the context below for design decisions.

--- CONTEXT PACKET ---
{{CONTEXT_PACKET}}
--- END CONTEXT PACKET ---

Quality checks before returning:
- All colors reference CSS variables, no hardcoded values
- Contrast passes WCAG AA for all text
- Font sizes follow a type scale
- Spacing uses a consistent base unit
- Interactive elements have focus states
- Touch targets are at least 44x44px
```
