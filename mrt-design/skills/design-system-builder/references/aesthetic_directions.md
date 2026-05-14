# Aesthetic Directions

14 curated aesthetic starting points with complete color, font, spacing, and radius specs.
Use these when the user needs inspiration, says "surprise me", or matches a vibe.

Each preset is a starting point — adjust colors, fonts, and spacing to match
the specific brand. Never apply a preset without customization.

**Token names** in the preset tables below use canonical semantic tokens from
`schemas/token_schema.md`: `--fg`, `--fg-muted`, `--accent`, `--bg`, `--bg-alt`,
`--bg-inset` (dark mode inset surfaces), `--on-accent` (text on accent backgrounds).
The `--accent-alt` token is a raw palette extension for dual-accent presets —
agents should map it to a component-level token (e.g., `--badge-bg`, `--chart-secondary`).

---

## 1. Warm Editorial

**Vibe:** Magazine-quality, serif-heavy, generous whitespace. Think: The New Yorker meets a premium SaaS.

| Role         | Token               | Value     |
|--------------|----------------------|-----------|
| Primary      | --fg      | #1C1917   |
| Secondary    | --fg-muted    | #78716C   |
| Accent       | --accent       | #C2410C   |
| Surface      | --bg      | #FAFAF9   |
| Surface Alt  | --bg-alt  | #F5F5F4   |

**Typography:** Libre Baskerville (display) + Source Sans 3 (body) + JetBrains Mono (code)
**Radii:** sm 4px, md 8px, lg 12px — restrained, not bubbly
**Spatial Philosophy:** Space tells the story. Margins are generous and slightly asymmetric. Headlines breathe; body text is contained. Cards have soft borders, not shadows. The grid is a suggestion, not a cage. Whitespace is the primary design element — it creates rhythm between sections without relying on dividers or color breaks.
**Component Character:** Buttons are understated with subtle hover darkening. Cards use thin borders and generous padding. Inputs are clean with bottom-border emphasis. Navigation is horizontal with generous letter-spacing.
**Best for:** Content-heavy sites, publications, portfolio brands, consulting firms

#### Dark Mode Variant
| Token | Light Mode | Dark Mode |
|-------|-----------|-----------|
| --bg | #FAFAF9 | #1A1814 |
| --bg-alt | #F5F5F4 | #252220 |
| --bg-inset | #FFFFFF | #2D2A27 |
| --fg | #1C1917 | #F5F0E8 |
| --fg-muted | #78716C | #9B9488 |
| --accent | #C2410C | #D4A574 |
| --on-accent | #FFFFFF | #1A1814 |
| --border | #E7E5E4 | #3D3833 |

---

## 2. Neon Dashboard

**Vibe:** Dark canvas, vibrant accents, data-dense. Think: trading terminal meets gaming HUD.

| Role         | Token               | Value     |
|--------------|----------------------|-----------|
| Primary      | --fg      | #E4E4E7   |
| Secondary    | --fg-muted    | #71717A   |
| Accent       | --accent       | #22D3EE   |
| Accent Alt   | --accent-alt   | #A78BFA   |
| Surface      | --bg      | #09090B   |
| Surface Alt  | --bg-alt  | #18181B   |

**Typography:** Space Grotesk (display) + IBM Plex Sans (body) + Fira Code (code)
**Radii:** sm 6px, md 10px, lg 14px — slightly softer on dark
**Spatial Philosophy:** Density is a feature. Information is packed but organized — tight grid cells, minimal decorative whitespace, data-first layout. Sidebar navigation is always visible. Sections are bounded by thin lines, not padding. Space between data points is measured in pixels, not rems. The layout optimizes for scanning speed over visual comfort.
**Component Character:** Buttons are compact with sharp functional styling. Cards are dense data containers with monospace labels. Inputs are dark with subtle borders. Navigation is a persistent sidebar with icon+label compact rows.
**Best for:** Developer tools, analytics dashboards, fintech, monitoring platforms

#### Dark Mode Variant
| Token | Light Mode | Dark Mode |
|-------|-----------|-----------|
| --bg | #09090B | #0A0A0F |
| --bg-alt | #18181B | #12121A |
| --bg-inset | #18181B | #16161F |
| --fg | #E4E4E7 | #E8E8ED |
| --fg-muted | #71717A | #6B6B7B |
| --accent | #22D3EE | #22D3EE |
| --on-accent | #09090B | #0A0A0F |
| --border | #27272A | #2A2A35 |

---

## 3. Approachable Enterprise

**Vibe:** Warm, approachable, and trustworthy. Rounded corners, soft palette. Think: Notion meets Calm app.

| Role         | Token               | Value     |
|--------------|----------------------|-----------|
| Primary      | --fg      | #1E293B   |
| Secondary    | --fg-muted    | #64748B   |
| Accent       | --accent       | #0D9488   |
| Surface      | --bg      | #FFFFFF   |
| Surface Alt  | --bg-alt  | #F8FAFC   |

**Typography:** DM Sans (display + body — single family, varied weights)
**Radii:** sm 8px, md 12px, lg 16px — everything is rounded
**Spatial Philosophy:** Generous padding creates breathing room. Cards are elevated with soft shadows, not borders. Sections flow vertically with ample spacing between them. The layout is centered and balanced — symmetry is intentional, not lazy. Whitespace communicates "calm" and "organized."
**Component Character:** Buttons are pill-shaped with gradient hover states. Cards have prominent rounded corners and soft shadows. Inputs are rounded with floating labels. Navigation is clean top-bar with rounded active indicators.
> ⚠️ **This preset is closest to AI-default output.** Do not ship without customization — change the accent, add a signature moment, and ensure asymmetric layouts where the content warrants visual interest.
**Best for:** B2B SaaS, productivity tools, consumer apps, health/wellness

#### Dark Mode Variant
| Token | Light Mode | Dark Mode |
|-------|-----------|-----------|
| --bg | #FFFFFF | #1C1C2E |
| --bg-alt | #F8FAFC | #252540 |
| --bg-inset | #F8FAFC | #2D2D48 |
| --fg | #1E293B | #F0EFF4 |
| --fg-muted | #64748B | #9896A8 |
| --accent | #0D9488 | #2DD4BF |
| --on-accent | #FFFFFF | #1C1C2E |
| --border | #E2E8F0 | #3A3A55 |

---

## 4. Brutalist Raw

**Vibe:** Monochrome, raw, no decoration. Think: exposed concrete architecture meets punk poster.

| Role         | Token               | Value     |
|--------------|----------------------|-----------|
| Primary      | --fg      | #000000   |
| Secondary    | --fg-muted    | #666666   |
| Accent       | --accent       | #FF0000   |
| Surface      | --bg      | #FFFFFF   |
| Surface Alt  | --bg-alt  | #F0F0F0   |

**Typography:** Archivo Black (display) + Space Mono (body — monospaced throughout)
**Radii:** 0px everywhere — sharp corners only
**Borders:** 2-3px solid black, no shadows
**Spatial Philosophy:** No comfort, no padding handouts. Elements butt against each other. Gutters are tight or nonexistent. The grid is rigid and obvious — alignment is absolute. Whitespace is either massive (hero negative space) or zero (dense content blocks). There is no middle ground. Asymmetry is achieved through deliberate misalignment, not gentle offsets.
**Component Character:** Buttons are thick-bordered rectangles. Cards are bordered containers with no rounded corners or shadows. Inputs are brutalist with visible borders. Navigation is stark and typographic.
**Best for:** Creative agencies, art studios, design festivals, developer portfolios

#### Dark Mode Variant
| Token | Light Mode | Dark Mode |
|-------|-----------|-----------|
| --bg | #FFFFFF | #111111 |
| --bg-alt | #F0F0F0 | #1A1A1A |
| --bg-inset | #F0F0F0 | #222222 |
| --fg | #000000 | #F0F0F0 |
| --fg-muted | #666666 | #888888 |
| --accent | #FF0000 | #FFDD00 |
| --on-accent | #FFFFFF | #111111 |
| --border | #000000 | #333333 |

---

## 5. Luxury Premium

**Vibe:** Dark surfaces, metallic accents, restrained. Think: Bugatti showroom or Aesop store.

| Role         | Token               | Value     |
|--------------|----------------------|-----------|
| Primary      | --fg      | #F5F5F0   |
| Secondary    | --fg-muted    | #8A8A80   |
| Accent       | --accent       | #C9A96E   |
| Surface      | --bg      | #0A0A0A   |
| Surface Alt  | --bg-alt  | #141414   |

**Typography:** Cormorant Garamond (display) + Outfit (body, light weight 300-400)
**Radii:** sm 2px, md 4px — minimal rounding
**Details:** Gold (#C9A96E) used sparingly — only for CTAs and key accents
**Spatial Philosophy:** Restraint is the aesthetic. Vast dark spaces frame every element like a gallery wall. Content is centered and isolated — one focal point per viewport. Margins are larger than expected. Sections are separated by elegant negative space, never dividers. The layout whispers wealth through what it chooses not to fill.
**Component Character:** Buttons are minimal with thin gold borders. Cards float on dark surfaces with subtle elevation. Inputs are understated with light-weight typography. Navigation is sparse — few items, generous spacing, uppercase tracking.
**Best for:** Premium brands, luxury goods, high-end hospitality, fashion

#### Dark Mode Variant
| Token | Light Mode | Dark Mode |
|-------|-----------|-----------|
| --bg | #0A0A0A | #0F0F0F |
| --bg-alt | #141414 | #1A1918 |
| --bg-inset | #141414 | #222120 |
| --fg | #F5F5F0 | #F5F0EB |
| --fg-muted | #8A8A80 | #8A8580 |
| --accent | #C9A96E | #D4AF37 |
| --on-accent | #0A0A0A | #0F0F0F |
| --border | #2A2A2A | #302E2C |

---

## 6. Retro Terminal

**Vibe:** Phosphor green on dark, CRT scanlines, hacker aesthetic.

| Role         | Token               | Value     |
|--------------|----------------------|-----------|
| Primary      | --fg      | #33FF33   |
| Secondary    | --fg-muted    | #1A9E1A   |
| Accent       | --accent       | #FFD700   |
| Surface      | --bg      | #0D0D0D   |
| Surface Alt  | --bg-alt  | #1A1A1A   |

**Typography:** VT323 or Share Tech Mono (monospace throughout)
**Radii:** 0-2px — terminal-sharp
**Details:** CRT scanline overlay via CSS, subtle text glow via text-shadow
**Spatial Philosophy:** Everything is gridded in monospace rhythm. Character width dictates spacing — gaps are multiples of `ch` units. Content is stacked vertically like terminal output. No decoration, no flourishes. Width is constrained (80ch max). The page reads top-to-bottom like a log file.
**Component Character:** Buttons are blocky with monospace text and terminal-green accents. Cards are bordered text blocks with monospaced headers. Inputs are monospaced with blinking cursors. Navigation is a command-line-style list.
**Best for:** Developer tools, cybersecurity products, coding bootcamps, gaming

#### Dark Mode Variant
| Token | Light Mode | Dark Mode |
|-------|-----------|-----------|
| --bg | #0D0D0D | #0A0A0A |
| --bg-alt | #1A1A1A | #141414 |
| --bg-inset | #1A1A1A | #1A1A1A |
| --fg | #33FF33 | #33FF33 |
| --fg-muted | #1A9E1A | #1A8C1A |
| --accent | #FFD700 | #33FF33 |
| --on-accent | #0D0D0D | #0A0A0A |
| --border | #2A2A2A | #2A2A2A |

---

## 7. Earth Organic

**Vibe:** Warm naturals, rounded shapes, handcrafted feel. Think: artisan coffee brand.

| Role         | Token               | Value     |
|--------------|----------------------|-----------|
| Primary      | --fg      | #292524   |
| Secondary    | --fg-muted    | #78716C   |
| Accent       | --accent       | #B45309   |
| Surface      | --bg      | #FFFBEB   |
| Surface Alt  | --bg-alt  | #FEF3C7   |

**Typography:** Fraunces (display, variable with "wonk" axis) + Nunito (body, rounded)
**Radii:** sm 8px, md 12px, lg 20px — generous curves
**Spatial Philosophy:** Space flows like water — organic, not gridded. Sections overlap slightly. Backgrounds have warm gradients that bleed into each other. Content is grouped in soft clusters, not rigid columns. Whitespace is warm (tinted, never pure white). The layout feels handmade, with intentional slight irregularities.
**Component Character:** Buttons are rounded with warm hover fills. Cards have generous curves and soft shadows. Inputs are rounded with organic focus rings. Navigation uses warm highlight backgrounds.
**Best for:** Food/beverage, wellness, sustainability, artisan/handmade brands

#### Dark Mode Variant
| Token | Light Mode | Dark Mode |
|-------|-----------|-----------|
| --bg | #FFFBEB | #1A1612 |
| --bg-alt | #FEF3C7 | #252018 |
| --bg-inset | #FEF3C7 | #2D2820 |
| --fg | #292524 | #F0E8D8 |
| --fg-muted | #78716C | #9B8E7A |
| --accent | #B45309 | #C4956A |
| --on-accent | #FFFFFF | #1A1612 |
| --border | #D6D3D1 | #3D3528 |

---

## 8. Tech Blueprint

**Vibe:** Engineering precision, blueprint-blue, structured. Think: NASA control room.

| Role         | Token               | Value     |
|--------------|----------------------|-----------|
| Primary      | --fg      | #E2E8F0   |
| Secondary    | --fg-muted    | #94A3B8   |
| Accent       | --accent       | #38BDF8   |
| Surface      | --bg      | #0F172A   |
| Surface Alt  | --bg-alt  | #1E293B   |

**Typography:** Rajdhani (display, angular) + Inter Tight (body, condensed) + JetBrains Mono
**Radii:** sm 2px, md 4px — engineering-precise
**Details:** Thin grid lines as background pattern, uppercase labels, geometric decoration
**Spatial Philosophy:** Precision is paramount. Every element snaps to a visible or implied grid. Spacing is systematic and mathematically consistent — the same gap everywhere. Sections are delineated by thin structural lines. Content is arranged in technical columns. Margins are tight and functional. The layout should look like it was drafted, not designed.
**Component Character:** Buttons are precise with angular details. Cards are technical containers with status indicators. Inputs are compact with monospace labels. Navigation is tabular with active-state underlines.
**Best for:** Engineering tools, infrastructure, aerospace, scientific computing

#### Dark Mode Variant
| Token | Light Mode | Dark Mode |
|-------|-----------|-----------|
| --bg | #0F172A | #0D1117 |
| --bg-alt | #1E293B | #161B22 |
| --bg-inset | #1E293B | #1C222B |
| --fg | #E2E8F0 | #E6EDF3 |
| --fg-muted | #94A3B8 | #7D8590 |
| --accent | #38BDF8 | #58A6FF |
| --on-accent | #0F172A | #0D1117 |
| --border | #334155 | #30363D |

---

## 9. Candy Pop

**Vibe:** Bold, playful, high-energy. Think: creative agency meets music festival.

| Role         | Token               | Value     |
|--------------|----------------------|-----------|
| Primary      | --fg      | #1A1A2E   |
| Secondary    | --fg-muted    | #6B7280   |
| Accent       | --accent       | #F43F5E   |
| Accent Alt   | --accent-alt   | #8B5CF6   |
| Surface      | --bg      | #FEFCE8   |
| Surface Alt  | --bg-alt  | #FFF7ED   |

**Typography:** Syne (display, bold/quirky) + Outfit (body, clean)
**Radii:** sm 8px, md 16px, lg 24px — playful curves
**Details:** Multiple accents, gradient backgrounds, rotated/tilted elements
**Spatial Philosophy:** Break the grid with intention. Elements overlap, rotate, and escape their containers. Sections use bold color blocking — large areas of color instead of borders. The layout is dynamic and energetic, with diagonal movement and layered elements. Whitespace exists but is punctuated by bursts of color. Every section should feel slightly different from the last.
**Component Character:** Buttons are bold with gradient fills and playful hover animations. Cards are colorful with dynamic hover transforms. Inputs have playful focus states. Navigation uses bold colors and active-state rotations.
**Best for:** Creative agencies, entertainment, youth brands, event sites

#### Dark Mode Variant
| Token | Light Mode | Dark Mode |
|-------|-----------|-----------|
| --bg | #FEFCE8 | #1A1A2E |
| --bg-alt | #FFF7ED | #252540 |
| --bg-inset | #FFF7ED | #2D2D48 |
| --fg | #1A1A2E | #F8F0FF |
| --fg-muted | #6B7280 | #A898B8 |
| --accent | #F43F5E | #FF6B9D |
| --on-accent | #FFFFFF | #1A1A2E |
| --border | #E5E7EB | #3D3D55 |

---

## 10. Swiss Precision

**Vibe:** International Typographic Style. Grid-perfect, Helvetica-adjacent, systematic.

| Role         | Token               | Value     |
|--------------|----------------------|-----------|
| Primary      | --fg      | #111111   |
| Secondary    | --fg-muted    | #767676   |
| Accent       | --accent       | #E63946   |
| Surface      | --bg      | #FFFFFF   |
| Surface Alt  | --bg-alt  | #F5F5F5   |

**Typography:** Instrument Sans (display + body + labels — single family, varied weights)
**Radii:** sm 2px, md 4px — minimal
**Details:** Strong grid, asymmetric layouts, red accent used once per screen, horizontal rules
**Spatial Philosophy:** The grid is absolute law. 12-column grid with mathematical precision. Asymmetry is achieved through content weight, not decorative offset — a 7/5 split is typical. Red accent appears exactly once per viewport. Horizontal rules separate sections. Margins are systematic. The layout achieves visual interest through type scale contrast and spatial proportion, not decoration.
**Component Character:** Buttons are precise and systematic. Cards are clean with sharp edges and strong type hierarchy. Inputs are minimal with bottom borders. Navigation is a structured horizontal bar with dividers.
**Best for:** Professional services, corporate sites, design studios, editorial

#### Dark Mode Variant
| Token | Light Mode | Dark Mode |
|-------|-----------|-----------|
| --bg | #FFFFFF | #141414 |
| --bg-alt | #F5F5F5 | #1E1E1E |
| --bg-inset | #F5F5F5 | #262626 |
| --fg | #111111 | #F5F5F5 |
| --fg-muted | #767676 | #999999 |
| --accent | #E63946 | #FF3333 |
| --on-accent | #FFFFFF | #141414 |
| --border | #E5E5E5 | #333333 |

---

## 11. Wabi-Sabi Serene

**Vibe:** Japanese minimalism. Imperfection, natural textures, muted earth tones. Think: handmade ceramics, weathered wood, handmade paper.

| Role         | Token               | Value     |
|--------------|----------------------|-----------|
| Primary      | --fg      | #3D3D3D   |
| Secondary    | --fg-muted    | #A89984   |
| Accent       | --accent       | #8B9A7B   |
| Surface      | --bg      | #F5F0E8   |
| Surface Alt  | --bg-alt  | #D4C5B2   |

**Typography:** Noto Serif JP or similar serif (display) + clean sans (body)
**Radii:** sm 4px, md 8px, lg 12px — restrained, organic
**Details:** Embrace subtle irregularity — slight offsets, hand-touched edges, soft texture overlays
**Spatial Philosophy:** Empty space is the composition. Inspired by ma (negative space in Japanese aesthetics), elements are placed with contemplative distance. Alignment is approximate, not pixel-perfect. Margins are generous and sometimes asymmetric in ways that feel natural, not calculated. Content breathes. The layout values stillness over dynamism.
**Component Character:** Buttons are understated with organic hover transitions. Cards have subtle texture and gentle shadows. Inputs are clean with minimal decoration. Navigation is sparse and contemplative.
**Best for:** Artisan products, wellness, cultural institutions, mindfulness apps

#### Dark Mode Variant
| Token | Light Mode | Dark Mode |
|-------|-----------|-----------|
| --bg | #F5F0E8 | #1A1815 |
| --bg-alt | #D4C5B2 | #252220 |
| --bg-inset | #D4C5B2 | #2D2A25 |
| --fg | #3D3D3D | #F0E8D8 |
| --fg-muted | #A89984 | #9B9488 |
| --accent | #8B9A7B | #A0B090 |
| --on-accent | #F5F0E8 | #1A1815 |
| --border | #C4B8A0 | #3D3833 |

---

## 12. Rajwada Splendor

**Vibe:** Indian warmth and vibrancy. Rich colors, ornate patterns, layered textures. Think: block-printed textiles, marigold garlands, spice markets.

| Role         | Token               | Value     |
|--------------|----------------------|-----------|
| Primary      | --fg      | #4A1A4B   |
| Secondary    | --fg-muted    | #8B1A1A   |
| Accent       | --accent       | #E8A838   |
| Surface      | --bg      | #FDF6E3   |
| Surface Alt  | --bg-alt  | #1A7A6D   |

**Typography:** Devanagari display fonts (Mukta, Poppins) + warm serif body
**Radii:** sm 6px, md 12px, lg 20px — generous, ornamental
**Details:** Rich color layering, decorative borders, textile-inspired patterns
**Spatial Philosophy:** Layering creates richness. Elements are stacked with overlapping colors and decorative borders, inspired by textile layering. The grid is generous and horizontal, with wide content areas. Sections are separated by decorative dividers, not whitespace alone. Every surface is an opportunity for pattern and color. The layout feels abundant and celebratory.
**Component Character:** Buttons are ornate with layered backgrounds. Cards have decorative borders and rich color fills. Inputs use warm backgrounds with patterned focus states. Navigation is colorful with ornamental active states.
**Best for:** Lifestyle brands, food/beverage, cultural events, hospitality

#### Dark Mode Variant
| Token | Light Mode | Dark Mode |
|-------|-----------|-----------|
| --bg | #FDF6E3 | #1A1018 |
| --bg-alt | #F5EDD0 | #2A1A28 |
| --bg-inset | #F5EDD0 | #352530 |
| --fg | #4A1A4B | #FDF6E3 |
| --fg-muted | #8B6A5A | #B8A090 |
| --accent | #E8A838 | #F0B848 |
| --on-accent | #4A1A4B | #1A1018 |
| --border | #D4B880 | #4A3040 |

---

## 13. Islamic Geometry

**Vibe:** Middle Eastern geometric precision. Interlocking patterns, calligraphic emphasis, gold accents on deep backgrounds. Think: mosque tilework, arabesque patterns, illuminated manuscripts.

| Role         | Token               | Value     |
|--------------|----------------------|-----------|
| Primary      | --fg      | #1A1F3D   |
| Secondary    | --fg-muted    | #2D7D7B   |
| Accent       | --accent       | #C9A84C   |
| Surface      | --bg      | #F0E6D3   |
| Surface Alt  | --bg-alt  | #B85C38   |

**Typography:** Noto Naskh Arabic or similar (display) + clean geometric sans for UI (body)
**Radii:** sm 2px, md 4px, lg 8px — precise, geometric
**Details:** Interlocking geometric patterns, gold accents used sparingly, calligraphic headlines
**Spatial Philosophy:** Geometry governs all space. Layout follows mathematical proportions derived from Islamic geometric principles — recursive subdivision, radial symmetry, interlocking patterns. Content is framed within geometric borders. Gold accents appear as thin structural lines, not fills. The grid is sacred and precise. Whitespace is shaped, not random — negative space forms part of the geometric composition.
**Component Character:** Buttons are geometric with precise borders. Cards feature interlocking pattern accents. Inputs have geometric focus indicators. Navigation uses geometric active states with pattern fills.
**Best for:** Financial services, luxury brands, cultural institutions, education platforms

#### Dark Mode Variant
| Token | Light Mode | Dark Mode |
|-------|-----------|-----------|
| --bg | #F0E6D3 | #0F1225 |
| --bg-alt | #E0D4BC | #1A1F38 |
| --bg-inset | #E0D4BC | #252A48 |
| --fg | #1A1F3D | #F0E6D3 |
| --fg-muted | #6A6A80 | #8A8AA0 |
| --accent | #C9A84C | #D4B860 |
| --on-accent | #1A1F3D | #0F1225 |
| --border | #C0B090 | #3A3A58 |

---

## 14. Afrofuture Modern

**Vibe:** Contemporary African design. Bold geometry, vibrant contrasts, pattern-rich surfaces. Think: Ankara textiles, Ndebele murals, modern African art.

| Role         | Token               | Value     |
|--------------|----------------------|-----------|
| Primary      | --fg      | #1A1A1A   |
| Secondary    | --fg-muted    | #6B21A8   |
| Accent       | --accent       | #E85D26   |
| Accent Alt   | --accent-alt   | #2563EB   |
| Surface      | --bg      | #FACC15   |
| Surface Alt  | --bg-alt  | #1A1A1A   |

**Typography:** Bold geometric display (Outfit, Space Grotesk) + clean sans body
**Radii:** sm 4px, md 8px, lg 16px — bold geometric
**Details:** High-contrast color blocking, pattern-rich surfaces, rhythmic repetition
**Spatial Philosophy:** Bold blocks of color define space. The layout uses large color fields as structural elements — backgrounds, sections, and containers are distinguished by bold color contrast rather than borders or shadows. Rhythmic repetition of patterns creates visual movement. Sections are full-width color blocks. The layout is confident and assertive — elements claim their space boldly.
**Component Character:** Buttons are bold with high-contrast fills. Cards use pattern backgrounds and vivid borders. Inputs have strong color-blocked focus states. Navigation uses color blocking for active indicators.
**Best for:** Creative agencies, tech startups, music/entertainment, fashion

#### Dark Mode Variant
| Token | Light Mode | Dark Mode |
|-------|-----------|-----------|
| --bg | #F8F8F0 | #0E0E10 |
| --bg-alt | #F0F0E0 | #1A1A1E |
| --bg-inset | #F0F0E0 | #252528 |
| --fg | #1A1A1A | #F8F8F0 |
| --fg-muted | #6A6A6A | #909090 |
| --accent | #E85D26 | #FF7040 |
| --on-accent | #FFFFFF | #0E0E10 |
| --border | #D0D0C0 | #3A3A40 |

---

## Usage Mapping

| User says                      | Closest preset       | Adjustment notes                  |
|--------------------------------|----------------------|-----------------------------------|
| "clean and professional"       | Swiss Precision      | Safe default for most businesses  |
| "modern SaaS"                  | Approachable Enterprise  | Add more spacing, softer edges   |
| "dark mode dashboard"          | Neon Dashboard       | Adjust accent to brand color     |
| "luxurious", "high-end"        | Luxury Premium       | Match gold accent to brand metal |
| "fun", "creative", "colorful"  | Candy Pop            | Pick 2-3 brand colors, not all   |
| "natural", "organic", "warm"   | Earth Organic        | Match earth tones to brand       |
| "minimalist", "stripped down"  | Brutalist Raw        | Use only if audience expects it  |
| "editorial", "content-heavy"   | Warm Editorial       | Pair with good typography        |
| "techy", "developer-focused"   | Tech Blueprint       | Or Brutalist Raw as alternative  |
| "retro", "terminal", "hacker"  | Retro Terminal       | Niche, use with intent           |
| "like Stripe"                  | Swiss Precision      | Adjust accent, add illustration style |
| "like Vercel"                  | Brutalist Raw + Neon | Dark, technical, precise         |
| "like Airbnb"                  | Earth Organic + Soft | Warm, photographic, friendly     |
| "like Apple"                   | Luxury Premium + Swiss | Minimal, premium, precise      |
| "Japanese", "zen", "minimal"   | Wabi-Sabi Serene       | Adjust warmth to brand         |
| "Indian", "vibrant", "spice"   | Rajwada Splendor       | Pick 2-3 brand colors          |
| "Arabic", "geometric", "gold"  | Islamic Geometry       | Match gold to brand metal      |
| "African", "bold pattern"      | Afrofuture Modern      | Adjust contrasts to brand      |

---

## Hybrid Approach

Most real brands don't fit one preset exactly. Combine:
- **Surface + neutral temperature** from one preset
- **Accent color + energy** from another
- **Typography direction** from a third
- **Roundness** matching the brand's character

Example: "Like Vercel but warmer" = Brutalist Raw structure + Earth Organic neutrals + custom accent.

---

## Dark Mode Derivation Rules

When generating dark mode from any preset (including hybrid or custom aesthetics):

1. **Backgrounds:** Shift 3-5 steps darker on the neutral scale. Never use pure #000 — use the preset's neutral temperature (warm = reddish dark, cool = bluish dark, neutral = gray dark).
2. **Foregrounds:** Shift to light values that maintain the preset's temperature. Never use pure #FFF unless the preset is stark/technical.
3. **Accent:** Increase saturation by 10-15% and lightness by 5-10% to maintain vibrancy on dark backgrounds. Verify 3:1 contrast on new bg.
4. **Muted text:** Use 50-60% opacity of the foreground color. Must pass 3:1 on dark bg.
5. **Borders:** Use 15-20% opacity foreground on background. Subtle but visible.
6. **Elevated surfaces:** Each step (bg → surface → elevated) increases lightness by 3-5%.
7. **Never invert:** Dark mode is NOT color inversion. It's a deliberate recalibration maintaining the preset's personality.

---

## Preset-to-Packet Quick Reference

When the interview resolves to a known preset, use this mapping to pre-populate
the context packet. All values should be customized — never ship a preset verbatim.

| Preset | neutral_family | temperature | Font Direction | risk_dial | asset_strategy |
|--------|---------------|-------------|----------------|-----------|----------------|
| Warm Editorial | Sandstone | warm | editorial serif + humanist sans | elevated | real assets |
| Neon Dashboard | Ink | cool | technical grotesque + readable sans | bold | generated bitmap |
| Approachable Enterprise | Smoke or Moss | neutral-warm | rounded sans + humanist sans | safe | real assets |
| Brutalist Raw | Concrete | neutral | heavy grotesque or mono + plain sans | bold | CSS pattern |
| Luxury Premium | Brass or Ink | neutral | high-contrast serif + restrained sans | elevated | real assets (licensed) |
| Retro Terminal | Ink | cool | monospace throughout | bold | CSS pattern |
| Earth Organic | Moss | warm | organic serif + warm sans | elevated | real assets |
| Tech Blueprint | Smoke | cool | geometric display + condensed sans | elevated | icon-only |
| Candy Pop | custom high-chroma | warm | expressive display + plain body | bold | generated bitmap |
| Swiss Precision | Concrete or Smoke | neutral | neo-grotesque family | safe | CSS pattern |
| Wabi-Sabi Serene | Sandstone or Moss | warm-neutral | CJK serif + clean sans | safe | real assets |
| Rajwada Splendor | Sandstone or custom | warm | Devanagari display + warm serif | elevated | real assets (licensed) |
| Islamic Geometry | Smoke or custom | cool | Naskh/Kufi display + geometric sans | elevated | CSS pattern |
| Afrofuture Modern | custom bold | warm | bold geometric display + clean sans | bold | generated bitmap |
