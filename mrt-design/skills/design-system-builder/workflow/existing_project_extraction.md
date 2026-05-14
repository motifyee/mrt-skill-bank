# Existing Project Extraction

How to reverse-engineer a design system from a living codebase, normalize it into
token format, and refactor the project to use the new system. This is the "ingest
and upgrade" path — the complement to the greenfield discovery workflow.

---

## When to Use This Workflow

- The user says "extract my design system", "audit my current styles", "refactor
  our CSS", or "turn our existing styles into a design system"
- The user provides a codebase or points to a project directory
- The user wants to migrate from informal styling (hardcoded values, Tailwind
  utilities, SCSS variables) to a token-based design system
- The user wants to enhance consistency across an existing product

---

## Phase 1: Project Scan

Analyze the project to understand its structure, tech stack, and styling approach.

### 1.1 Identify the Tech Stack

Check for:
- Framework: React, Vue, Astro, Svelte, Next.js, Nuxt, plain HTML
- Styling: CSS modules, SCSS/LESS, Tailwind, styled-components, Emotion, CSS-in-JS
- Existing design tokens: CSS custom properties, SCSS variables, Tailwind config,
  theme files, design token JSON/YAML
- Component library: any existing component system (MUI, Chakra, Radix, custom)

### 1.2 Locate Style Sources

Build a file inventory:

```
styles/
├── global.css            <- global styles, reset, base elements
├── tokens.css            <- existing CSS custom properties (if any)
├── typography.css        <- font-related styles
├── components/           <- component-specific styles
│   ├── button.css
│   ├── card.css
│   └── ...
└── utilities.css         <- helper classes

tailwind.config.js        <- Tailwind configuration (if used)
src/theme/                <- theme files (if React/Styled)
```

### 1.3 Automated Extraction

Run these searches across the project to collect raw design values. Capture every
unique value — deduplication happens in Phase 2.

> **Cross-platform note:** The commands below use Unix `grep` and `find`. On Windows
> PowerShell, use `rg` (ripgrep) as the primary tool — it works identically on
> Windows, macOS, and Linux. Install via `winget install BurntSushi.ripgrep.MSVC`
> or from https://github.com/BurntSushi/ripgrep. Key mapping:
> - `grep -roh` → `rg -ro --no-filename`
> - `grep --include='*.css'` → `rg -g '*.css'`
> - `sort | uniq -c | sort -rn` → pipe to `Sort-Object` and `Group-Object` in PowerShell, or use `uniq` via WSL/Git Bash
> - `find . -type f` → `Get-ChildItem -Recurse -File` or `rg --files`
>
> If `rg` is not available, PowerShell equivalents are shown after each Unix command.

**Colors** — extract all color values:
```bash
# Hex colors
grep -roh '#[0-9a-fA-F]\{3,8\}' --include='*.css' --include='*.scss' --include='*.tsx' --include='*.jsx' --include='*.vue' --include='*.astro' --include='*.html' | sort | uniq -c | sort -rn
# PowerShell: rg -ro --no-filename '#[0-9a-fA-F]{3,8}' -g '*.css' -g '*.scss' -g '*.tsx' -g '*.jsx' -g '*.vue' -g '*.astro' -g '*.html' | Group-Object | Sort-Object Count -Descending

# rgb/rgba/hsl/hsla
grep -roh 'rgba\?\([^)]*\)' --include='*.css' --include='*.scss' | sort | uniq -c | sort -rn
# PowerShell: rg -ro --no-filename 'rgba?\([^)]*\)' -g '*.css' -g '*.scss' | Group-Object | Sort-Object Count -Descending

# CSS custom properties referencing colors
grep -roh '\-\-[^:]*color[^:]*:\s*[^;]*' --include='*.css' --include='*.scss' | sort -u
# PowerShell: rg -o '\-\-[^:]*color[^:]*:\s*[^;]*' -g '*.css' -g '*.scss' | Sort-Object -Unique

# Tailwind color usage
grep -roh 'bg-\S*\|text-\S*\|border-\S*\|fill-\S*' --include='*.tsx' --include='*.jsx' --include='*.vue' | sort | uniq -c | sort -rn
# PowerShell: rg -ro --no-filename 'bg-\S*|text-\S*|border-\S*|fill-\S*' -g '*.tsx' -g '*.jsx' -g '*.vue' | Group-Object | Sort-Object Count -Descending
```

**Typography** — extract font declarations:
```bash
# Font families
grep -roh 'font-family:\s*[^;]*' --include='*.css' --include='*.scss' | sort -u
# PowerShell: rg -o 'font-family:\s*[^;]*' -g '*.css' -g '*.scss' | Sort-Object -Unique

# Font sizes
grep -roh 'font-size:\s*[^;]*' --include='*.css' --include='*.scss' | sort -u
# PowerShell: rg -o 'font-size:\s*[^;]*' -g '*.css' -g '*.scss' | Sort-Object -Unique

# Font weights
grep -roh 'font-weight:\s*[^;]*' --include='*.css' --include='*.scss' | sort -u
# PowerShell: rg -o 'font-weight:\s*[^;]*' -g '*.css' -g '*.scss' | Sort-Object -Unique

# Line heights
grep -roh 'line-height:\s*[^;]*' --include='*.css' --include='*.scss' | sort -u
# PowerShell: rg -o 'line-height:\s*[^;]*' -g '*.css' -g '*.scss' | Sort-Object -Unique
```

**Spacing** — extract padding, margin, gap values:
```bash
grep -roh '\(padding\|margin\|gap\):\s*[^;]*' --include='*.css' --include='*.scss' | sort -u
# PowerShell: rg -o '(padding|margin|gap):\s*[^;]*' -g '*.css' -g '*.scss' | Sort-Object -Unique
```

**Borders & Radii**:
```bash
grep -roh 'border-radius:\s*[^;]*' --include='*.css' --include='*.scss' | sort -u
# PowerShell: rg -o 'border-radius:\s*[^;]*' -g '*.css' -g '*.scss' | Sort-Object -Unique

grep -roh 'border:\s*[^;]*' --include='*.css' --include='*.scss' | sort -u
# PowerShell: rg -o 'border:\s*[^;]*' -g '*.css' -g '*.scss' | Sort-Object -Unique
```

**Shadows**:
```bash
grep -roh 'box-shadow:\s*[^;]*' --include='*.css' --include='*.scss' | sort -u
# PowerShell: rg -o 'box-shadow:\s*[^;]*' -g '*.css' -g '*.scss' | Sort-Object -Unique
```

**Transitions & Animations**:
```bash
grep -roh 'transition:\s*[^;]*' --include='*.css' --include='*.scss' | sort -u
# PowerShell: rg -o 'transition:\s*[^;]*' -g '*.css' -g '*.scss' | Sort-Object -Unique

grep -roh 'animation:\s*[^;]*' --include='*.css' --include='*.scss' | sort -u
# PowerShell: rg -o 'animation:\s*[^;]*' -g '*.css' -g '*.scss' | Sort-Object -Unique
```

### 1.4 Component Inventory

Scan for component patterns:
```bash
# Find component files (Unix/macOS/Git Bash)
find . -type f \( -name '*.tsx' -o -name '*.jsx' -o -name '*.vue' -o -name '*.svelte' -o -name '*.astro' \) | head -100

# PowerShell equivalent
Get-ChildItem -Recurse -File -Include *.tsx,*.jsx,*.vue,*.svelte,*.astro | Select-Object -First 100

# Cross-platform using ripgrep (recommended)
rg --files -g '*.tsx' -g '*.jsx' -g '*.vue' -g '*.svelte' -g '*.astro' | head -100
```

For each component found, note:
- File path
- Visual purpose (button, card, form input, nav, etc.)
- Styling approach (CSS module, inline styles, Tailwind classes, styled-components)

---

## Phase 2: Token Extraction & Normalization

Convert raw extracted values into a structured token inventory.

### 2.1 Color Normalization

1. Convert all colors to hex (normalize rgb, rgba, hsl, hsla, named colors)
2. Cluster similar colors — values within 5% lightness are likely the same token
3. Build the palette:

```
Raw extracted:        Clustered into:
#1a1a2e (23 uses)  →  --neutral-900 (darkest)
#16213e (18 uses)  →  --neutral-900 (same cluster)
#0f3460 (12 uses)  →  --neutral-800
#533483 (8 uses)   →  --accent (primary brand)
#e94560 (15 uses)  →  --accent-alt
#ffffff (89 uses)  →  --neutral-0 (lightest)
#f5f5f5 (22 uses)  →  --neutral-50
```

4. Identify the accent color(s) — usually the least frequent but most distinctive
5. Determine neutral temperature: do the grays lean warm (yellow undertone) or cool (blue)?
6. Verify the palette covers: primary, secondary, accent, surface, surface-alt,
   success, warning, error

### 2.2 Typography Normalization

1. List all font-family declarations. Identify 1-3 families used:
   - Which is the display/heading font?
   - Which is the body font?
   - Is there a monospace font?
2. Collect all unique font-size values. Map to a scale:
   ```
   Raw: 11px, 12px, 13px, 14px, 16px, 18px, 20px, 24px, 28px, 32px, 40px, 48px, 56px, 72px
   Scale: caption(11) label(12) body-sm(14) body(16) body-lg(18) h4(20) h3(28) h2(40) h1(56) display(72)
   ```
3. Calculate the scale ratio (divide consecutive sizes). Is it consistent?
4. Collect all font-weight values. Map to named weights.
5. Collect all line-height values. Categorize by usage (tight for headings, comfortable for body).

### 2.3 Spacing Normalization

1. Collect all unique spacing values (px, rem, em)
2. Convert to px (assuming 16px base for rem)
3. Determine the base unit: do values cluster on multiples of 4px or 8px?
4. Build the spacing scale:
   ```
   Raw: 4px, 8px, 12px, 16px, 20px, 24px, 32px, 40px, 48px, 64px, 80px, 96px, 128px
   Base unit: 4px
   Scale: space-1(4) space-2(8) space-3(12) space-4(16) space-5(20→24) space-6(24→32) ...
   ```
5. Note any outliers — values that don't fit the scale are candidates for correction.

### 2.4 Radii, Shadow, Motion Normalization

Apply the same clustering approach:
- Group radius values into sm/md/lg/xl/full
- Group shadow values into elevation levels (0-4)
- Group transition durations into fast/base/slow
- Extract easing curves

### 2.5 Generate Extracted Token Report

Produce a structured summary of all extracted tokens, noting for each:
- Token name (proposed)
- Raw value(s) found in codebase
- Frequency of use
- Confidence level (high if consistent, low if scattered)
- Semantic role (what this token appears to mean in the UI)

---

## Phase 3: Gap Analysis & Audit

Compare the extracted system against professional standards.

### 3.1 Consistency Audit

Check for:
- **Same purpose, different values**: e.g., 3 different "primary blue" shades across files
- **Hardcoded values that should be tokens**: hex colors in component files, inline styles
- **Missing states**: components without hover, focus, disabled, or error states
- **Inconsistent spacing**: similar components using different padding/margin
- **Inconsistent radii**: cards with 8px in one file, 12px in another
- **Inconsistent typography**: same semantic level using different sizes

### 3.2 Accessibility Audit

Using `foundations/ux_and_accessibility.md`, check:
- All text/background color pairs meet WCAG AA (4.5:1 normal, 3:1 large)
- Focus indicators exist and are visible on all interactive elements
- Touch targets meet 44x44px minimum
- Color is not the only indicator of state
- Reduced motion is respected

### 3.3 Coverage Audit

Using the skill's foundation files, check what's present vs. missing:

| Category | Reference | Check |
|----------|-----------|-------|
| Color system | visual_system.md | Full palette? Semantic colors? Dark mode? |
| Typography | visual_system.md | Type scale? Line heights? Weight strategy? |
| Spacing | visual_system.md | Consistent scale? Applied uniformly? |
| Components | foundations/components/ | Buttons, cards, forms, nav all present? |
| Responsive | responsive_system.md | Breakpoint strategy? Mobile-first? |
| Motion | motion_choreography.md | Entrance patterns? Scroll animations? |
| Micro-interactions | micro_interactions.md | Loading, toasts, empty states? |
| Theming | theming_schema.md | Light/dark support? |
| Imagery | imagery_and_illustration.md | Photo treatment? Placeholder strategy? |

### 3.4 Generate Audit Report

Produce a structured report:
```markdown
# Design System Audit: [Project Name]

## Extracted Tokens
[Summary of all normalized tokens with confidence levels]

## Consistency Issues
- [X] different values used for the same semantic purpose
- [X] hardcoded values that should be tokens
- [X] components missing interaction states

## Accessibility Issues
- [X] contrast failures
- [X] missing focus indicators
- [X] undersized touch targets

## Coverage Gaps
- [Missing components]
- [Missing token categories]
- [Missing responsive breakpoints]
- [Missing dark mode support]

## Enhancement Opportunities
- [Specific improvements from the skill's foundation files]
- [Layout patterns from layout_compositions.md not yet used]
- [Micro-interactions from micro_interactions.md to add]
```

---

## Phase 4: Design System Generation

Generate the new design system from extracted + enhanced tokens.

### 4.1 Merge Extracted with Enhanced

For each token category:
1. Start with the extracted value (respect what exists)
2. Fill gaps with values derived from the skill's foundations
3. Resolve inconsistencies by choosing the most-used value
4. Add missing tokens (dark mode variants, semantic colors, responsive breakpoints)

### 4.2 Apply Aesthetic Direction

If the existing system lacks clear aesthetic direction:
1. Analyze the dominant characteristics (warm/cool, sharp/round, dense/spacious)
2. Map to the closest aesthetic preset from `aesthetic_directions.md`
3. Or synthesize a novel direction using `aesthetic_synthesis.md`
4. Note the direction as "extracted and refined" (not "chosen from scratch")

### 4.3 Generate Deliverables

Follow `workflow/generation_flow.md` to produce:
1. `tokens.css` — tokens file
2. `README.md` — system documentation
3. `DECISIONS.md` — extracted + enhanced decision rationale log
4. `design-system.html` — visual preview dashboard
5. Component previews
6. UI kits (if surface types are identified)

For extraction projects, add:
7. **Migration map** — which old values map to which new tokens
8. **Diff report** — what changed and why

### 4.4 Parallel Agent Generation

For large projects, use `workflow/agent_decomposition.md` to generate
deliverables in parallel. The context packet is built from the extracted
and enhanced token data (Phase 2+3 output) rather than interview answers.

---

## Phase 5: Refactoring Strategy

Plan and execute the migration from old styling to the new token-based system.

### 5.1 Migration Approaches

Choose based on project size and risk tolerance:

**Approach A: Layered Migration (Low Risk)**
1. Add the new token CSS file to the project (alongside existing styles)
2. Map old values to new tokens via a compatibility shim
3. Gradually replace hardcoded values with token references
4. Remove old styles once migration is complete

**Approach B: Component-by-Component (Medium Risk)**
1. Start with leaf components (buttons, badges, inputs)
2. Replace their styles with token-based versions
3. Move to composite components (cards, forms, nav)
4. Finish with layout components and pages

**Approach C: Full Rewrite (High Risk, High Reward)**
1. Create the new design system in isolation
2. Build a parallel set of components using the new system
3. Switch over with a feature flag
4. Remove old components after verification

### 5.2 Token Replacement Map

Generate a find-and-replace map for safe migration:

```markdown
| Old Value | New Token | Files Affected |
|-----------|-----------|----------------|
| #533483 | var(--accent) | 8 files |
| #e94560 | var(--accent-alt) | 5 files |
| padding: 16px | padding: var(--space-4) | 23 files |
| font-size: 14px | font-size: var(--fs-body-sm) | 31 files |
```

### 5.3 Tailwind Migration (Special Case)

If the project uses Tailwind:
1. Map the extracted tokens to a `tailwind.config.js` theme extension
2. Generate the config with custom colors, spacing, typography, and radii
3. Replace magic values in class names with theme references
4. Document which Tailwind defaults are overridden

### 5.4 Component Refactoring

For each component being refactored:
1. Identify current styling approach
2. Map to the equivalent pattern from `foundations/components/`
3. Replace hardcoded values with token references
4. Add missing states (hover, focus, disabled, error)
5. Add responsive behavior from `responsive_system.md`
6. Add dark mode support from `theming_schema.md`
7. Verify accessibility compliance

### 5.5 Parallel Refactoring with Agents

Split the refactoring work across parallel agents using the decomposition
from `workflow/agent_decomposition.md`:

```
Agent 1: Refactor global styles and tokens
Agent 2: Refactor button, badge, and input components
Agent 3: Refactor card, modal, and overlay components
Agent 4: Refactor navigation and layout components
Agent 5: Refactor page-level styles and section layouts
Agent 6: Add responsive breakpoints and mobile adaptations
Agent 7: Add dark mode support across all components
Agent 8: Run accessibility audit on refactored output
```

Each agent receives:
- The token replacement map
- The relevant component pattern from `foundations/components/`
- The list of files to refactor
- Quality checks specific to their scope

---

## Phase 6: Enhancement

After the base migration, enhance the system using the skill's expanded knowledge.

### 6.1 Add Missing Components

Cross-reference the component inventory (Phase 1.4) with the full catalog in
`foundations/components/`. Add any missing components:
- Badges/tags if not present
- Toast/notification system from `micro_interactions.md`
- Empty states from `micro_interactions.md`
- Loading skeletons from `micro_interactions.md`
- Data table patterns from `data_visualization.md` (if applicable)
- Dashboard layouts from `layout_compositions.md` (if applicable)

### 6.2 Apply Layout Patterns

Using `foundations/layout_compositions.md`, improve page-level layouts:
- Replace ad-hoc grids with named composition patterns
- Add section rhythm from `visual_storytelling.md`
- Create signature moments per page
- Improve visual hierarchy

### 6.3 Apply Motion System

Using `foundations/motion_choreography.md`:
- Add entrance animations for page loads
- Add scroll-triggered reveals for below-fold content
- Standardize hover/press/focus transitions
- Add reduced-motion support

### 6.4 Apply Responsive System

Using `foundations/responsive_system.md`:
- Establish breakpoint tokens
- Ensure all layouts adapt at each breakpoint
- Verify touch targets on mobile
- Apply fluid typography with `clamp()`

### 6.5 Apply Theming

Using `schemas/theming_schema.md`:
- Generate dark mode variant
- Generate high-contrast variant (if needed)
- Add theme switching JavaScript
- Verify contrast in all themes

### 6.6 Framework-Specific Output

Using `schemas/framework_targets.md`, generate components in the project's
framework (React, Vue, Astro, Svelte) with proper TypeScript types.

---

## Output Checklist

Before delivering the extracted and enhanced design system:

- [ ] All extracted tokens documented with source locations
- [ ] Migration map generated (old → new values)
- [ ] DECISIONS.md captures major extraction and normalization decisions
- [ ] Accessibility audit completed and issues resolved
- [ ] Dark mode theme generated
- [ ] Responsive breakpoints established
- [ ] Missing components added
- [ ] Component refactoring complete (or plan documented)
- [ ] Preview dashboard renders correctly
- [ ] No hardcoded values remaining in refactored files
- [ ] Reduced-motion media query included
- [ ] Substitution flags for any generated assets
