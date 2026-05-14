# Iteration Workflow

How to modify a previously generated design system without regenerating everything.
Produces targeted diffs, not full rebuilds.

---

## When to Use This Workflow

- The user says "change the accent color to blue"
- The user says "make the headings bigger"
- The user says "add a new component"
- The user says "switch from light to dark-first"
- Any request to modify tokens or components in an existing design system

---

## Iteration Principles

1. **Diff, don't rebuild.** Change only the affected files. Never regenerate the
   entire system for a token change.
2. **Propagate token changes.** When a token value changes, trace every file that
   references it and update accordingly.
3. **Version increment.** After each iteration, bump the version in README.md.
4. **Changelog entry.** Document what changed and why.

---

## Iteration Flow

### Step 1: Identify Change Scope

| Change Type | Files Affected | Scope |
|------------|----------------|-------|
| Token value change | `tokens.css` + all files using that token | Medium |
| Token addition | `tokens.css` + `README.md` + `design-system.html` | Medium |
| Component change | Specific `preview/*.html` + relevant UI kit | Small |
| Aesthetic direction shift | All files (treat as regeneration) | Full |
| Typography language/script change | `tokens.css` + `README.md` + `DECISIONS.md` + typography previews + all text-heavy UI kits | Large |
| New surface added | New `ui_kits/{surface}/` + `README.md` | Large |
| Voice/tone change | `README.md` + all UI kit copy | Large |

### Step 2: Locate Target Files

Read the design system's file tree from `README.md` (Project Index section).
Identify which files reference the changed tokens.

For token changes, search across all files:
```
Files referencing --accent: tokens.css, design-system.html,
  preview/colors-primary.html, preview/buttons.html, preview/buttons-on-dark.html,
  all ui_kits/*/index.html, README.md
```

### Step 3: Apply Changes

Make targeted edits to affected files. Do not regenerate entire files.

**Token value change:**
1. Update the raw token in `tokens.css` (raw palette section)
2. If semantic mapping is affected, update the semantic section
3. If dark mode is affected, update the `[data-theme="dark"]` or `@media` block
4. Update `design-system.html` where the old value appears in inline styles or docs
5. Update relevant `preview/*.html` files

**Component change:**
1. Edit the specific preview file
2. Edit the UI kit file if the component appears there
3. No token changes needed

**Voice/tone change:**
1. Update `README.md` Content Fundamentals section
2. Update copy in all UI kit `index.html` files
3. No token changes needed

**Typography language/script change:**
1. Update `typography.language_strategy`, expressive roles, and font rationale in documentation
2. Update `--font-*`, type scale, line-height, and letter-spacing tokens in `tokens.css`
3. Update typography specimens and any multilingual examples in `design-system.html`
4. Update text-heavy UI kits (marketing, docs, dashboard tables/settings) for overflow, line-height, and fallback behavior
5. Re-run visual regression for script rendering and fallback stacks

### Step 4: Verify

After applying changes:
- Verify contrast ratios still pass WCAG AA (especially for color changes)
- Verify token references are consistent across all files
- Verify the preview dashboard renders correctly
- Spot-check 2-3 component previews

### Step 5: Update Metadata

1. Increment version in `README.md` header (e.g., v1.0 → v1.1)
2. Add a changelog entry (see format below)
3. Update `design-system.html` version display
4. Update `SKILL.md` quick reference if signature tokens changed

---

## Changelog Format

Append to `README.md` in a Changelog section:

```markdown
## Changelog

### v1.1 (YYYY-MM-DD)
- Changed: accent color from #22D3EE to #2563EB (user requested blue shift)
- Updated: all semantic accent tokens in tokens.css
- Updated: dark mode accent remapping
- Verified: WCAG AA contrast passes for new accent (7.1:1 on bg)
```

---

## Multi-Token Changes

When changing multiple tokens at once (e.g., switching aesthetic direction):

1. Read the new aesthetic preset from `references/aesthetic_directions.md`
2. Generate a token diff: list every token that changes, old → new
3. Apply changes section by section in `tokens.css`
4. Run a full verification pass (contrast, cross-references)
5. Update all preview files and UI kits that reference changed tokens
6. Consider dispatching a single agent to update all UI kit files in parallel

---

## Adding Components to an Existing System

1. Read `foundations/component_patterns.md` for the component spec
2. Generate a new `preview/{component}.html` file
3. Add the component to `design-system.html` (new section + sidebar link)
4. Add the component to relevant UI kits
5. Update `README.md` component documentation and file index
6. Dispatch Agent H (evaluation) to verify the new component passes quality checks

---

## Version Migration

When upgrading a design system generated by an earlier version of this skill:

### Schema Version Detection

Read the version from the generated `SKILL.md` front matter or `README.md` header.
If no version is present, assume `1.0`.

### Breaking Changes by Version

| From → To | Change | Migration Action |
|-----------|--------|-----------------|
| 1.0 → 1.1 | File renames (`colors_and_type.css` → `tokens.css`, `Design_System.html` → `design-system.html`) | Rename files, update all cross-references in HTML/CSS, update any import paths |
| 1.0 → 1.1 | Token naming convention changed (evocative → systematic as default) | If tokens use evocative names (`--ink`, `--paper`), they still work — systematic names are optional. Add systematic aliases if desired. |
| 1.0 → 1.1 | Three-layer architecture enforcement | Verify `tokens.css` has all three sections (raw, semantic, component). If a layer is missing, regenerate that section. |

### Migration Steps

1. **Backup** the existing design system folder
2. **Rename files** to new conventions (if applicable)
3. **Update imports** — grep for old filenames in all HTML/CSS files
4. **Verify structure** — confirm three-layer architecture is intact
5. **Run validation** — use `validation/css_validation.md` checks
6. **Update version** — bump version in README.md and SKILL.md
7. **Test** — open design-system.html and verify visual parity

### Non-Breaking Changes

New foundation files, new presets, new validation rules, and template improvements
do not require migration. They are available for new generations without affecting
existing output.

---

## Post-Generation Capture (Learning Feedback Loop)

After each generation or iteration, capture what the user changed, rejected, or
fixed. This feedback improves future generations for the same project.

### What to Capture

| Category | What to Track | Why |
|----------|---------------|-----|
| Rejected directions | Aesthetic directions considered and rejected, with reason | Avoid suggesting the same direction again |
| User edits | Which tokens or components the user changed after delivery | Reveal gaps in interview or synthesis |
| Evaluation misses | Issues the user found that Agent H did not catch | Improve the evaluation checklist |
| Recurring corrections | Same fix applied across multiple iterations | Signal a systematic gap in the skill |

### LEARNINGS.md Format

Store a project-local `LEARNINGS.md` file in the design system root:

```markdown
# Learnings — [Brand] Design System

Captured from user feedback on generated output. Feed into future packet synthesis.

## Rejected Directions
- [direction]: [why rejected] (YYYY-MM-DD)

## User Edits
- [file]: [what changed] — [inferred reason] (YYYY-MM-DD)

## Evaluation Misses
- [issue]: [why H missed it] — [checklist fix if applicable] (YYYY-MM-DD)

## Recurring Corrections
- [pattern]: [how many times] — [root cause hypothesis] (YYYY-MM-DD)
```

### How to Use Learnings

1. **Before synthesis:** Read `LEARNINGS.md` if it exists. Apply known preferences
   to the context packet before dispatching agents.
2. **After delivery:** When the user modifies generated output, note which
   tokens/components changed and append to the relevant section.
3. **Across projects:** If the same user generates multiple design systems,
   carry project-level learnings into the interview phase as suggested defaults.
4. **Skill improvement:** Recurring evaluation misses should be proposed as
   additions to `schemas/evaluation_checklist.md`.

---

## Cross-Project Learning Mechanism

After every 5 generations for the same user or project, propose amendments to core skill files based on accumulated `LEARNINGS.md` entries.

### Amendment Triggers

After 5 generations, review `LEARNINGS.md` for patterns:

| Pattern Type | Amendment Target |
|--------------|------------------|
| Rejected aesthetic directions | `references/aesthetic_directions.md` — update or add presets |
| Recurring user token edits | `schemas/agent_context_packet.md` — update defaults or recommendations |
| Recurring evaluation misses | `schemas/evaluation_checklist.md` — add new checks |
| Recurring component issues | `foundations/components/*.md` — update guidance |
| Recurring workflow friction | `workflow/generation_flow.md` — adjust steps or guidance |

### Amendment Process

1. After the 5th generation, analyze all `LEARNINGS.md` entries
2. Group by pattern type and frequency
3. Draft specific amendment proposals with rationale
4. Present to user: "Based on 5 generations, I recommend these changes to improve future outputs:"
5. If user approves, make the targeted edits to the specified files
6. Document the amendment in the skill's own changelog

### Example Amendments

```markdown
## Proposed Amendments (After 5 Generations)

### to `schemas/agent_context_packet.md`
- **Change:** Add `typography.display_fallback` field with default "Inter"
- **Rationale:** 3/5 generations needed fallback for custom display fonts

### to `schemas/evaluation_checklist.md`
- **Add:** Check for "hover states beyond opacity"
- **Rationale:** 2/5 user edits addressed hover state variety

### to `workflow/interview_framework.md`
- **Update:** Add explicit question about data density preference
- **Rationale:** 4/5 generations required post-generation density adjustments
```
