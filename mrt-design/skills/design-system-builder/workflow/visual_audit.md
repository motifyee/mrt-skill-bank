# Visual Audit Workflow

Standalone audit mode for scoring an existing UI against professional design
standards. Produces a report card with dimension scores, specific evidence,
and actionable fixes.

Use this when:
- The user has an existing site/app and wants a visual health check
- Before a redesign — understand what's working and what isn't
- The UI "looks off" but nobody can articulate why
- Reviewing a PR that touches significant styling
- Evaluating a design system's real-world application

---

## Audit Modes

### Quick Audit (single surface)

Score one page or surface. Takes 5-10 minutes.
Use when the user points at a specific URL or screenshot.

### Full Audit (entire product)

Score every surface type in the product. Takes 20-40 minutes.
Use when the user wants a comprehensive visual health check.

### Slop Check (AI detection only)

Run only the AI slop detection dimension. Takes 2-5 minutes.
Use for quick sanity checks on generated interfaces.

---

## 10-Dimension Scoring Rubric

Each dimension is scored 0-10. Use the band definitions below:

| Score | Meaning |
|-------|---------|
| 9-10  | Professional-grade, intentional, consistent |
| 7-8   | Good overall, minor inconsistencies |
| 5-6   | Functional but inconsistent, lacks polish |
| 3-4   | Noticeable problems, feels unfinished |
| 0-2   | Broken, arbitrary, no systematic approach |

### 1. Color Consistency

**What to check:**
- Is there a defined palette, or are colors arbitrary?
- Are the same semantic colors used consistently (primary CTA always the same blue)?
- Are there random hex values that don't belong to any palette?
- Do accent colors complement or clash with the base palette?
- Are decorative colors purposeful or random?

**Scoring guide:**
| Score | Criteria |
|-------|----------|
| 9-10  | Every color traces to a token, palette is cohesive, semantic usage is consistent |
| 7-8   | Palette exists, 1-3 rogue values that don't fit |
| 5-6   | General palette visible but applied inconsistently, several arbitrary values |
| 3-4   | No clear palette, many random colors, semantic usage is mixed |
| 0-2   | Colors appear chosen ad-hoc with no system |

### 2. Typography Hierarchy

**What to check:**
- Is there a clear h1 > h2 > h3 > body > caption hierarchy?
- Do heading sizes decrease proportionally (not random jumps)?
- Is there a consistent type scale (modular, ratio-based)?
- Are font weights used semantically (bold for emphasis, not decoration)?
- Is line-height consistent and readable?

**Scoring guide:**
| Score | Criteria |
|-------|----------|
| 9-10  | Clear type scale, consistent weights, readable line-heights, no orphan styles |
| 7-8   | Hierarchy visible, 1-2 places where sizing breaks the scale |
| 5-6   | Hierarchy roughly exists but inconsistent sizing or weight usage |
| 3-4   | Headings and body blur together, multiple font families competing |
| 0-2   | No discernible hierarchy, random sizes and weights |

### 3. Spacing Rhythm

**What to check:**
- Is there a consistent spacing scale (4px, 8px, 16px, 24px, 32px...)?
- Do components use consistent internal padding?
- Is the gap between sections predictable?
- Are margins and paddings arbitrary or systematic?

**Scoring guide:**
| Score | Criteria |
|-------|----------|
| 9-10  | All spacing traces to a scale, rhythm is visually consistent, no arbitrary gaps |
| 7-8   | Scale exists, 2-3 places where spacing breaks rhythm |
| 5-6   | Rough consistency but many arbitrary pixel values |
| 3-4   | Spacing feels random, tight in some places and loose in others |
| 0-2   | No discernible spacing system |

### 4. Component Consistency

**What to check:**
- Do similar elements look similar? (All cards same border-radius, shadow, padding?)
- Are button styles consistent across the product?
- Do form inputs share styling?
- Are icons the same size and weight within a context?

**Scoring guide:**
| Score | Criteria |
|-------|----------|
| 9-10  | Components are clearly systematic, variants are intentional |
| 7-8   | Mostly consistent, 1-2 components that diverge |
| 5-6   | Similar components styled differently in several places |
| 3-4   | Little consistency between similar elements |
| 0-2   | Every instance of a component looks different |

### 5. Responsive Behavior

**What to check:**
- Does the layout work at 375px, 768px, 1024px, 1280px?
- Does typography scale down appropriately on mobile?
- Does navigation collapse or adapt?
- Are touch targets at least 44px on mobile?
- Is there any horizontal scroll at narrow widths?

**Scoring guide:**
| Score | Criteria |
|-------|----------|
| 9-10  | Fluid at all breakpoints, mobile-first approach evident |
| 7-8   | Works at all sizes, 1-2 minor issues at specific breakpoints |
| 5-6   | Desktop works, mobile has layout issues but content is accessible |
| 3-4   | Broken at one or more breakpoints, content overlaps or hides |
| 0-2   | Desktop-only, no responsive adaptation |

### 6. Dark Mode / Theme Completeness

**What to check (if dark mode exists):**
- Are all surfaces remapped (not just background and text)?
- Do images and media have appropriate treatment?
- Do shadows work on dark backgrounds (or use elevation instead)?
- Are contrast ratios maintained in both modes?

**If no dark mode:** Score N/A and skip. Note in the report whether dark mode is expected for the product type.

**Scoring guide:**
| Score | Criteria |
|-------|----------|
| 9-10  | Every token remapped, images adapted, elevation replaces shadow, contrasts verified |
| 7-8   | Most surfaces remapped, 1-2 inconsistencies |
| 5-6   | Background and text changed, but cards, borders, shadows inconsistent |
| 3-4   | Partial attempt, many surfaces still light-themed |
| 0-2   | Dark mode is just inverted colors or barely functional |

### 7. Motion & Animation

**What to check:**
- Do animations serve a purpose (feedback, orientation, delight)?
- Are durations consistent (150-300ms for micro, 300-500ms for transitions)?
- Is easing natural (ease-out for entrances, ease-in for exits)?
- Does `prefers-reduced-motion` work?
- Are there gratuitous animations that distract?

**Scoring guide:**
| Score | Criteria |
|-------|----------|
| 9-10  | Purposeful motion, consistent timing, reduced-motion respected |
| 7-8   | Good motion overall, 1-2 gratuitous or mistimed animations |
| 5-6   | Some purposeful motion, some arbitrary, timing inconsistent |
| 3-4   | Animations feel added for decoration, no consistent approach |
| 0-2   | Either no animation at all (static/dead) or chaotic/nauseating |

### 8. Accessibility

**What to check:**
- Do all text/background combinations pass WCAG AA contrast?
- Are focus indicators visible and high-contrast?
- Are interactive elements reachable by keyboard?
- Are form inputs properly labeled?
- Is color the sole indicator for any state? (it shouldn't be)
- Are touch targets at least 44x44px?

Use automated tools (Lighthouse, axe) where available, manual check otherwise.
Cross-reference with `foundations/ux_and_accessibility.md` for the full checklist.

**Scoring guide:**
| Score | Criteria |
|-------|----------|
| 9-10  | All checks pass, focus states visible, semantic HTML, ARIA where needed |
| 7-8   | Passes most checks, 1-2 contrast or focus issues |
| 5-6   | Basic structure is semantic, but contrast or keyboard nav has gaps |
| 3-4   | Significant accessibility problems, many contrast failures |
| 0-2   | No attention to accessibility, unusable by keyboard or screen reader |

### 9. Information Density

**What to check:**
- Is the content density appropriate for the surface type?
- Landing pages: is there breathing room?
- Dashboards: is data scannable without clutter?
- Is there a clear visual hierarchy guiding the eye?
- Does the squint test pass? (primary content identifiable when squinting)

**Scoring guide:**
| Score | Criteria |
|-------|----------|
| 9-10  | Density matches surface type, hierarchy guides reading, whitespace is intentional |
| 7-8   | Generally well-balanced, 1-2 sections too dense or too sparse |
| 5-6   | Some sections cluttered, others sparse, inconsistent approach |
| 3-4   | Everything is equally dense (wall of content) or equally sparse (wasteful) |
| 0-2   | No hierarchy, overwhelming or content-starved |

### 10. Polish & Edge States

**What to check:**
- Do hover states exist on interactive elements?
- Are focus states styled?
- Are active/pressed states present?
- Do loading states exist (skeleton screens, spinners)?
- Are empty states handled (no just-blank screens)?
- Are error states styled and informative?
- Do transitions exist between states?

**Scoring guide:**
| Score | Criteria |
|-------|----------|
| 9-10  | All states handled, transitions smooth, empty/error states designed |
| 7-8   | Most states handled, a few missing hover/focus/empty states |
| 5-6   | Hover exists but no focus/active/empty/error states |
| 3-4   | Default state only, no state feedback at all |
| 0-2   | Broken states, unstyled interactive elements |

---

## Audit Execution

### Step 1: Gather

- **Live URL:** Navigate to the page(s). Take screenshots at 375px, 768px, 1280px.
- **Codebase access:** Scan CSS/HTML for token usage, hardcoded values, font declarations.
- **Screenshots only:** Analyze what's visible. Note that some checks (keyboard nav, code-level) won't be possible.

### Step 2: Score

Work through each dimension. For each:
1. Record the score (0-10)
2. List 2-4 specific observations that justify the score
3. For each observation, provide a fix with the exact file:line if available

### Step 3: Compile

Present the audit as a report card:

```
## Visual Audit Report

**Surface:** [page/component name]
**Date:** [audit date]
**Overall Score:** [average] / 10

| Dimension              | Score | Status |
|------------------------|-------|--------|
| Color Consistency      | 8/10  | Good   |
| Typography Hierarchy   | 6/10  | Fair   |
| Spacing Rhythm         | 4/10  | Needs work |
| Component Consistency  | 7/10  | Good   |
| Responsive Behavior    | 9/10  | Great  |
| Dark Mode              | N/A   | —      |
| Motion & Animation     | 5/10  | Fair   |
| Accessibility          | 7/10  | Good   |
| Information Density    | 8/10  | Good   |
| Polish & Edge States   | 3/10  | Needs work |

### Critical Issues (score ≤ 4)
- **Spacing Rhythm (4/10):** [specific observations with file:line]
- **Polish & Edge States (3/10):** [specific observations with file:line]

### Quick Wins (easy fixes, high impact)
1. [Fix] — [file:line]
2. [Fix] — [file:line]

### Strengths
- [What's working well]
```

### Step 4: Prioritize Fixes

Group fixes by impact and effort:

| Priority | Criteria |
|----------|----------|
| P0 — Fix now | Accessibility failures, broken layouts |
| P1 — Next sprint | Low-score dimensions (≤4), quick wins in medium-score dimensions |
| P2 — Backlog | Polish items, medium-score improvements |
| P3 — Nice to have | High-score refinements (8→10) |
