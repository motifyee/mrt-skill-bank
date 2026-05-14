# AI Slop Detection Reference

Concrete checklist for identifying generic AI-generated design patterns.
Use during evaluation (Phase 5) or as a standalone slop check on any interface.

---

## What Is AI Slop?

Design output that is technically functional but aesthetically generic — the kind
of interface that looks like every other AI-generated page. It's not "bad" in a
functional sense, but it's instantly recognizable as machine-made, lacks
personality, and undermines trust with design-literate audiences.

---

## Second-Order Slop

First-order slop is widely documented. AI systems trained on "how to avoid AI slop" guides
converge on a new set of recognizable patterns — second-order slop. This is more dangerous
because it superficially appears distinctive while remaining generic at the system level.

**The tell:** The output has clearly tried to be different, but the specific way it's different
is itself predictable. A trained eye recognizes it as "the anti-slop response" rather than
a genuine design decision.

### Common Second-Order Slop Signatures

| Pattern | Why it emerged | What makes it still slop |
|---------|---------------|--------------------------|
| **Space Grotesk + teal (#14B8A6) + asymmetric hero** | Replacing Inter + purple as "the non-generic developer tool" | This combination is now the default anti-slop developer-tool output. It signals "I avoided purple" not "I have a design direction." |
| **Neue Haas Grotesk + off-white (#FAFAF8) + terracotta accent** | "Calm luxury" aesthetic for SaaS | Became the default "premium fintech/design tool" combination after appearing in anti-slop articles. |
| **Clash Display + thick borders + bright primary** | Brutalist as an anti-softness reaction | Adopted as a shortcut for "bold and distinctive" without a brand reason for structural aggression. |
| **Plus Jakarta Sans + deep navy (#1E3A5F) + subtle gradient** | "Safe enterprise" substitute for pure blue | The combination is now the second most common AI-generated enterprise look. |
| **Epilogue/Manrope + 3D illustrations + gradient mesh** | Friendly SaaS alternative to corporate stiffness | Overused in the 2022–2024 SaaS explosion. Still generic despite the "personality." |
| **Glassmorphism on dark with neon glow** | "Sophisticated dark mode" vs. flat dark | The frosted panel + gradient border combination signals "AI dark mode" as reliably as purple-to-blue. |

### How to Detect Second-Order Slop

After applying the first-order anti-slop check, ask:

1. **Would this aesthetic combination appear in a "top AI design trends" roundup?** If yes, it's second-order slop.
2. **Does the font/color/layout combination have a brand-specific rationale, or does it reflect "what good anti-slop looks like"?** Document the rationale — undocumented choices are suspects.
3. **Has the signature moment been derived from brand context, or selected from a list of "distinctive" visual moves?** Signatures derived from the brand (industry, audience, product domain) resist convergence; signatures borrowed from aesthetic trend lists don't.

### Escape Velocity Rule

To escape both first and second-order slop, every major design decision must be traceable
to a brand constraint, not a stylistic trend:

- **Font choice** → traceable to brand value word + audience expectation (not "what's not Inter")
- **Accent derivation** → traceable to industry color temperature + trust level (not "what's not purple")
- **Layout asymmetry** → traceable to content structure (not "asymmetry is distinctive")
- **Signature moment** → traceable to the product's core function or user insight (not "a glow effect")

If the design decision can be explained as "because it's less generic than X," it may already
be converging on second-order slop. The question is always: "Why for THIS brand?"

---

## Affirmative Creative Moves

These are specific positive actions that produce genuine differentiation — not avoidance
tactics, but creative decisions that add a distinct voice. Apply these when the design
feels competent but anonymous.

### Move 1: Derive the Palette from Domain Color Temperature

Don't pick a "non-default" accent. Instead, map the product domain to its natural
color temperature:

| Domain | Natural temperature | Starting hue range |
|--------|--------------------|--------------------|
| Finance / legal | Cool-warm neutral | Warm grey, deep copper, midnight navy |
| Healthcare | Clean cool | Teal-to-cerulean, not clinical white |
| Developer tools | Cool technical | Blue-green, electric cyan, zinc — not purple |
| Consumer lifestyle | Warm expressive | Coral, clay, marigold, moss |
| B2B enterprise | Trust cool | Deep indigo-navy, steel, charcoal |
| Creative tools | High chroma | Single dominant hue at 70+ saturation |

A palette derived from domain temperature is defensible to any design reviewer and
reads as intentional rather than arbitrary.

### Move 2: Introduce Typographic Role Contrast

Pick fonts that create tension through classification contrast, not just weight contrast:

| Effective pairs | Why they contrast |
|----------------|-----------------|
| High-contrast serif + geometric sans | Historical authority × contemporary precision |
| Condensed grotesque + humanist sans | Urgency × approachability |
| Monospace display + clean sans | Engineering × usability |
| Variable display + tight proportioned sans | Expressive range × stability |

The rule: if both fonts feel equally safe, one of them is wrong.

### Move 3: Anchor Whitespace to Product Rhythm

Generic layouts use uniform spacing. Distinctive layouts mirror the product's rhythm:

- **CI/CD tool**: Dense tables anchor most of the surface, with single atmospheric hero moment — not balanced alternation
- **Creative tool**: Wide margin "breathing" sections between dense work areas — mirrors the act/reflect rhythm of creative work
- **Legal/finance**: Editorial whitespace at heading level; compressed inside data — signals that narrative and data are different registers
- **Communication tool**: Overlapping surfaces, spatial conversation metaphors — layout IS the product metaphor

Define the spacing contrast before writing CSS. Dense vs. generous is a layout decision, not a token value.

### Move 4: Give Components Character Rules That Conflict

The best design systems have components that resolve a stated tension. Generic systems give every component the same treatment. Specify one intentional conflict:

- **Tight border radius on forms, generous radius on marketing cards** — Engineering precision meets consumer warmth
- **Flat cards in the UI, elevated cards in the marketing surface** — Same brand, different context register
- **Uppercase labels with sentence-case buttons** — Signals that labels are metadata, not content
- **Full border on inputs, underline-only on search** — Search is exploratory; forms are structured

### Move 5: Make the Signature Irreplaceable

A signature moment passes this test: **can you put it in a different brand's design
without changing it?** If yes, it's not a signature — it's a style.

Generic signatures (replaceable): glow effect, gradient mesh background, animated
count-up numbers, frosted glass card, dot pattern background.

Brand-derived signatures (irreplaceable):
- Vercel's deployment-status pill — signals correctness, pulled from the product
- Stripe's blurred gradient — specifically blurred to suggest the complexity hidden beneath
- Linear's priority-dot system — product metaphor (importance = size/color) made visual

When writing `signature_anchor`, ask: "If you stripped the brand name, would this signature
still tell you what this product does?" Good signatures answer that question with yes.

---

## Creative Brief Exemption Rule

**Before flagging any output as slop, check the `creative_brief` field in the context packet.**

Documented intentional patterns are NOT slop. If the creative brief explicitly documents:
- Rounded corners + sans-serif + blue as a brand requirement
- Centered layouts as a deliberate choice
- Specific color/font combinations that match an existing brand

Then these patterns are intentional, not generic. Only flag as slop when the output:
1. Lacks any creative brief documentation, OR
2. The creative brief documents X but the output defaults to Y

This exemption prevents false positives when the user has intentionally chosen conventional aesthetics.

## Detection Checklist

### Color Anti-Patterns

- [ ] **Purple-to-blue gradient hero** — The most common AI tell. Purple (#6366f1) fading to blue (#3b82f6) as a hero background. No human designer with a brief would default to this.
- [ ] **Indigo as primary without rationale** — Default primary color is indigo-500/600 without any brand rationale. If the aesthetic direction explicitly calls for it (e.g., Soft SaaS for B2B) and the rationale is documented, this is acceptable. Flag only when indigo appears as an unexamined default.
- [ ] **Generic success/error/warning colors** — Using Tailwind default green-500, red-500, yellow-500 without adjusting hue or saturation to match the palette.
- [ ] **Neutrals are pure gray** — All grays are `#000` with opacity or pure `#808080`. No warm or cool bias. Every professional palette has a temperature.
- [ ] **Rainbow accent usage** — Every section uses a different accent color from the rainbow, with no semantic meaning.

### Typography Anti-Patterns

- [ ] **Inter as default heading font** — Inter is a fine body font but as a heading font it screams "I didn't think about typography." Similarly: System UI stack with no personality.
- [ ] **No display/body font contrast** — Same font family for headings and body with only weight differentiation. The pair lacks tension.
- [ ] **Font sizes are round numbers** — All sizes are 12, 14, 16, 18, 20, 24, 32, 48. No type scale (no ratio like 1.25 or 1.333).
- [ ] **Centered text everywhere** — Body copy, feature descriptions, and long paragraphs are center-aligned. Center alignment is for headlines and short CTAs only.
- [ ] **Inter paired with purple/indigo accent** — The combination of Inter as display font + #6366F1/#8B5CF6 accent is the single most common AI-generated design. If this pairing appears, at minimum change the font or the accent.

### Layout Anti-Patterns

- [ ] **Centered-everything layout** — Every section is a centered column of content. No asymmetric layouts, no grid variety, no spatial tension.
- [ ] **Cookie-cutter section rhythm** — Every section is: [heading] + [subtitle] + [3 equal cards]. The same rhythm repeats 5+ times down the page.
- [ ] **No whitespace variation** — Equal spacing between every section. No breathing room, no dramatic pauses, no rhythm.
- [ ] **Identical card styling** — Feature cards, testimonial cards, pricing cards, and stat cards all share the same border-radius, shadow, padding, and layout.
- [ ] **Three-column feature grid** — Exactly three feature cards in a row, every time. No 2+1, no bento, no full-width features.

### Component Anti-Patterns

- [ ] **Glass morphism with no purpose** — `backdrop-filter: blur()` on cards that aren't over a gradient or image. The frosted glass has nothing to frost.
- [ ] **Rounded corners on everything** — Every element has `border-radius: 8px` or `12px`, including elements that should be sharp (code blocks, data tables, separators).
- [ ] **Box shadows on every card** — Every card has a subtle `box-shadow: 0 1px 3px rgba(0,0,0,0.1)`. No variety in elevation. Flat elements where shadows would help are unshadowed.
- [ ] **Hover states are just opacity change** — `opacity: 0.8` on hover for every interactive element. No scale, no color shift, no elevation change, no underline.
- [ ] **Single accent used everywhere** — The one accent color appears on buttons, links, badges, headings, icons, and dividers with no visual hierarchy through color restraint.
- [ ] **Symmetrical grid in every section** — Every section uses the same centered symmetric grid. No asymmetric splits, no offset columns, no edge-to-edge breaks.
- [ ] **Uniform border-radius** — Every element (buttons, cards, inputs, badges, modals) uses the same `border-radius` value. No intentional variation between interactive, structural, and decorative elements.

### Motion Anti-Patterns

- [ ] **Fade-up on scroll for everything** — Every section fades in from below using Intersection Observer. No variation in entrance direction, timing, or style.
- [ ] **Excessive scroll animations** — Every scroll trigger fires an animation. Nothing is static. The page never settles.
- [ ] **Bouncy easing on everything** — All animations use `cubic-bezier(0.34, 1.56, 0.64, 1)` or similar spring easing, even for utilitarian transitions.
- [ ] **No reduced-motion fallback** — Animations play regardless of `prefers-reduced-motion` preference.

### Content Anti-Patterns

- [ ] **Generic hero copy** — "Build something amazing" / "Streamline your workflow" / "Empower your team" — vague, benefit-free headlines.
- [ ] **Feature names are gerunds** — Every feature is named with a gerund: "Building", "Tracking", "Managing", "Optimizing". No distinctive naming.
- [ ] **Emojis as section icons** — Each feature card has an emoji instead of an icon or illustration. The "building" feature has 🏗️, "speed" has ⚡.
- [ ] **Testimonials are obviously fake** — Generic praise ("This changed everything!") from people with no company or role specified.
- [ ] **Pricing section with three tiers** — Always exactly three tiers. The middle one is "Most Popular." The features list is a wall of checkmarks.

### Structural Anti-Patterns

- [ ] **Generic hero with gradient background** — Centered text over a purple-to-blue gradient. Maybe a mesh gradient. No product screenshot, no illustration, no visual hook.
- [ ] **No signature moment** — The page has no unique visual element that would make it memorable. It's interchangeable with any other SaaS landing page.
- [ ] **Footer is an afterthought** — Footer is just a list of links. No brand reinforcement, no visual closure, no personality.
- [ ] **No above-the-fold differentiation** — Above the fold looks exactly like every competitor's above the fold. Nothing signals "this is [brand]."

---

## Severity Classification

| Severity | Pattern | Impact |
|----------|---------|--------|
| Critical | Generic hero, no signature moment, purple-to-blue gradient | Page is indistinguishable from AI output |
| Major | Centered-everything layout, cookie-cutter rhythm, no font contrast | Page feels template-generated |
| Minor | Emojis as icons, bouncy easing everywhere, identical card styling | Page has personality gaps but isn't totally generic |

---

## Fixing AI Slop

For each detected pattern, apply the corresponding antidote:

| Anti-pattern | Antidote |
|---|---|
| Purple-to-blue gradient | Choose a brand-derived palette. Reference `references/aesthetic_directions.md`. |
| Inter as heading font | Pick a display font with personality. Reference `foundations/visual_system.md` Typography. |
| Centered-everything layout | Introduce asymmetric grids, side-by-side sections, bento layouts. Reference `foundations/layout_compositions.md`. |
| Cookie-cutter rhythm | Vary section patterns: full-bleed, split, card cluster, data showcase, editorial. Reference `references/pattern_innovation.md`. |
| Glass morphism with no purpose | Either place glass over a gradient/image, or remove it and use solid surfaces. |
| Rounded corners on everything | Be intentional: round interactive elements, keep structural elements sharp. |
| Hover as opacity change | Add multi-property hover: scale + shadow + color shift. Reference `foundations/micro_interactions.md`. |
| Fade-up on everything | Vary entrance directions, stagger timing, use clip-path reveals. Reference `foundations/motion_choreography.md`. |
| Generic hero copy | Write specific, benefit-driven headlines that pass the "blank brand" test. Reference `foundations/content_design.md`. |
| No signature moment | Design one unique visual element per page that's immediately recognizable. Reference `foundations/visual_storytelling.md`. |

---

## Positive Distinctiveness Requirements

Every anti-pattern has a required positive alternative. For each slop pattern detected, the system must provide a specific distinctive replacement — not just flag the absence of quality.

### Required Positive Choices

| If you detect this slop... | Required positive alternative |
|---|---|
| Inter as display font | Must choose a font with structural personality: condensed grotesque, geometric slab, high-contrast serif, or editorial display |
| Default purple/indigo accent (#6366F1, #8B5CF6) | Must derive accent from brand context: industry-appropriate hue, non-default saturation, documented rationale |
| Centered-everything hero layout | Must introduce asymmetry: left-aligned type, offset media, deliberate dead space |
| Generic card grid with equal weights | Must create visual hierarchy: one card leads (larger/accented), others follow |
| Gradient on white background | Must use gradient purposefully or replace with: flat accent block, textured pattern, typographic treatment |
| Rounded pill buttons everywhere | Must define button character rule: either use pill intentionally with rationale, or choose a distinctive alternative (sharp, angled, underline-only) |
| System-default shadow stack | Must customize shadow for brand: color-tinted shadow using accent, ultra-flat with only border, or exaggerated depth for drama |
| Generic blue links | Must define link character: accent color, underline style (offset, thick, animated), or remove underline with other affordance |

### Mandatory Checkable Distinctiveness Rules

These are CHECKABLE requirements, not suggestions. Each anti-pattern below is
paired with a specific positive alternative that MUST be verifiable in the output.
If the positive alternative is not present, the output fails the slop check.

| Anti-pattern detected | REQUIRED positive alternative (verifiable) |
|---|---|
| "No centered-everything" | At least 2 sections MUST use asymmetric layouts from `layout_compositions.md` (split-view, bento, offset grid, or edge-to-edge). Verify by counting sections with non-centered grid templates. |
| "No cookie-cutter rhythm" | Section rhythm MUST alternate between at least 3 distinct layout patterns (e.g., hero-split + feature-grid + bento + metrics-band). Verify by listing each section's pattern type. |
| "No generic rounded corners" | At least 1 component MUST use a non-standard radius or border treatment (e.g., sharp corners on code blocks, asymmetric border-width, accent segment border, mixed radius across variants). Verify by inspecting border-radius and border-style declarations. |
| "No fade-up on scroll" | Motion MUST use choreography tokens (`--dur-fast`, `--dur-base`, `--dur-slow`) with at least 2 distinct motion patterns (e.g., clip-path reveal + staggered entrance, or slide-in + scale-up). Verify by counting distinct animation/transition patterns. |
| "No Inter as display font" | Display font MUST be from a different classification family than the body font. Verify by checking that display is not Inter, system-ui, or the same family as body. |
| "No default purple gradient" | Accent color MUST be derived from brand context with documented rationale, not #6366F1/#8B5CF6/#3B82F6. Verify by checking the hex value in the packet. |

Verification method: count the positive alternatives present. If fewer than 4 of
the 6 rules are satisfied, the output is classified as **Fails distinctiveness**.

### Positive Distinctiveness Requirements

Passing anti-slop requires evidence of good, not just absence of bad:

| Area | Required positive proof |
|---|---|
| Palette | Uses a named neutral family plus documented accent rationale |
| Typography | Display typography expresses a named brand value and has language/script rationale |
| Layout | At least two sections use asymmetric, split, bento, full-bleed, dashboard-density, or editorial patterns where appropriate |
| Rhythm | Page rhythm uses at least three distinct layout patterns across the page |
| Components | Shared components follow `components.character_rules` and `components.component_style_contract` |
| Signature | Signature DNA appears in at least three component categories |
| Motion | Motion has purposeful state/orientation roles, or a documented calm/static strategy |
| Surprise | Any deliberate rule-breaking is recorded in `rule_breaking_budget` and limited to safe locations |

If a design avoids all common AI tells but fails these proof checks, classify it
as **Moderate slop: safe but generic**.

> **Enforcement for safe-but-generic classification:**
> When a design is classified as "safe but generic":
> 1. Flag to user for approval before auto-delivering
> 2. Do not deliver without user acknowledgment
> 3. Suggest 2-3 specific changes to improve distinctiveness:
>    - "Change display font from [current] to [alternative] for more personality"
>    - "Add asymmetric layout to hero section (e.g., 7fr/5fr split instead of centered)"
>    - "Replace default accent with brand-derived color that relates to [product domain]"
>    - "Add signature moment: [specific visual idea that propagates to 3+ components]"

### Distinctiveness Score

After anti-slop check, calculate a distinctiveness score:
- +2 points: custom display font with documented personality rationale
- +2 points: non-default accent with documented derivation
- +1 point: asymmetric hero layout
- +1 point: signature visual element that propagates to ≥ 3 components
- +1 point: typography tension (different font classification families)
- +1 point: custom shadow treatment (not default 3-shadow stack)
- -3 points: Inter as display font
- -3 points: default purple/indigo accent
- -2 points: centered-everything layout
- -2 points: no signature moment

**Minimum passing score: 3. Below 3: regenerate before evaluation.**

### Distinctiveness Coherence Check (Required for Full Systems)

For full design systems (not quick mode), replace the additive scoring above with a binary coherence check. ALL of the following must be true for a passing design:

1. **Non-default font** — Display font is NOT Inter, Roboto, or system-ui
2. **Brand-derived accent** — Accent color is NOT default blue (#3B82F6), indigo (#6366F1), violet (#8B5CF6), or green (#10B981) unless explicitly chosen by the user
3. **Asymmetric section** — At least one hero or major section uses asymmetric layout (not center-aligned column)
4. **Creative brief propagation** — The `creative_brief` statement from the context packet is visible in 3+ component categories (e.g., buttons, cards, inputs, navigation all reflect the stated direction)

**Score should FAIL if any single criterion is missing.** Regenerate before evaluation if coherence check fails.

---

## Integration with Evaluation Checklist

This reference supplements the "Anti-Slop Check" section of `schemas/evaluation_checklist.md`.
During Phase 5 evaluation, run through both checklists:

1. Start with the evaluation checklist for structural quality (tokens, accessibility, responsiveness)
2. Run this slop detection checklist for aesthetic quality
3. Any "Critical" slop finding should block delivery until fixed
4. Any "Major" slop finding should be flagged to the user for approval
