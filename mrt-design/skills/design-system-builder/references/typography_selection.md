# Language-Aware Typography Selection

Typography is not only a font pairing decision. It is how the brand's values,
language, and page role become visible in text. Every professional system should
choose type by script, emotional intent, surface role, and performance constraints.

---

## Selection Inputs

Before selecting fonts, define these inputs in the context packet:

| Input | Why it matters |
|---|---|
| `primary_language` | Determines script, reading rhythm, available font quality, and fallback stack |
| `secondary_languages` | Prevents Latin-first systems from breaking in bilingual products |
| `script_system` | Latin, Arabic, CJK, Devanagari, Bengali, Tamil, Cyrillic, Hebrew, Thai, etc. |
| `brand_values` | Luxury, warmth, play, precision, authority, beauty, speed, craft, restraint |
| `surface_roles` | Hero, marketing body, dashboard UI, docs, settings, data, code, captions |
| `voice_profile` | Determines whether type should feel formal, friendly, technical, editorial, comic |
| `risk_dial` | Safe, elevated, bold, experimental; controls how expressive display type can be |

Never select typography from product category alone. A fintech product can be
playful; a healthcare brand can be editorial; an Arabic interface can be modern,
luxurious, scholarly, comic, or utilitarian depending on context.

---

## Script-Aware Beauty Principles

Each writing system has different visual traditions. Respect the script's own
forms instead of forcing a Latin aesthetic onto it.

| Script | Beauty cues | Avoid |
|---|---|---|
| Latin | Contrast between serif/sans, width, weight, optical size, editorial rhythm | Default geometric sans everywhere |
| Arabic | Calligraphic flow, generous line height, Naskh readability, Kufi geometry for display | Tight line height, forced Latin spacing, decorative calligraphy in body text |
| Persian/Urdu | Nastaliq or calligraphic display where culturally appropriate, readable Naskh body | Overly mechanical Arabic fallbacks |
| CJK | Balance, texture, ideographic clarity, Mincho/Song authority, Gothic utility | Tiny UI text, Latin-first hierarchy, insufficient font fallback coverage |
| Devanagari | Clear shirorekha rhythm, open counters, generous vertical metrics | Condensed Latin display fonts as primary identity |
| Bengali/Tamil | Rounded forms, script-specific proportions, comfortable body line height | Generic Latin-derived sans choices without native-script quality |
| Cyrillic | Proper Cyrillic design, not Latin fonts with weak glyph extensions | Fonts with incomplete or awkward Cyrillic forms |
| Hebrew | Strong text color, readable body forms, cautious display styling | Latin-like tracking and over-tight headings |
| Thai | Tall vertical rhythm, careful line height, readable loop/no-loop choice | Small labels without tested legibility |

If the product is multilingual, the primary script drives display identity and
the secondary script must be tuned to harmonize. Do not let English headings
define the entire system when the product's main language is not English.

---

## Emotional Font Roles

Map brand values to typographic traits before choosing names.

| Value / feel | Useful typographic traits | Typical use |
|---|---|---|
| Luxury | High contrast, refined serif/calligraphic display, restrained sans body | Hero, product pages, hospitality, premium services |
| Beauty | Elegant proportions, graceful curves, generous whitespace, careful optical sizes | Editorial moments, brand statements, portfolios |
| Friendly | Humanist or rounded forms, open counters, moderate weight, warm rhythm | Consumer apps, onboarding, support flows |
| Comic / playful | Expressive display, bounce in shapes, high x-height body, controlled irregularity | Youth brands, entertainment, education |
| Technical | Monospace accents, engineered grotesques, clear numerals, compact scale | Devtools, dashboards, docs, data products |
| Authority | Strong serif or institutional sans, stable rhythm, low novelty | Finance, healthcare, legal, enterprise |
| Craft | Organic serif, soft contrast, slightly imperfect details, warm body face | Food, wellness, handmade, cultural brands |
| Speed | Condensed or forward-leaning display, tight headings, crisp UI sans | Logistics, sports, developer operations |
| Calm | Low contrast, open forms, relaxed line height, minimal weight jumps | Wellness, healthcare, mindfulness, docs |

The selected font must be justified by the brand value it expresses. If no value
is expressed, the font is decoration and should be replaced.

---

## Page-Part Typography Matrix

Different page parts can carry different levels of expression. A beautiful system
does not use the same typographic voice everywhere.

| Page part | Typography role | Expression limit |
|---|---|---|
| Brand mark / wordmark | Highest identity signal | Can be custom, expressive, or script-specific |
| Hero display | Emotional thesis and memorability | Highest expressive type; must remain readable |
| Section headings | Rhythm and hierarchy | Moderate expression; reinforces display system |
| Body copy | Trust and comprehension | Readability first; no decorative fonts |
| Navigation | Orientation and confidence | Compact, highly legible, stable metrics |
| Buttons / labels | Action clarity | Medium/semibold, strong legibility, no novelty that slows action |
| Data tables | Comparison and scanning | Tabular numerals, compact leading, low decoration |
| Documentation | Long reading comfort | Text face optimized for paragraphs and code adjacency |
| Captions / helper text | Support and reassurance | High legibility at small sizes; no fragile thin weights |
| Code | Literal precision | Monospace with clear punctuation and numerals |

For multilingual systems, repeat this matrix per script if roles need different
fonts. Example: Arabic hero uses Kufi display, Arabic body uses Naskh, English
technical labels use a geometric sans that harmonizes by weight and width.

---

## Professional Selection Process

1. **Name the language and script.** Identify primary and secondary languages,
   writing direction, and script-specific legibility constraints.
2. **Name the brand essence.** Choose 2-3 values the typography must embody.
3. **Assign page roles.** Decide which page parts may be expressive and which
   must be quiet.
4. **Choose script-native candidates.** Select fonts designed well for the
   primary script before considering Latin pairings.
5. **Test emotional fit.** Check whether the font forms express the values:
   luxury, comic, friendly, beauty, authority, precision, craft, calm, etc.
6. **Build the type scale.** Tune ratio, weights, line heights, and tracking for
   the script. Do not reuse Latin tracking rules blindly.
7. **Check multilingual harmony.** Compare x-height or visual size, stroke
   weight, contrast, and text color between scripts.
8. **Document rationale.** Record why the font fits the brand, where it is used,
   what it must not be used for, and what fallback preserves the spirit.

---

## Script-Specific Scale Adjustments

| Script | Scale and metric guidance |
|---|---|
| Latin | Body 16px, headings can use tight line-height; negative tracking only for large display |
| Arabic | Body usually needs 17-18px equivalent, line-height 1.7-1.9, avoid negative tracking |
| CJK | Body often 15-16px, line-height 1.6-1.8, avoid overly dramatic size jumps |
| Devanagari | Body 16-18px, line-height 1.6-1.8, ensure matra marks have vertical room |
| Thai | Body 16-18px, line-height 1.7+, avoid tiny labels |
| Hebrew | Body 16px+, line-height 1.5-1.7, avoid Latin-style letter spacing |
| Data-heavy UI | Prefer smaller ratio (1.125-1.2) but preserve script legibility |
| Brand-forward hero | Ratio can be 1.333+ if the script remains readable and culturally appropriate |

---

## Packet Output Requirements

The context packet should include:

```yaml
typography:
  language_strategy:
    primary_language: ""
    secondary_languages: []
    script_system: ""
    reading_direction: ""
    cultural_typographic_notes: []
  expressive_roles:
    brand_mark: ""
    hero: ""
    section_heading: ""
    body: ""
    ui_label: ""
    data: ""
    code: ""
  font_rationale:
    display:
      font: ""
      expresses: []
      script_fit: ""
      use_for: []
      avoid_for: []
    body:
      font: ""
      expresses: []
      script_fit: ""
      use_for: []
      avoid_for: []
    mono:
      font: ""
      expresses: []
      script_fit: ""
      use_for: []
      avoid_for: []
  multilingual_fallbacks:
    - language: ""
      stack: ""
      reason: ""
```

These fields are not decorative. Agents must use them to decide which font token
applies to each page part and to document why the typography fits the brand.
