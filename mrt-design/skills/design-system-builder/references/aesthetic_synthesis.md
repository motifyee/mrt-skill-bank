# Aesthetic Synthesis Engine

This document is the creative engine for generating novel aesthetics beyond static presets. It provides structured tools for synthesizing new design directions from mood, context, brand attributes, language/script needs, and narrative descriptions.

---

## Mood Matrix

Maps 30 mood/feeling words to concrete design properties. Use this to translate abstract brand attributes into specific token decisions.

### Cultural Context

Design aesthetics are culturally situated. Before applying mood properties, consider the product's cultural context:

| Cultural Context | Typical Weight | Typical Roundness | Typical Density | Accent Discipline |
|---|---|---|---|---|
| Western / Northern European | light-medium | moderate | sparse-comfortable | Single accent, restrained |
| East Asian (Japanese, Korean) | light | moderate | comfortable-sparse | Single accent, nature-derived |
| South Asian (Indian, etc.) | medium-heavy | round | comfortable-dense | Multi-accent, vibrant layering |
| Middle Eastern / North African | medium | sharp-moderate | dense | Gold/warm accent, geometric pattern |
| Sub-Saharan African | heavy | moderate-round | dense | Bold multi-accent, high contrast |
| Latin American | medium-heavy | round | comfortable | Warm multi-accent, expressive |

These are starting tendencies, not rules. Individual brands within any culture may deliberately subvert these defaults. Always verify with the user during discovery.

| Mood | Temperature | Saturation | Weight | Roundness | Density | Energy | Script Context |
|------|------------|-----------|--------|-----------|---------|--------|
| Confident | neutral | moderate | heavy | moderate | comfortable | calm | Latin |
| Playful | warm | vivid | light | round | sparse | dynamic | Latin |
| Authoritative | cool | muted | heavy | sharp | dense | calm | Latin |
| Serene | cool | muted | light | moderate | sparse | calm | CJK |
| Energetic | warm | vivid | medium | round | comfortable | dynamic | Latin |
| Luxurious | neutral | muted | medium | moderate | sparse | calm | Latin |
| Trustworthy | cool | moderate | medium | moderate | comfortable | calm | Latin |
| Innovative | cool | vivid | light | sharp | sparse | dynamic | Latin |
| Nostalgic | warm | muted | medium | round | comfortable | calm | Latin |
| Futuristic | cool | vivid | light | sharp | sparse | dynamic | Latin |
| Organic | warm | moderate | light | round | comfortable | moderate | Latin |
| Technical | cool | muted | medium | sharp | dense | moderate | Latin |
| Minimal | neutral | muted | light | moderate | sparse | calm | Latin |
| Bold | warm | vivid | heavy | moderate | dense | dynamic | Latin |
| Friendly | warm | moderate | light | round | comfortable | moderate | Latin |
| Sophisticated | cool | muted | medium | moderate | comfortable | calm | Latin |
| Raw | warm | muted | heavy | sharp | dense | moderate | Latin |
| Refined | cool | moderate | light | moderate | sparse | calm | Latin |
| Mysterious | cool | muted | heavy | moderate | dense | calm | Arabic |
| Honest | warm | moderate | medium | moderate | comfortable | moderate | Latin |
| Disruptive | warm | vivid | heavy | sharp | dense | dynamic | Latin |
| Classic | warm | muted | medium | moderate | comfortable | calm | Latin |
| Whimsical | warm | vivid | light | round | sparse | dynamic | Latin |
| Clinical | cool | muted | light | sharp | sparse | calm | Latin |
| Artisan | warm | moderate | medium | round | comfortable | moderate | CJK |
| Corporate | cool | muted | medium | moderate | dense | calm | Latin |
| Adventurous | warm | vivid | medium | moderate | comfortable | dynamic | Latin |
| Grounded | warm | muted | heavy | moderate | dense | calm | Devanagari |
| Electric | cool | vivid | medium | sharp | dense | dynamic | Latin |
| Understated | neutral | muted | light | moderate | sparse | calm | Latin |

**Script Context notes for underrepresented scripts:**
- **Cyrillic:** Inherits Latin mood weights as a baseline. Cyrillic display faces (e.g., PT Sans, IBM Plex) tend to run heavier — reduce weight one step (bold → semibold) for equivalent visual presence.
- **Hebrew:** Use "Authoritative" or "Sophisticated" Latin mood as starting point. Hebrew letterforms are naturally dense — increase line-height by 0.05–0.1 and reduce letter-spacing toward 0 or slightly positive. RTL alignment requires mirrored grid layouts.
- **Bengali:** Use "Organic" or "Friendly" Latin mood. Bengali script has complex ligatures — prefer larger body sizes (17–18px minimum) and generous line-height (1.7+).
- **Tamil:** Use "Technical" or "Refined" Latin mood. Tamil has tall ascending/descending glyphs — increase line-height to 1.65+ and ensure vertical rhythm accommodates the full glyph height range.
- **Thai:** Use "Friendly" or "Elegant" Latin mood. Thai has no word spaces — spacing and padding are the primary rhythm tools. Use generous block spacing between sections since intra-word spacing is unavailable.

For any script not listed here, use the closest cultural neighbor's mood weights and verify against `typography_selection.md`.

### How to Read the Matrix

- **Temperature** drives the neutral scale: warm neutrals lean beige/cream, cool lean blue-grey, neutral stays true grey.
- **Saturation** sets the accent chroma: muted accents are desaturated, vivid accents are fully saturated.
- **Weight** maps to font weight and stroke width: light = thin strokes, heavy = thick strokes.
- **Roundness** sets border-radius tokens: sharp = 0-4px, moderate = 8-12px, round = 16-24px+.
- **Density** controls spacing scale: sparse = generous whitespace, dense = compact layouts.
- **Energy** determines motion and interaction style: calm = subtle transitions, dynamic = assertive animations.
- **Script Context** indicates the primary writing system the design should support: Latin, CJK, Arabic, Devanagari, or Multi-script. This affects font selection, line-height, and text layout direction.

> When the script context is non-Latin, the font pairing engine should prefer region-appropriate display fonts. See `aesthetic_directions.md` non-Western presets for font recommendations by tradition. The synthesis algorithm weights the primary mood at 60% but does NOT override cultural context — if the brand serves an East Asian audience, CJK font support takes precedence over Latin font aesthetics.

### Blending Moods

Products rarely express a single mood. Blend two moods by averaging their properties, with the primary mood weighted 60/40 over the secondary. Example:

- Primary: Trustworthy (cool, moderate, medium, moderate, comfortable, calm)
- Secondary: Innovative (cool, vivid, light, sharp, sparse, dynamic)
- Result: cool, moderate-vivid, medium-light, moderate-sharp, comfortable-sparse, calm-dynamic

This blend produces a trustworthy-but-forward design: conservative spacing, slight edge in roundness, accent saturation above average, controlled energy in interactions.

---

## Language-Aware Font Pairing Engine

Rules for generating valid display + body font pairings. Every pairing must satisfy
the rules below and the language-aware process in `references/typography_selection.md`.
Font selection is a brand-expression decision, not a generic aesthetic preference.

### Pairing Rules

1. **Script rule**:
   - Identify the primary language and script before selecting Latin candidates.
   - Prefer fonts designed well for the primary script over fonts that only have
     incidental glyph coverage.
   - For multilingual systems, define the primary-script stack first, then harmonize
     secondary-language stacks by visual size, stroke weight, contrast, and rhythm.
   - Do not use Latin letter-spacing, line-height, or display scale defaults blindly
     for Arabic, CJK, Devanagari, Bengali, Tamil, Hebrew, Thai, or Cyrillic systems.

2. **Essence rule**:
   - Each selected font must express at least one named brand value such as luxury,
     friendliness, comic energy, beauty, authority, precision, craft, calm, or speed.
   - Record the value in `typography.font_rationale.*.expresses`.
   - If a font does not express a brand value or improve legibility, reject it.

3. **Role rule**:
   - Assign separate typographic roles for brand mark, hero, section heading, body,
     UI labels, data, docs, and code when needed.
   - The most expressive font belongs in brand/hero contexts, not dense controls.
   - Body, forms, settings, tables, and documentation must prioritize comprehension.

4. **Contrast rule**: Paired fonts must differ in at least 2 of these dimensions:
   - Serif vs sans-serif
   - Geometric vs humanist vs grotesque
   - Weight range (light body vs heavy display, or vice versa)
   - Proportional vs monospaced

5. **Harmony rule**: Paired fonts must share:
   - Similar x-height ratios (within ~10%)
   - Compatible character widths (neither condensed paired with extended)
   - Consistent baseline alignment at matching sizes

6. **Context rule**:
   - Display font carries personality (used for headings, hero text, brand marks)
   - Body font prioritizes readability (used for paragraphs, UI labels, data)
   - If one font is decorative, the other must be highly neutral

7. **Distinctiveness rule**:
   - Record each chosen pairing as `common`, `distinctive`, or `specialist`.
   - Common Google-font pairings are allowed for speed or strict open-source
     requirements, but the decision log must explain why a less-used pairing
     was rejected.
   - For non-Latin scripts, use Noto as a reliable fallback rather than the
     default premium recommendation when licensed fonts are possible.

### Distinctive Type Sources

Use these as prompts when the project can support licensed or less-common type.
Always provide an open-source fallback in `substitutions`.

| Script/Context | Professional Sources | Fallback Strategy |
|---|---|---|
| Latin editorial/luxury | Klim, Commercial Type, Pangram Pangram, Sharp Type | Newsreader, Cormorant, Source Serif 4 |
| Latin technical/product | Dinamo, Colophon, Grilli Type, Type Network | IBM Plex, Barlow, Archivo |
| Japanese | Type Project, Fontworks, Morisawa | Shippori Mincho, Zen Kaku Gothic New |
| Devanagari/Indian scripts | Indian Type Foundry, Ek Type | Mukta, Hind, Tiro Devanagari |
| Arabic | TPTQ Arabic, Boutros Fonts, 29LT | Amiri, Noto Naskh Arabic, Reem Kufi |

### Expressive Font Attribute Matrix

Use this matrix before choosing actual font names.

| Brand value | Useful type traits | Good page roles |
|---|---|---|
| Luxury | high contrast, elegant curves, calligraphic or refined serif influence | brand mark, hero, premium section headings |
| Beauty | graceful proportions, generous whitespace, optical-size sensitivity | hero, editorial headings, portfolio captions |
| Comic / playful | expressive display, rounded or irregular forms, controlled bounce | hero, campaign headings, empty states |
| Friendly | humanist rhythm, open counters, moderate weight, soft terminals | body, onboarding, support, buttons |
| Technical | engineered grotesque, monospace accents, clear numerals | dashboard, docs, labels, code |
| Authority | stable serif or institutional sans, low novelty, strong hierarchy | enterprise, healthcare, legal, finance |
| Craft | organic serif, tactile irregularity, warm text color | food, wellness, handmade, cultural brands |
| Calm | low contrast, relaxed line height, restrained weight jumps | healthcare, mindfulness, docs, settings |

### Page-Part Assignment

After selecting fonts, assign them to page parts:

| Page part | Required decision |
|---|---|
| Brand mark / wordmark | Highest identity signal; may use custom or expressive type |
| Hero display | Emotional thesis; can be most expressive if readable |
| Section headings | Reinforce identity with less drama than hero |
| Body copy | Reading comfort and trust |
| Navigation and buttons | Orientation and action clarity |
| Data tables and charts | Tabular numerals, stable metrics, low decoration |
| Documentation | Paragraph comfort, code adjacency, scan rhythm |
| Captions/helpers | Small-size legibility and calm support |

### Validated Pairings by Mood Family

#### Warm and Friendly

Personality-driven but approachable. Rounded features, open letterforms, warm impression.

| Display Font | Body Font | Contrast Dimensions | Open-Source Equivalent |
|---|---|---|---|
| Fraunces | Nunito | Serif vs sans, organic vs geometric | (all open-source) |
| Playfair Display | Source Sans 3 | Didone serif vs humanist sans | (all open-source) |
| Lora | Open Sans | Transitional serif vs humanist sans | (all open-source) |
| Roslindale | Nunito Sans | Display serif vs geometric sans | (all open-source) |
| Bricolage Grotesque | Karla | Variable grotesque vs humanist sans | (all open-source) |

#### Technical and Precise

Structured, systematic, engineered feel. Monospaced or geometric influences.

| Display Font | Body Font | Contrast Dimensions | Open-Source Equivalent |
|---|---|---|---|
| Space Grotesk | IBM Plex Sans | Geometric grotesque vs neo-grotesque | (all open-source) |
| JetBrains Mono | Inter | Monospaced vs proportional, weight | (all open-source) |
| Rajdhani | Barlow | Geometric display vs grotesque | (all open-source) |
| Space Mono | IBM Plex Sans | Monospaced vs humanist sans | (all open-source) |
| Archivo | Source Sans 3 | Grotesque vs humanist | (all open-source) |

#### Luxury and Premium

High contrast, refined proportions, restrained elegance.

| Display Font | Body Font | Contrast Dimensions | Open-Source Equivalent |
|---|---|---|---|
| Cormorant Garamond | Outfit | Display serif vs geometric sans | (all open-source) |
| Bodoni Moda | Sora | Didone serif vs geometric sans | (all open-source) |
| Libre Caslon Text | DM Sans | Caslon revival vs geometric sans | (all open-source) |
| Playfair Display | Sora | Didone vs geometric, weight | (all open-source) |
| DM Serif Display | Outfit | Slab-influenced serif vs geometric sans | (all open-source) |

#### Bold and Creative

Expressive, high-impact, personality-first. Used for brands that need to stand out.

| Display Font | Body Font | Contrast Dimensions | Open-Source Equivalent |
|---|---|---|---|
| Syne | Work Sans | Geometric display vs grotesque | (all open-source) |
| Archivo Black | Karla | Ultra-bold grotesque vs humanist | (all open-source) |
| Bricolage Grotesque | Outfit | Variable grotesque vs geometric | (all open-source) |
| Clash Display | Satoshi | Geometric display vs neo-grotesque | Syne or Space Grotesk + Inter |
| Instrument Serif | Inter | Slab-serif vs neo-grotesque | (all open-source) |

#### Minimal and Swiss

Reduction, clarity, grid-driven. Often single-family with weight variation.

| Display Font | Body Font | Contrast Dimensions | Open-Source Equivalent |
|---|---|---|---|
| Inter Tight | Inter | Display cut vs text cut, weight | (all open-source) |
| Manrope | Manrope | Single family, weight + size contrast only | (all open-source) |
| Helvetica Neue | Helvetica Neue | Single family, weight variation | IBM Plex Sans (open-source) |
| Akzidenz Grotesk | Akzidenz Grotesk | Single family, weight + tracking | IBM Plex Sans or Libre Franklin (open-source) |
| Suisse Int'l | Suisse Int'l | Single family, weight + case variation | Inter or Libre Franklin (open-source) |

#### Organic and Natural

Humanist influences, imperfect details, warm but not cute.

| Display Font | Body Font | Contrast Dimensions | Open-Source Equivalent |
|---|---|---|---|
| Fraunces (wonk axis) | Nunito | Variable serif with quirk vs geometric sans | (all open-source) |
| Newsreader | Source Sans 3 | Old-style serif vs humanist sans | (all open-source) |
| Roslindale | DM Sans | Display serif vs geometric sans | (all open-source) |
| Lora | Lato | Transitional serif vs humanist sans | (all open-source) |
| Cooper Hewitt | Source Sans 3 | Geometric serif vs humanist sans | (all open-source) |

#### Corporate and Institutional

Conservative, established, trustworthy. Favours well-known families.

| Display Font | Body Font | Contrast Dimensions | Open-Source Equivalent |
|---|---|---|---|
| Roboto Slab | Roboto | Slab vs sans within same family | (all open-source) |
| Merriweather | Open Sans | Serif vs sans, weight | (all open-source) |
| IBM Plex Serif | IBM Plex Sans | Serif vs sans within same family | (all open-source) |
| Source Serif 4 | Source Sans 3 | Serif vs sans within same family | (all open-source) |
| PT Serif | PT Sans | Serif vs sans within same family | (all open-source) |

#### Dark and Editorial

High-contrast, dramatic, magazine-quality. For brands with edge or authority.

| Display Font | Body Font | Contrast Dimensions | Open-Source Equivalent |
|---|---|---|---|
| Bodoni Moda | Inter | Didone serif vs neo-grotesque | (all open-source) |
| Clash Display | Inter | Geometric display vs neo-grotesque | Syne or Space Grotesk + Inter (Clash Display commercial; Inter open-source) |
| Canela | Graphik | Display serif vs grotesque | Playfair Display + DM Sans or Inter |
| GT America | GT America | Single family, compressed vs regular width | Libre Franklin (open-source) |
| Tanzkur | Neue Haas Grotesk | Didone display vs neo-grotesque | Playfair Display + IBM Plex Sans |

#### East Asian — Japanese, Korean, Chinese

Clean, refined, with strong typographic tradition. CJK display fonts paired with Latin body.

| Display Font | Body Font | Contrast Dimensions | Notes |
|---|---|---|---|
| Noto Serif JP | Noto Sans JP | Serif vs sans within CJK family | Japanese; Google Fonts |
| Shippori Mincho | Zen Kaku Gothic New | Traditional mincho vs gothic | Japanese; Google Fonts |
| Noto Serif KR | Noto Sans KR | Serif vs sans within Korean family | Korean; Google Fonts |
| Noto Serif SC | Noto Sans SC | Serif vs sans within Chinese family | Simplified Chinese; Google Fonts |
| LXGW WenKai | Noto Sans SC | Calligraphic vs gothic | Chinese; open-source |

For products targeting both CJK and Latin audiences, pair a CJK serif display with a Latin sans body. Ensure both font families are loaded and the `font-family` stack lists CJK first for CJK text.

#### South Asian — Devanagari, Bengali, Tamil

Warm, expressive, with rich calligraphic traditions. Devanagari display fonts paired with Latin or Devanagari body.

| Display Font | Body Font | Contrast Dimensions | Notes |
|---|---|---|---|
| Mukta Malar | Mukta | Display weight vs regular weight within Devanagari family | Hindi, Marathi; Google Fonts |
| Noto Serif Devanagari | Noto Sans Devanagari | Serif vs sans within Devanagari family | Hindi, Marathi, Nepali; Google Fonts |
| Hind Siliguri | Noto Sans Bengali | Display weight vs body weight for Bengali | Bengali; Google Fonts |
| Noto Serif Tamil | Noto Sans Tamil | Serif vs sans within Tamil family | Tamil; Google Fonts |
| Poppins | Noto Sans Devanagari | Latin geometric display + Devanagari body | Bilingual Latin/Devanagari |

#### Middle Eastern — Arabic, Persian, Urdu

Geometric precision, calligraphic emphasis, right-to-left considerations. Naskh/Nastaliq display paired with geometric sans.

| Display Font | Body Font | Contrast Dimensions | Notes |
|---|---|---|---|
| Noto Naskh Arabic | Noto Sans Arabic | Naskh serif vs sans within Arabic family | Arabic, Persian, Urdu; Google Fonts |
| Amiri | Noto Kufi Arabic | Traditional Naskh vs modern Kufi | Arabic; Google Fonts |
| Reem Kufi | Noto Sans Arabic | Display Kufi vs text Naskh | Arabic; Google Fonts |
| Lateef | Noto Naskh Arabic | Calligraphic display vs clean text Naskh | Arabic, Urdu; Google Fonts |
| Harmattan | Noto Sans Arabic | West African Arabic style vs standard Naskh | Arabic; Google Fonts |

For RTL layouts, always set `dir="rtl"` on the HTML element and use CSS logical properties (`margin-inline-start` instead of `margin-left`) throughout the design system.

---

## Color Narrative System

Instead of picking colors from a generic "warm" or "cool" label, generate palettes from evocative narrative descriptions. Each narrative produces a complete palette with semantic roles assigned.

### Named Neutral Families

Use neutral families as the core palette material. This prevents the safe-middle
trap of "grey plus a temperature nudge."

| Family | Scale | Best For |
|---|---|---|
| Sandstone | `#2A1F17` / `#4A3828` / `#7A6550` / `#A8937D` / `#D4C4B0` / `#F0E8DC` | Editorial, hospitality, warm trust |
| Concrete | `#121214` / `#1E1E22` / `#35353C` / `#5A5A64` / `#909098` / `#C8C8CE` | Industrial, brutalist, precise systems |
| Ink | `#08080C` / `#101018` / `#1E1E2A` / `#353544` / `#606072` / `#A0A0B0` | Dark-first, premium, technical depth |
| Moss | `#121C15` / `#1F3325` / `#3A5640` / `#6B8E72` / `#A8C4AD` / `#DDE8DF` | Organic, grounded, sustainability |
| Smoke | `#101416` / `#1E2428` / `#3A4248` / `#6A757C` / `#A8B2B8` / `#DEE3E6` | Clinical, calm technical, minimal |
| Brass | `#1C1810` / `#332C1A` / `#5A4D30` / `#8A7A55` / `#C4B68A` / `#EDE5D0` | Luxury, institutional, heritage |

### Hex Lookup Table

When the packet specifies a `neutral_family`, resolve to raw palette tokens using
the scale position order: darkest → lightest maps to `--neutral-900` through `--neutral-400`
(add `--neutral-50` / `--neutral-100` from the lightest two stops).

| Family | --neutral-900 | --neutral-800 | --neutral-700 | --neutral-600 | --neutral-500 | --neutral-400 |
|--------|---------------|---------------|---------------|---------------|---------------|---------------|
| Sandstone | #2A1F17 | #4A3828 | #7A6550 | #A8937D | #D4C4B0 | #F0E8DC |
| Concrete | #121214 | #1E1E22 | #35353C | #5A5A64 | #909098 | #C8C8CE |
| Ink | #08080C | #101018 | #1E1E2A | #353544 | #606072 | #A0A0B0 |
| Moss | #121C15 | #1F3325 | #3A5640 | #6B8E72 | #A8C4AD | #DDE8DF |
| Smoke | #101416 | #1E2428 | #3A4248 | #6A757C | #A8B2B8 | #DEE3E6 |
| Brass | #1C1810 | #332C1A | #5A4D30 | #8A7A55 | #C4B68A | #EDE5D0 |

### Nature Narratives

**Northern Ocean at Dusk**
- Narrative: Deep water under fading light, foam at the shore, cold horizon.
- Accent: `#4A90B8` (slate blue)
- Neutral scale: `#0F1B2D` / `#1E2D45` / `#3A4F6B` / `#8B9DB5` / `#C4D0DE` / `#E8EDF3`
- Surface: `#F5F7FA`
- Semantic: success `#3B8A6E`, error `#C75D5D`, warning `#D4A24E`
- Temperature: cool
- Family: Technical, Trustworthy

**Desert Sunrise**
- Narrative: Warm sand, terracotta ridges, dusty sky, early light.
- Accent: `#C2784E` (terracotta)
- Neutral scale: `#2A1F17` / `#4A3828` / `#7A6550` / `#A8937D` / `#D4C4B0` / `#F0E8DC`
- Surface: `#FAF5EE`
- Semantic: success `#6B8E5A`, error `#B54545`, warning `#D49A3E`
- Temperature: warm
- Family: Artisan, Organic

**Forest Canopy**
- Narrative: Deep green, filtered light through leaves, bark and moss.
- Accent: `#3D6B4F` (forest green)
- Neutral scale: `#121C15` / `#1F3325` / `#3A5640` / `#6B8E72` / `#A8C4AD` / `#DDE8DF`
- Surface: `#F2F6F3`
- Semantic: success `#4A7C5C`, error `#A14B4B`, warning `#C4953A`
- Temperature: cool-warm
- Family: Organic, Grounded

**Alpine Winter**
- Narrative: Snow on granite, thin air, blue shadows, birch bark.
- Accent: `#5B7FA5` (icy blue)
- Neutral scale: `#0D1520` / `#1A2A3A` / `#3A5068` / `#7A95AE` / `#B8C8D8` / `#E5ECF2`
- Surface: `#F7FAFC`
- Semantic: success `#4E8268`, error `#A55252`, warning `#B8923E`
- Temperature: cool
- Family: Clinical, Serene

**Volcanic Earth**
- Narrative: Cooled lava, mineral deposits, sulphur vents, dark stone.
- Accent: `#D4763A` (magma orange)
- Neutral scale: `#1A1210` / `#2D2320` / `#504035` / `#7A6A5A` / `#B5A898` / `#E5DDD5`
- Surface: `#F8F4F0`
- Semantic: success `#5A8A50`, error `#C44545`, warning `#D49A3A`
- Temperature: warm
- Family: Bold, Raw

**Coral Reef**
- Narrative: Living coral, shallow turquoise, white sand, deep water.
- Accent: `#E86B56` (living coral)
- Neutral scale: `#1A1520` / `#2D2535` / `#4A3D52` / `#7A6B85` / `#BDB0C8` / `#EBE5F0`
- Surface: `#F8F5FA`
- Semantic: success `#3BA88A`, error `#D45656`, warning `#D4A24E`
- Temperature: warm-cool
- Family: Playful, Energetic

**Golden Prairie**
- Narrative: Tall dry grass, distant tree line, big sky, warm wind.
- Accent: `#C49A3A` (wheat gold)
- Neutral scale: `#1C1810` / `#332C1A` / `#5A4D30` / `#8A7A55` / `#C4B68A` / `#EDE5D0`
- Surface: `#FAF6EA`
- Semantic: success `#5A8A40`, error `#B54545`, warning `#D4883A`
- Temperature: warm
- Family: Classic, Grounded

**Arctic Lichen**
- Narrative: Pale rock, pale green growth, frost, mineral grey.
- Accent: `#7A9E7E` (lichen green)
- Neutral scale: `#101416` / `#1E2428` / `#3A4248` / `#6A757C` / `#A8B2B8` / `#DEE3E6`
- Surface: `#F2F4F5`
- Semantic: success `#5A8E60`, error `#A45050`, warning `#B89240`
- Temperature: cool
- Family: Clinical, Minimal

### Urban Narratives

**Tokyo Neon After Rain**
- Narrative: Dark asphalt reflecting electric signs, wet surfaces, high contrast.
- Accent: `#00D4FF` (electric cyan)
- Neutral scale: `#0A0A0F` / `#14141F` / `#252535` / `#45455A` / `#8A8AA0` / `#D0D0DD`
- Surface: `#0F0F18`
- Semantic: success `#00E89A`, error `#FF4466`, warning `#FFD23A`
- Temperature: cool
- Family: Futuristic, Disruptive

**London Fog**
- Narrative: Warm grey stone, muted gold details, charcoal ironwork, cream paper.
- Accent: `#B89A50` (muted gold)
- Neutral scale: `#1A1816` / `#2E2B26` / `#4A4540` / `#7A756E` / `#B0A99E` / `#DDD7CE`
- Surface: `#F5F1EB`
- Semantic: success `#5A8A5A`, error `#A85050`, warning `#C49A3A`
- Temperature: warm-neutral
- Family: Sophisticated, Classic

**Mediterranean Villa**
- Narrative: White stucco walls, terracotta tile, olive trees, deep blue sea.
- Accent: `#C45A3A` (terracotta)
- Neutral scale: `#1A1614` / `#2E2824` / `#504640` / `#8A7E74` / `#C4B8A8` / `#EDE5DA`
- Surface: `#FAF6F0`
- Semantic: success `#5A8A50`, error `#B54545`, warning `#D49A3A`
- Temperature: warm
- Family: Friendly, Organic

**Brutalist Downtown**
- Narrative: Raw concrete, exposed rebar, graffiti accent, overcast sky.
- Accent: `#E84040` (safety red / graffiti)
- Neutral scale: `#121214` / `#1E1E22` / `#35353C` / `#5A5A64` / `#909098` / `#C8C8CE`
- Surface: `#F0F0F2`
- Semantic: success `#40A060`, error `#D44040`, warning `#C49030`
- Temperature: neutral
- Family: Raw, Bold

**Scandinavian Apartment**
- Narrative: Blonde wood, white walls, muted textiles, black detail.
- Accent: `#4A6B5A` (sage)
- Neutral scale: `#0F1210` / `#1A201C` / `#303830` / `#5A6458` / `#98A494` / `#D5DDD2`
- Surface: `#F5F8F4`
- Semantic: success `#4A7A50`, error `#A05050`, warning `#B8923A`
- Temperature: cool-warm
- Family: Minimal, Refined

**Night Market**
- Narrative: Warm lantern light, deep shadows, saturated food colors, textile patterns.
- Accent: `#E87040` (lantern orange)
- Neutral scale: `#140E0A` / `#261A12` / `#4A3020` / `#7A5840` / `#B89070` / `#E5D0B8`
- Surface: `#1A1210`
- Semantic: success `#60A050`, error `#E04545`, warning `#E8B030`
- Temperature: warm
- Family: Adventurous, Bold

### Material Narratives

**Brushed Aluminum**
- Narrative: Industrial precision, machined surfaces, clean reflection.
- Accent: `#4A7AAA` (blue steel)
- Neutral scale: `#0D1014` / `#1A1E24` / `#2E343C` / `#5A626C` / `#A0A8B0` / `#D8DCE0`
- Surface: `#F4F6F8`
- Semantic: success `#4A8A6A`, error `#AA4A4A`, warning `#AA8A3A`
- Temperature: cool
- Family: Technical, Clinical

**Aged Leather**
- Narrative: Cognac patina, brass hardware, cream stitching, warm depth.
- Accent: `#8A5A30` (cognac)
- Neutral scale: `#1A1410` / `#2E2418` / `#504028` / `#7A6848` / `#B0A080` / `#DED0B8`
- Surface: `#F8F2E8`
- Semantic: success `#5A8A40`, error `#A84545`, warning `#C49030`
- Temperature: warm
- Family: Luxurious, Classic

**Raw Concrete**
- Narrative: Cool aggregate, steel ties, white form-work marks, rust stains.
- Accent: `#A85030` (rust)
- Neutral scale: `#141416` / `#222226` / `#3A3A40` / `#606068` / `#9A9AA0` / `#D0D0D4`
- Surface: `#EEEEF0`
- Semantic: success `#4A8A58`, error `#B04040`, warning `#A89030`
- Temperature: cool-neutral
- Family: Raw, Bold

**Matte Ceramic**
- Narrative: Smooth matte glaze, soft pastel, tactile surface, kiln warmth.
- Accent: `#7A8AA0` (blue-grey glaze)
- Neutral scale: `#141618` / `#22262A` / `#3A3E44` / `#5A6068` / `#9AA0A8` / `#D2D6DA`
- Surface: `#F6F8FA`
- Semantic: success `#5A9070`, error `#B05858`, warning `#B89A3A`
- Temperature: cool-warm
- Family: Refined, Understated

**Oiled Wood**
- Narrative: Warm grain, amber finish, brass inlay, natural variation.
- Accent: `#9A7A40` (amber)
- Neutral scale: `#1C1610` / `#302618` / `#504028` / `#7A6840` / `#B0A070` / `#DED4B8`
- Surface: `#FAF4E6`
- Semantic: success `#5A8A40`, error `#A84545`, warning `#C49830`
- Temperature: warm
- Family: Artisan, Organic

**Frosted Glass**
- Narrative: Translucent white, soft diffusion, cool undertone, light passing through.
- Accent: `#6A8AAA` (frosted blue)
- Neutral scale: `#0E1218` / `#1A2028` / `#2E3640` / `#505A68` / `#8A94A2` / `#C8D0D8`
- Surface: `#F0F4F8`
- Semantic: success `#4A9070`, error `#A85050`, warning `#B89040`
- Temperature: cool
- Family: Futuristic, Minimal

### Emotional Narratives

**Quiet Confidence**
- Narrative: Deep navy suit, white shirt, single gold watch. Nothing louder than necessary.
- Accent: `#C4A050` (single gold)
- Neutral scale: `#0A0F1A` / `#141C30` / `#253050` / `#4A5878` / `#8A94B0` / `#C8CEE0`
- Surface: `#F2F4FA`
- Semantic: success `#4A8A60`, error `#A85050`, warning `#C4943A`
- Temperature: cool
- Family: Sophisticated, Authoritative

**Controlled Chaos**
- Narrative: Near-black base with three saturated accents at varying visual weights. Intentionally unbalanced.
- Primary accent: `#E84060` (hot pink-red)
- Secondary accent: `#40C8A0` (mint)
- Tertiary accent: `#FFB030` (amber)
- Neutral scale: `#08080C` / `#101018` / `#1E1E2A` / `#353544` / `#606072` / `#A0A0B0`
- Surface: `#0C0C14`
- Semantic: success `#40C8A0`, error `#E84060`, warning `#FFB030`
- Temperature: neutral
- Family: Disruptive, Electric

**Gentle Strength**
- Narrative: Soft surfaces that resist pressure. Warm but immovable.
- Accent: `#5A7A90` (muted steel blue)
- Neutral scale: `#10141A` / `#1C222C` / `#343C4A` / `#586270` / `#90A0AE` / `#CED8E0`
- Surface: `#F0F4F8`
- Semantic: success `#5A9070`, error `#A85858`, warning `#B89440`
- Temperature: cool-warm
- Family: Trustworthy, Grounded

**Radical Optimism**
- Narrative: Bright, warm, assertive. Color as energy.
- Accent: `#FF6B35` (warm orange)
- Neutral scale: `#1A1210` / `#2E2220` / `#504040` / `#7A6A68` / `#B8AAA8` / `#E5DCDA`
- Surface: `#FFF8F4`
- Semantic: success `#3AAA60`, error `#E84050`, warning `#F0C030`
- Temperature: warm
- Family: Energetic, Friendly

**Deep Focus**
- Narrative: Dark mode native. Low contrast neutrals, single cool accent, no distractions.
- Accent: `#5090D0` (focus blue)
- Neutral scale: `#0A0C10` / `#12161C` / `#1E242C` / `#303840` / `#505860` / `#808890`
- Surface: `#0E1014`
- Semantic: success `#40A870`, error `#D05050`, warning `#D0A040`
- Temperature: cool
- Family: Technical, Futuristic

**Warm Authority**
- Narrative: Established, human, grounded. Not cold corporate, but equally serious.
- Accent: `#8A5A38` (warm umber)
- Neutral scale: `#181410` / `#2A2420` / `#443830` / `#6A5C50` / `#A09080` / `#D4C8B8`
- Surface: `#F6F0E8`
- Semantic: success `#5A8A40`, error `#A84545`, warning `#C49830`
- Temperature: warm
- Family: Authoritative, Classic

---

## Synthesis Algorithm

Step-by-step process for generating a novel aesthetic from user inputs.

### Step 1: Extract Mood Keywords

From the discovery interview or product description, identify mood keywords. Map synonyms to the 30 canonical moods:

- "reliable", "stable", "safe" -> Trustworthy
- "fun", "joyful", "lighthearted" -> Playful
- "premium", "elegant", "high-end" -> Luxurious
- "cutting-edge", "forward", "next-gen" -> Futuristic
- "craft", "handmade", "authentic" -> Artisan
- "clean", "precise", "sterile" -> Clinical
- "powerful", "strong", "unapologetic" -> Bold
- "calm", "peaceful", "tranquil" -> Serene
- "smart", "clever", "witty" -> Whimsical
- "serious", "formal", "dignified" -> Authoritative

If multiple moods are present, select a primary and secondary. The primary controls 60% of design decisions.

### Step 2: Look Up Mood Properties

Retrieve the 7 properties (temperature, saturation, weight, roundness, density, energy, script context) from the Mood Matrix for both primary and secondary moods. Blend with 60/40 weighting.

### Step 3: Match Color Narrative

If the user provided a visual or narrative description ("our brand feels like a desert sunrise"), find the closest match in the Color Narratives. If no narrative was given, select based on the mood's temperature and saturation properties:

| Temperature | Saturation | Narrative Pool |
|---|---|---|
| warm | muted | Desert Sunrise, Aged Leather, Oiled Wood, Warm Authority |
| warm | vivid | Volcanic Earth, Night Market, Radical Optimism |
| cool | muted | Northern Ocean at Dusk, Alpine Winter, Deep Focus |
| cool | vivid | Tokyo Neon After Rain, Arctic Lichen, Frosted Glass |
| neutral | moderate | London Fog, Brushed Aluminum, Gentle Strength |

### Synthesis Routing Table

Use this table in Phase 2 to resolve abstract inputs into concrete packet fields.
Agents should receive the resolved result, not the full preset tables.

| Aesthetic Origin | Industry / Surface | Primary Mood | Color Narrative | Neutral Family | Font Direction |
|---|---|---|---|---|---|
| Warm Editorial | Services, publishing, consulting | refined/classic | London Fog or Golden Prairie | Brass or Sandstone | editorial serif + humanist sans |
| Neon Dashboard | Devtools, analytics, fintech dashboards | technical/electric | Tokyo Neon After Rain | Ink | technical grotesque + readable sans |
| Approachable Enterprise | Productivity, health, consumer SaaS | friendly/trustworthy | Scandinavian Apartment | Moss or Smoke | rounded sans + humanist sans |
| Brutalist Raw | Creative, portfolio, experimental | raw/disruptive | Brutalist Downtown | Concrete | heavy grotesque or mono + plain sans |
| Luxury Premium | Hospitality, fashion, premium B2B | luxurious/understated | London Fog or Obsidian Gallery | Ink or Brass | high-contrast serif + restrained sans |
| Earth Organic | Wellness, food, sustainability | organic/grounded | Forest Canopy or Desert Sunrise | Moss or Sandstone | organic serif + warm sans |
| Swiss Precision | Enterprise, documentation, product | authoritative/minimal | Brushed Aluminum | Smoke or Concrete | neo-grotesque family with width/weight tension |
| Candy Pop | Entertainment, education, youth | playful/energetic | Coral Reef or Night Market | custom high-chroma | expressive display + plain body |

If a row feels too obvious for the product, keep the neutral family but change
the color narrative or font direction. Document that deviation in
`aesthetic.modifications`.

### Step 4: Generate the Color Palette

From the selected narrative:
1. Extract the accent color
2. Build the neutral scale (6 stops from darkest to lightest)
3. Assign surface color (lightest or darkest based on mode preference)
4. Assign semantic colors (success, error, warning)
5. If the user has brand colors, replace the accent and adjust the neutral scale temperature to harmonize

### Step 5: Select Font Pairing

From the mood family, select a pairing from the Font Pairing Engine:
1. Match primary mood to the closest mood family
2. Select a pairing from that family
3. If the user has a font preference, find the closest match in the validated pairings
4. Verify the pairing satisfies all three rules (contrast, harmony, context)

### Step 6: Derive Structural Tokens

From the blended mood properties:
- **Density** -> spacing scale multiplier (sparse: 1.5x, comfortable: 1x, dense: 0.75x)
- **Roundness** -> border-radius base (sharp: 2px, moderate: 8px, round: 16px)
- **Weight** -> default font-weight and stroke width
- **Energy** -> transition duration and easing (calm: 200ms ease, moderate: 150ms ease-out, dynamic: 100ms spring)

### Step 6.5: Derive Spatial Logic

From the aesthetic's spatial philosophy and component character:

1. **Grid discipline** — Is the grid strict (Swiss Precision, Tech Blueprint), structured (Friendly Enterprise, Retro Terminal), loose (Warm Editorial, Wabi-Sabi Serene), or broken (Candy Pop, Brutalist)? Encode as `spatial_logic.grid_discipline`: "strict" | "structured" | "loose" | "broken"

2. **Section rhythm** — How sections alternate: uniform spacing, alternating emphasis (hero + quiet + hero), or flowing overlap. Encode as `spatial_logic.section_rhythm`: "uniform" | "alternating" | "overlapping" | "dramatic"

3. **Whitespace attitude** — Is whitespace generous (Warm Editorial, Luxury Premium), functional (Neon Dashboard, Tech Blueprint), tight (Retro Terminal, Swiss Precision), or confrontational (Brutalist)? Encode as `spatial_logic.whitespace_attitude`: "generous" | "functional" | "tight" | "confrontational"

4. **Alignment preference** — Centered, left-aligned, asymmetric split, or color-block defined. Encode as `spatial_logic.alignment`: "centered" | "left" | "asymmetric" | "color-block"

5. **Component character rules** — Derive 3-5 specific CSS-level rules for how buttons, cards, inputs, navigation, and sections express the aesthetic. Encode as `spatial_logic.component_character` with `border_treatment`, `hover_behavior`, `elevation`, and `padding_asymmetry` per component category.

**Example spatial_logic object for Warm Editorial:**

```json
{
  "spatial_logic": {
    "grid_discipline": "loose",
    "section_rhythm": "alternating",
    "whitespace_attitude": "generous",
    "alignment": "asymmetric",
    "component_character": {
      "buttons": {
        "border_treatment": "none",
        "hover_behavior": "subtle-darken",
        "elevation": "none",
        "padding": "generous-horizontal"
      },
      "cards": {
        "border_treatment": "thin-border",
        "elevation": "none",
        "padding_asymmetry": "generous-vertical",
        "shadow": "none"
      },
      "inputs": {
        "border_treatment": "bottom-only",
        "focus_behavior": "underline-expand"
      },
      "navigation": {
        "treatment": "horizontal-minimal",
        "letter_spacing": "generous",
        "active_state": "underline"
      }
    },
    "example_layout_hints": [
      "Hero headlines use 2x vertical margin of body text",
      "Cards are grouped in asymmetrical clusters, not rigid columns",
      "Sections separated by generous whitespace, never dividers",
      "Key CTAs use accent color, all others are understated outlines"
    ]
  }
}
```

**Critical:** This spatial logic is included in the context packet and MUST reach all generation agents. Without it, different presets produce structurally identical layouts. Agents generating UI components MUST reference `spatial_logic` when deciding:
- Component spacing and padding
- Border vs elevation treatment
- Layout grid adherence
- Section transitions
- Alignment and asymmetry

### Step 7: Uniqueness Check

Compare the synthesized aesthetic against the 14 presets in aesthetic_directions.md.

**Uniqueness criteria:** If the output shares accent hue (within 30 degrees on HSL wheel), same font pairing, AND same radius base as any preset, it is too similar. **Require at least 2 of 3 to differ** from any preset:

1. **Accent hue** — Must differ by at least 30 degrees HSL from all presets, OR be explicitly user-provided
2. **Font pairing** — Must use different display or body font than the preset match
3. **Radius base** — Must use different roundness category (sharp vs moderate vs round vs full)

When too similar to a preset, change the concept before changing decoration:

- Write a philosophy statement: "This system believes X and refuses to Y"
- Add one component character rule that affects ordinary UI
- Swap the font pairing to a more distinctive tier
- Shift accent hue by 15-30 degrees only if it supports the philosophy
- Adjust roundness by one level only if it changes component character

### Step 8: Generate Complete Token Set

Output the full CSS custom property set covering colors, typography, spacing, borders, shadows, and motion. Follow the token structure defined in the skill's output format.

---

## Preset Escape Velocity

Presets are starting points, not end states. A preset used verbatim — even with
modified colors or a different font — is recognizable as "the preset." Preset
Escape Velocity is the minimum design distance required before the output reads
as a genuinely new system rather than a dressed-up template.

### The Problem: Low-Distance Modifications

These modifications have ZERO escape velocity:
- Changing the accent color while keeping all other token values and font pairs
- Swapping one font while keeping all layout, spacing, and border conventions
- Adding a dark mode to a light preset without changing the structural character
- Renaming the "Approachable Enterprise" preset to "B2B Friendly"

These modifications create cosmetic diversity — they look different in a screenshot
but generate the same user experience and structural patterns. An experienced designer
can recognize the underlying preset immediately.

### The Escape Velocity Test

After choosing a preset and applying modifications, ask:

1. **Philosophy test.** Can you write a sentence starting "This system believes..."
   that would NOT be true of the base preset? If not, you haven't escaped.

2. **Negative constraint test.** Can you name one thing this system explicitly
   refuses that the preset doesn't refuse? If not, you haven't escaped.

3. **Component character test.** Does at least one component category (buttons,
   cards, inputs, navigation, or sections) have a character rule that would
   surprise someone familiar with the base preset? If not, you haven't escaped.

4. **Spatial test.** Does the spatial logic (whitespace attitude, alignment
   preference, or section rhythm) differ from the preset default in a verifiable
   way? If not, you haven't escaped.

All four tests must pass. If fewer than 4 pass, increase the divergence step.

### Divergence Direction Table

For each base preset, here are the highest-velocity escape directions:

| Base Preset | Default Pattern | Escape Direction A | Escape Direction B |
|-------------|----------------|-------------------|-------------------|
| Neon Dashboard | Dark + electric blue/cyan + geometric sans | Dark + amber/copper + condensed grotesque + structural asymmetry | Dark + monochrome + ultra-dense data + no color except semantic |
| Approachable Enterprise | Pastel + rounded + humanist sans | Warm neutral + moderate radius + organic serif hero + editorial section breaks | Cool technical + sharp radius + tabular data surfaces + mono labels |
| Warm Editorial | Sandstone + serif + generous whitespace | Brass + condensed serif + deliberate density contrast (not just generous) | Ink + editorial serif + architectural layout breaks |
| Swiss Precision | Neutral + geometric sans + strict grid | Smoke + variable-width grotesque + staggered text columns | Brass + institutional serif for headings + grid with intentional breaks in one zone |
| Luxury Premium | Near-black + metallic detail + restrained sans | Chalk + single high-contrast serif + ultra-sparse section rhythm | Near-white + editorial serif + restraint so total that absence is the signature |
| Brutalist Raw | Concrete + heavy grotesque + exposed structure | Ink + condensed grotesque + structural brutalism WITH warm accent | Concrete + a single decorative serif in one place, treating it as dissonance |

When selecting an escape direction, pick the one that better fits the product domain.
If both feel wrong, create a hybrid by taking one element from each direction.

### Escape Velocity Minimum Requirements

Before dispatching generation agents, the context packet `aesthetic.modifications`
field must document at least **two of the following** for the chosen preset:

- `font_divergence` — at least one font differs from the preset default AND the
  new font expresses a different brand value
- `spatial_divergence` — `spatial_logic.alignment`, `whitespace_attitude`, or
  `section_rhythm` differs from the preset default
- `component_divergence` — at least one component category has a character rule
  that contradicts the preset's default treatment
- `philosophy_divergence` — a philosophy statement and one "refuses to" constraint
  are documented in `creative_brief`
- `accent_divergence` — accent color hue differs by at least 30 degrees HSL from
  the preset canonical accent AND the reason is documented in the decision log

Two divergences create enough escape velocity for a Professional output. Three or
more open the path to an Exceptional (Tier 3) output.

---

## Anti-Slop Rules

When synthesizing novel aesthetics, apply these constraints by default. They may
be intentionally relaxed only when the chosen aesthetic requires it and the
decision log documents the reason.

1. **Never default to Inter, Roboto, or system-ui as the primary display font.** These are acceptable as body fonts when paired with a distinctive display font, but never as the heading/brand font.

2. **Never use purple gradients as the default accent treatment.** Purple gradients are the most overused visual pattern in generic design. If the user explicitly requests purple, use a single deep purple tone, not a gradient.

3. **Never create a "generic SaaS" look.** This means no: centered hero text, soft blue (#4A90D9) accent, Inter font, rounded-8px-everything, and pastel backgrounds. This is the single most common default output and must be actively avoided.

4. **Every synthesized aesthetic must have a distinctive characteristic.** At least one element must be unusual: an unexpected accent color, a bold font choice, an unconventional border-radius, an asymmetric layout density, or a strong weight contrast.

5. **If the result is too close to an existing preset, add one differentiating element.** Check against the 14 presets before finalizing. Differentiate via hue shift, font swap, or structural token change.

6. **Use saturation bounds by aesthetic, not universally.** Brutalist, Swiss,
   and Wabi-Sabi systems may intentionally use very low chroma. Candy Pop,
   Afrofuture, Memphis-inspired, and entertainment systems may intentionally
   exceed high-saturation defaults. If breaking the normal range, document the
   reason in `decision_log` and preserve contrast/accessibility.

7. **Never pair two fonts from the same category without weight contrast.** Two geometric sans-serifs paired together need significant weight difference (at least 300 units apart) or they serve no purpose as a pairing.

---

## Hybrid Synthesis Examples

When inputs come from multiple sources (user description, brand assets, product type), use hybrid synthesis to combine them coherently.

### Example 1: Artisan Coffee Roaster

**Inputs:**
- User description: "Warm, handcrafted, honest but modern"
- Brand assets: Warm brown logo with cream text
- Product type: E-commerce with editorial content

**Synthesis:**
- Primary mood: Artisan (warm, moderate, medium, round, comfortable, moderate)
- Secondary mood: Honest (warm, moderate, medium, moderate, comfortable, moderate)
- Color source: Brand asset match -> Oiled Wood narrative, accent adjusted to match logo brown
- Font pairing: Roslindale + DM Sans (Organic family)
- Density: comfortable -> 1x spacing multiplier
- Roundness: round -> 16px base radius

**Token Output:**
```css
:root {
  /* Colors - Oiled Wood adjusted to brand */
  --color-accent: #8A6230;
  --color-accent-hover: #9A7240;
  --color-accent-muted: rgba(138, 98, 48, 0.15);

  --color-neutral-900: #1C1610;
  --color-neutral-800: #302618;
  --color-neutral-700: #504028;
  --color-neutral-600: #7A6840;
  --color-neutral-500: #B0A070;
  --color-neutral-400: #C4B888;
  --color-neutral-300: #D8CCA8;
  --color-neutral-200: #E8DECA;
  --color-neutral-100: #F0E8D8;
  --color-neutral-50: #FAF4E6;
  --color-surface: #FAF4E6;
  --color-surface-elevated: #FFFFFF;

  --color-success: #5A8A40;
  --color-error: #A84545;
  --color-warning: #C49830;

  /* Typography */
  --font-display: 'Roslindale', Georgia, serif;
  --font-body: 'DM Sans', sans-serif;
  --font-weight-display: 700;
  --font-weight-body: 400;
  --font-weight-label: 500;
  --line-height-body: 1.6;
  --line-height-heading: 1.15;

  /* Structure */
  --radius-sm: 8px;
  --radius-md: 16px;
  --radius-lg: 24px;
  --radius-full: 9999px;
  --spacing-unit: 8px;
  --spacing-scale: 1;

  /* Motion */
  --duration-fast: 150ms;
  --duration-normal: 200ms;
  --easing-default: ease;
}
```

### Example 2: Fintech Dashboard

**Inputs:**
- User description: "Trustworthy, precise, fast"
- Brand assets: Deep blue logo, white wordmark
- Product type: Data-heavy dashboard application

**Synthesis:**
- Primary mood: Trustworthy (cool, moderate, medium, moderate, comfortable, calm)
- Secondary mood: Technical (cool, muted, medium, sharp, dense, moderate)
- Color source: Brand asset match -> Northern Ocean at Dusk, accent adjusted to match brand blue
- Font pairing: Space Grotesk + IBM Plex Sans (Technical family)
- Density: dense -> 0.75x spacing multiplier (data-heavy product)
- Roundness: moderate -> 8px base radius

**Token Output:**
```css
:root {
  /* Colors - Northern Ocean adjusted to brand */
  --color-accent: #3A6EA5;
  --color-accent-hover: #4A7EB5;
  --color-accent-muted: rgba(58, 106, 165, 0.12);

  --color-neutral-900: #0F1B2D;
  --color-neutral-800: #1E2D45;
  --color-neutral-700: #3A4F6B;
  --color-neutral-600: #5A7090;
  --color-neutral-500: #8B9DB5;
  --color-neutral-400: #A8B8CA;
  --color-neutral-300: #C4D0DE;
  --color-neutral-200: #D8E0EA;
  --color-neutral-100: #E8EDF3;
  --color-neutral-50: #F5F7FA;
  --color-surface: #F5F7FA;
  --color-surface-elevated: #FFFFFF;

  --color-success: #3B8A6E;
  --color-error: #C75D5D;
  --color-warning: #D4A24E;

  /* Typography */
  --font-display: 'Space Grotesk', sans-serif;
  --font-body: 'IBM Plex Sans', sans-serif;
  --font-weight-display: 600;
  --font-weight-body: 400;
  --font-weight-label: 500;
  --font-weight-mono: 500;
  --line-height-body: 1.55;
  --line-height-heading: 1.2;

  /* Structure */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;
  --radius-full: 9999px;
  --spacing-unit: 6px;
  --spacing-scale: 0.75;

  /* Motion */
  --duration-fast: 100ms;
  --duration-normal: 150ms;
  --easing-default: ease-out;
}
```

### Example 3: Creative Agency Portfolio

**Inputs:**
- User description: "Bold, electric, disruptive"
- Brand assets: Hot pink accent on black, geometric logo
- Product type: Showcase portfolio with case studies

**Synthesis:**
- Primary mood: Disruptive (warm, vivid, heavy, sharp, dense, dynamic)
- Secondary mood: Electric (cool, vivid, medium, sharp, dense, dynamic)
- Color source: Brand asset match -> Controlled Chaos narrative, accent adjusted to match brand pink
- Font pairing: Syne + Work Sans (Bold family)
- Density: dense -> 0.75x spacing multiplier
- Roundness: sharp -> 2px base radius

**Token Output:**
```css
:root {
  /* Colors - Controlled Chaos adjusted to brand */
  --color-accent: #E84068;
  --color-accent-hover: #F05078;
  --color-accent-muted: rgba(232, 64, 104, 0.15);
  --color-accent-secondary: #40C8A0;
  --color-accent-tertiary: #FFB030;

  --color-neutral-900: #08080C;
  --color-neutral-800: #101018;
  --color-neutral-700: #1E1E2A;
  --color-neutral-600: #353544;
  --color-neutral-500: #505060;
  --color-neutral-400: #707080;
  --color-neutral-300: #909098;
  --color-neutral-200: #B0B0B8;
  --color-neutral-100: #D0D0D5;
  --color-neutral-50: #EEEFF0;
  --color-surface: #0C0C14;
  --color-surface-elevated: #181820;

  --color-success: #40C8A0;
  --color-error: #E84060;
  --color-warning: #FFB030;

  /* Typography */
  --font-display: 'Syne', sans-serif;
  --font-body: 'Work Sans', sans-serif;
  --font-weight-display: 800;
  --font-weight-body: 400;
  --font-weight-label: 600;
  --line-height-body: 1.6;
  --line-height-heading: 1.05;

  /* Structure */
  --radius-sm: 0px;
  --radius-md: 2px;
  --radius-lg: 4px;
  --radius-full: 9999px;
  --spacing-unit: 6px;
  --spacing-scale: 0.75;

  /* Motion */
  --duration-fast: 80ms;
  --duration-normal: 120ms;
  --easing-default: cubic-bezier(0.2, 0.8, 0.2, 1);
}
```
