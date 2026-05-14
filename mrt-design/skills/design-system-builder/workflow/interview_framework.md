# Interview Framework

Structured, adaptive discovery workflow for design system creation.
The goal: gather enough context to make informed design decisions without
overwhelming the user with questions.

---

## Interview Philosophy

- **Make recommendations** where the user is unsure — don't leave every decision to them.
- **Infer likely answers** from context (industry, product type, audience).
- **Present options with tradeoffs** rather than open-ended questions.
- **Skip questions** you can answer by examining uploads, codebase, or URLs.
- **Maximum 8-10 questions** before synthesis begins. Group related questions.
- **Batch independent questions** — if 3-4 questions don't depend on each other's answers, ask them all at once in a single message. Respect the user's time. Never ask one question per message when multiple fit.

---

## Discovery Dimensions

These are the things you need to know. Ask about them in roughly this order,
adapting based on what the user already provides.

### 1. Product Identity (essential)
- What is the product/company name?
- What does it do in one sentence?
- Is there an existing brand, logo, or visual identity?
- Any existing website, app, or reference material?

**If they provide a URL:** fetch it and extract colors, fonts, layout patterns.
**If they upload assets:** analyze them before asking anything else.
**If starting from scratch:** skip to aesthetic direction.

### 2. Audience (essential)
- Who uses this product? (developers, consumers, enterprise, creative professionals, etc.)
- What is their trust level on first visit? (cold, warm from referral, existing customer)
- What is their technical sophistication?

**Inference guide:**
| Product type        | Likely audience       | Trust level | Implication              |
|---------------------|----------------------|-------------|--------------------------|
| Developer tool      | Engineers             | Medium      | Technical, dense content  |
| B2B SaaS            | Business decision-makers | Low-medium | Benefit-driven, proof-heavy |
| Consumer app        | General public        | Low         | Simple, friendly, trust-building |
| Agency/studio site  | Prospective clients   | Low         | Portfolio-driven, credible |
| Internal tool       | Known employees       | High        | Dense, functional, minimal onboarding |

### 3. Aesthetic Direction (essential if no existing brand)
Present options rather than asking "what style do you want?"

Use the 14 aesthetic presets from `references/aesthetic_directions.md`:
1. Warm Editorial — magazine-quality, serif-heavy
2. Neon Dashboard — dark canvas, vibrant accents
3. Approachable Enterprise — pastel, rounded, friendly
4. Brutalist Raw — monochrome, no decoration
5. Luxury Premium — dark surfaces, metallic accents
6. Retro Terminal — phosphor green, monospace
7. Earth Organic — warm naturals, handcrafted
8. Tech Blueprint — engineering precision, blueprint-blue
9. Candy Pop — bold, playful, high-energy
10. Swiss Precision — grid-perfect, systematic
11. Wabi-Sabi Serene — organic minimalism, natural restraint
12. Rajwada Splendor — vibrant heritage-inspired luxury
13. Islamic Geometry — structured ornament and geometric rhythm
14. Afrofuture Modern — bold contrast, rhythmic modernity

**How to present:** "Which of these feels closest to what you want?" Show 3-4
that match the product type, not all 10. Include one wildcard.

**If they say "like [brand]":** fetch that brand's site, extract its direction,
and map it to the closest preset as a starting point.

### 4. Voice & Tone (important)
Present voice profiles and let them choose:

| Profile             | Characterized by            | Example words        |
|---------------------|-----------------------------|----------------------|
| Direct/Technical    | Precise, confident, no fluff | ship, build, deploy  |
| Warm/Approachable   | Friendly, helpful, not casual | help, together, easy |
| Authoritative       | Expert, definitive          | proven, established  |
| Playful/Creative    | Witty, surprising           | unleash, craft, wild |

**Default recommendation based on audience:**
- Technical audience -> Direct/Technical
- Consumer audience -> Warm/Approachable
- Enterprise -> Authoritative
- Creative industry -> Playful/Creative

### 5. Content Density (moderate importance)
- Is this a landing page (sparse), a product UI (dense), or editorial (long-form)?
- How many sections does the page/screen need?
- Will copy be provided or should it be generated?

**Default recommendation:** Most landing pages need 5-8 sections:
Hero, Features, Social proof, How it works, Pricing/CTA, Testimonials, Final CTA, Footer.

### 6. Color Direction (moderate importance)
- Does the brand have existing colors?
- Light, dark, or both?
- Warm or cool neutral temperature?

**Only ask if not already answered** by existing brand assets or aesthetic preset choice.

### 7. Typography Direction (low importance — usually derived)
- Serif, sans-serif, or mixed?
- Geometric, humanist, or other?
- What is the primary language and script? Any secondary languages?
- Should typography feel luxurious, comic/playful, friendly, beautiful/editorial,
  technical, authoritative, calm, or crafted?
- Which page part should carry the strongest typographic personality: wordmark,
  hero, section headings, product UI, docs, or data?

**Usually derived from** the aesthetic preset. Only ask if the user expresses a
strong preference, uses a non-Latin or multilingual context, or contradicts the preset.
For non-English or multilingual systems, do not treat typography as low importance:
language/script fit is essential and must be captured before synthesis.

### 8. Technical Constraints (situational)
- Framework preference? (Astro, Next.js, plain HTML, React)
- Target devices? (desktop-only, mobile-first, responsive)
- Any accessibility requirements beyond WCAG AA?
- Performance constraints? (static site, heavy animations)

**Only ask when relevant** — for single HTML artifacts, this rarely matters.

### 9. UI Surfaces Needed (essential for full system)
- What surfaces are needed? (marketing site, app dashboard, docs, admin panel)
- What components are needed? (just landing page or full component library?)

### 10. Competitor & Reference Scan (important)

Research 2-3 live competitor or reference sites before synthesizing. This fills
the gap between the static reference library (`references/design_references.md`)
the user's product lives in.

**When to do it:** After Phase A gathers the product type and audience. Before
aesthetic direction is locked in Phase B.

**How to do it:**
1. Identify 2-3 competitor products in the same space (ask the user, or infer from product type)
2. Fetch and analyze each competitor's site/app:
   - Color palette and temperature
   - Typography choices (display + body fonts)
   - Layout patterns (grid, hero style, section rhythm)
   - Component styling (buttons, cards, forms)
   - Signature visual moments
   - Overall density and tone
3. Document what works (worth borrowing or adapting) and what doesn't (differentiation opportunity)
4. Add 1-2 adjacent-category or aesthetic-forward references when the goal is strong brand distinction,
   inspiration, or innovation

**How to present findings:**
Don't dump raw analysis. Summarize as competitive positioning:
- "Competitor A uses warm neutrals + serif headings — very editorial feel"
- "Competitor B is dark-mode-first with neon accents — very developer-targeted"
- "Nobody in this space uses [pattern] — that's a differentiation opportunity"
- "Adjacent reference C uses [composition/motion/typography idea] that could be adapted without copying"

**When to skip:**
- The user already provided detailed reference sites (analyze those instead)
- Single-component or quick turnaround requests where landscape analysis is overkill
- Internal tools with no external competition

---

## Interview Flow

### Step 0: Read Project Learnings (if available)

Before beginning the interview, check for `LEARNINGS.md` in the project root. If it exists:
1. Read all entries to understand known preferences, patterns, and past decisions
2. Incorporate these learnings into the interview — don't re-ask settled questions
3. Reference specific learnings when making recommendations ("Based on your project's preference for X...")

This step prevents redundant questions and builds on established patterns.

### Phase A: Quick context (1 message, all at once)
Ask all of these in a single message — none depend on each other:
1. "What are you building?" (product name + description)
2. "Do you have existing brand assets, a website, or reference materials?" (if yes, analyze them)
3. "Who is this for?" (audience)
4. "What surfaces do you need?" (marketing site, dashboard, docs — or just a single page?)
5. "Any framework preference?" (React, Vue, Astro, plain HTML — or no preference?)

**Why batch:** Product identity, audience, surfaces, and framework are all independent. Asking one at a time wastes the user's time. Let them answer in any order.

### Phase A.5: Competitor scan (background research)
6. Identify and analyze 2-3 competitor/reference sites
7. Note differentiation opportunities and patterns worth adapting

### Phase B: Direction (1 message)
Ask these together — aesthetic choice informs the rest but the questions themselves are independent:
8. Present 3-4 aesthetic options that fit the product type (informed by competitor scan)
9. "Light, dark, or both?" (color mode)
10. "Dense or spacious?" (content density — or infer from product type)
11. **Risk tolerance question** — ask this when the product type or audience makes the answer non-obvious (see mapping below). Include with Phase B as a single question batched with the others:
    > "How adventurous should the design be? I'll default to [recommended level] for [product type]:
    > - **Safe** — Polished, conventional, no surprises. Focuses on trust and clarity.
    > - **Elevated** — Distinctive personality with one memorable creative move. Still professional.
    > - **Bold** — Pronounced visual identity, pushes beyond typical SaaS/product aesthetics.
    > - **Experimental** — Full creative latitude, may use unconventional CSS techniques."

    **Risk tolerance defaults by product type:**

    | Product type | Default risk_dial | Rationale |
    |---|---|---|
    | Healthcare / legal / finance (high-trust) | `safe` | User credibility depends on visual stability |
    | B2B SaaS / enterprise | `safe` or `elevated` | Trust-first, but differentiation matters |
    | Developer tools | `elevated` | Technical users appreciate precision + personality |
    | Consumer apps | `elevated` | Brand personality drives retention |
    | Creative tools / agencies | `bold` | Audience is design-literate, expects visual ambition |
    | Portfolios / studios | `bold` or `experimental` | The design IS the product |
    | Internal tools | `safe` | Function over form; no trust cost to being boring |

    **When to skip:** When the risk level is clearly implied by the aesthetic preset choice (e.g., choosing Brutalist Raw implies `bold`; choosing Approachable Enterprise implies `safe`). In those cases, document the inferred value and confirm in the Phase D synthesis summary rather than asking.

12. Recommend one likely signature moment direction if the project is brand-forward
13. If language or cultural context is unclear: "What language(s) should the
    interface primarily support, and should the typography feel luxurious,
    playful, friendly, beautiful/editorial, technical, authoritative, calm, or crafted?"

### Phase B.5: Divergence Snapshot (internal synthesis step)

Before locking the packet, create 2-3 concise micro-directions:
- One safest-fit direction
- One expressive/wildcard direction
- One hybrid direction if useful

For each, note:
- Creative thesis
- Differentiator versus competitors
- Likely risk

**Selection criteria for choosing a direction:**
1. **Creative brief fit.** Which micro-direction best matches the `creative_brief.statement`? The chosen direction must have at least one CSS consequence that traces directly to the brief.
2. **Structural variety.** Which direction introduces the most layout-level differentiation (not just token swaps)? Score grid breaks, asymmetric compositions, and spatial logic changes higher than color/type changes.
3. **Risk dial permission.** Which direction uses the highest `risk_dial` permission without exceeding it? The chosen direction should push toward the upper bound of the allowed risk level.

Score each direction 1-3 on all three criteria. Choose the highest scorer, or synthesize the top two if scores are tied. Never default to the safest option without explicit scoring.

Use this to choose a direction deliberately rather than defaulting to the first plausible preset.
Include one typography-specific difference between directions when language,
script, or brand personality is central. Example: "Arabic Kufi hero + Naskh body"
versus "modern Arabic sans throughout" versus "Latin geometric display with Arabic
body fallback."

### Phase C: Details (1 message, only if needed)
Batch any remaining questions that weren't answered above:
11. Specific colors, typography preferences, content, sections, constraints
12. Confirm understanding before proceeding to generation

### Phase D: Synthesis announcement
"I have enough to work with. Here's what I'm building:"
- Summarize the direction in 3-4 bullet points
- Note any assumptions or inferred values
- Ask for confirmation before generating

---

## Question Batching Rules

**Always batch questions that:**
- Don't depend on each other's answers
- Cover different discovery dimensions (product, audience, aesthetic, technical)
- Can be answered in any order

**Never batch questions where:**
- The second question only makes sense given the first answer (e.g., "Do you have a brand?" → "What are your brand colors?")
- One answer might make the other irrelevant (e.g., "Pick an aesthetic" + "What colors do you want?" — the preset already includes colors)

**Format:** Use numbered lists in a single message. Keep each question to one sentence. Add brief context if the user might not know what you're asking.

---

## Decision Helpers

When the user is unsure, use these heuristics:

| User says             | Recommend                  | Why                                  |
|-----------------------|----------------------------|--------------------------------------|
| "I don't know"        | Swiss Precision or Approachable Enterprise | Safe, professional, widely appealing |
| "Modern and clean"    | Swiss Precision            | Systematic, restrained, professional |
| "Premium/luxury"      | Luxury Premium             | Dark surfaces, metallic, restrained  |
| "Techy/developer"     | Brutalist Raw or Tech Blueprint | Technical, precise, dense       |
| "Friendly/approachable" | Approachable Enterprise or Earth Organic | Rounded, warm, low intimidation   |
| "Bold/creative"       | Candy Pop                  | Colorful, playful, high-energy      |
| "Like Stripe"         | Swiss Precision + custom accent | Clean, systematic, trust-building |
| "Like Vercel"         | Brutalist Raw + Neon Dashboard hybrid | Dark, technical, precise  |
| "Like Airbnb"         | Earth Organic + Approachable Enterprise hybrid | Warm, photographic, friendly  |
| "Like Apple"          | Luxury Premium + Swiss Precision hybrid | Minimal, premium, precise |
