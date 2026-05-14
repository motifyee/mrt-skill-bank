# Context Budget Calculator

Token-cost accounting for each phase and agent. Use this to select the right runtime mode.

## Token Accounting

Rough estimates: 1 line of prose ≈ 15 tokens, 1 line of YAML ≈ 8 tokens, 1 line of CSS ≈ 12 tokens.

### Main Chat Phases

| Phase | What's Loaded | Est. Tokens |
|-------|--------------|-------------|
| System prompt overhead | Role, rules, tool definitions | 2,000–4,000 |
| Phase 1: Interview | interview_framework.md + conversation history (5-15 messages) | 8,000–20,000 |
| Phase 2: Synthesis | generation_flow.md + aesthetic_directions.md + compact divergence notes + packet schema + context gates | 14,000–20,000 |
| Phase 3: Dispatch prep | agent_task_templates.md + compact packet (~140-200 lines YAML) | 7,000–11,000 |
| Phase 5: Assembly | Agent results + cross-reference verification | 5,000–15,000 |
| **Main chat total** | | **35,000–65,000** |

### Per-Agent Context Costs

Each agent receives: prompt template (~800 tokens) + context packet (~1,200–3,000 tokens depending on compact mode) + file reads.

| Agent | Files Read | File Read Tokens | Total Est. |
|-------|-----------|-----------------|------------|
| A (CSS Tokens) | token_schema.md, theming_schema.md | 4,000 | 5,500–7,000 |
| B (Dashboard) | token_schema.md, output_schemas.md | 4,500 | 6,000–8,000 |
| C (Previews) | token_schema.md, output_schemas.md | 4,500 | 6,000–8,000 |
| D (Marketing) | token_schema.md, output_schemas.md, 2 component files | 6,000 | 8,000–10,000 |
| E (Dashboard) | token_schema.md, output_schemas.md, 2 component files | 6,000 | 8,000–10,000 |
| F (Docs) | token_schema.md, output_schemas.md, 2 component files | 6,000 | 8,000–10,000 |
| G (Documentation) | output_schemas.md | 3,500 | 5,000–6,500 |
| H (Evaluation) | evaluation_checklist.md, ai_slop_detection.md, ALL generated outputs | 15,000–50,000 | 18,000–55,000 |

### Agent H is the bottleneck

Agent H must read all generated outputs. Estimated output sizes:

| Output | Est. Size |
|--------|----------|
| tokens.css | 3,000–5,000 tokens |
| design-system.html | 8,000–15,000 tokens |
| preview/*.html (10-15 files) | 15,000–30,000 tokens |
| ui_kits/marketing/ | 8,000–15,000 tokens |
| ui_kits/dashboard/ | 8,000–15,000 tokens |
| README.md + DECISIONS.md + SKILL.md | 4,000–8,000 tokens |
| **Total outputs** | **45,000–85,000 tokens** |

Agent H also reads evaluation_checklist.md (~3,500 tokens) and ai_slop_detection.md (~3,000 tokens).
**Agent H total: 52,000–94,000 tokens.** This will overflow most agent contexts.

**Mitigation:** Split Agent H into waves (see agent_decomposition.md) or evaluate files individually.

## Mode Selection

```
If ALL agent contexts support 100K+ tokens:
  → Mode 1 (Full Parallel): dispatch A-G simultaneously

If agent contexts are 50K-100K tokens:
  → Mode 2 (Batched): Wave 1 (A,B,G) → Wave 2 (C,D,E,F) → Wave 3 (H1,H2,H3)

If agent contexts are under 50K tokens:
  → Mode 3 (Sequential): one agent, sequential deliverables
```

## Warning Signs

- If any agent output is truncated, the context was too small — use Mode 2 or 3
- If Agent H fails to read all output files, use wave-based evaluation
- If the packet has > 10 empty required fields, return to Phase 1 interview
- If main chat exceeds 65K tokens, switch from Mode 1 to Mode 2

## Hard Gates (Block Dispatch)

Before dispatching any generation agents:

1. Packet line count > 250: downgrade to Mode 2 or Mode 3.
2. Packet line count > 320 after prose trimming: force Mode 3.
3. Main chat estimated total > 65K: stop loading docs and downgrade one mode.
4. Predicted evaluation payload > single-agent window: force H1/H2 parallel + H3 cross-reference wave.
5. If any gate fails and cannot be mitigated, pause generation and gather only missing required inputs.

## Packet Slicing Policy

Packet slicing is mandatory by default for A-F agents:

- A-F: use per-agent slices from `schemas/agent_context_packet.md`.
- A-F: prefer compact operational packet slices whenever packet line count exceeds 180.
- G: full packet (documentation + decision traceability).
- H1/H2/H3: minimal packet (`brand`, `constraints`, and only required references).

Use full-packet fanout only when packet size is under 180 lines and the runtime has verified surplus context.

## Creativity Budget Guidance

Do not let creative exploration consume the same budget as generation.

- Divergence step: keep to 2-3 micro-directions, each under 120 words.
- Research enrichment: keep only findings that change tokens, layout, motion, or signature moment.
- Inspiration references: summarize techniques, never paste long source descriptions into the main chat.
- If context is tight, preserve `creative_brief`, `signature_moment`,
  `components.character_rules`, `tension_points`, and research deltas before
  preserving verbose rationale prose.

## Targeted Preset Loading

`references/aesthetic_directions.md` contains 14 full preset definitions (~28,000 tokens total).
Loading the entire file during synthesis is the single largest avoidable context cost in
Phase 2. Most projects use exactly one preset as a starting point.

### Policy: Load Only the Matched Preset

After the aesthetic origin is chosen (Phase B of the interview), load only the matching
preset section using targeted reading. Do NOT load the full aesthetic_directions.md file
into the main chat context.

**How to implement:**

1. After Phase B locks in the aesthetic origin (e.g., "Neon Dashboard"), use a background
   agent or targeted grep to extract only that preset's section:
   ```
   # Extract only the chosen preset section
   grep -A 80 "^## Neon Dashboard" references/aesthetic_directions.md
   ```
2. Pass the extracted section (~2,000 tokens) to Phase 2 synthesis rather than the full file.
3. Record the chosen preset name in the context packet (`aesthetic.origin`) so it can be
   re-extracted if needed downstream — do not keep the prose in the main chat.

**Expected savings:** Phase 2 synthesis drops from ~20,000 to ~8,000 tokens. For Mode 2
(grouped tasks), this frees enough context for a richer context packet or more research evidence.

**When to load the full file:**
- The user has not chosen a preset and is selecting from all 14 options in Phase B
- A custom aesthetic uses multiple presets as reference points
- The divergence step explicitly requires comparing the chosen preset against alternatives

**Fallback:** If targeted extraction is not available in the runtime (e.g., Mode 3 sequential
single-agent), load the file once, extract the preset section mentally, then clear the reference.
Do not maintain the full file in active working memory after the matched section is identified.
