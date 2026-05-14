# Prompt Classification

Classify before doing anything else. Classification determines enhancement strategy, reasoning depth, output structure, and context engineering approach.

---

## Prompt Categories

| Category                 | Signals                                                                  | Primary Enhancement Focus                                                  |
| ------------------------ | ------------------------------------------------------------------------ | -------------------------------------------------------------------------- |
| **Coding**               | functions, APIs, debugging, implementation, scripts                      | Architecture, error handling, edge cases, testability, integration         |
| **Architecture**         | system design, scaling, infrastructure, distributed systems              | Tradeoffs, constraints, failure modes, scalability, operational concerns   |
| **Research**             | analyze, compare, investigate, evaluate, survey                          | Evidence rigor, source quality, analytical depth, bias detection           |
| **Design**               | UI, UX, layout, visual, interaction, wireframe                           | Hierarchy, accessibility, responsiveness, interaction states               |
| **Business**             | strategy, market, revenue, positioning, GTM                              | Stakeholder impact, ROI, competitive analysis, risk                        |
| **Writing**              | content, copy, article, narrative, blog, email                           | Tone, audience, structure, clarity, engagement                             |
| **Agent / Automation**   | workflow, agent, pipeline, orchestration, multi-step                     | Tool usage, state management, failure recovery, verification               |
| **Analysis**             | data, metrics, insights, patterns, dashboards                            | Completeness, methodology, bias awareness, visualization                   |
| **Planning**             | roadmap, milestones, priorities, sprint, backlog                         | Dependencies, risks, sequencing, success criteria                          |
| **Creative**             | brainstorm, ideate, generate ideas, explore, invent                      | Divergent thinking, novelty, constraint relaxation                         |
| **Extraction / Parsing** | extract, parse, structure, convert, transform, identify                  | Schema clarity, coverage completeness, null/miss handling, format fidelity |
| **Evaluation / Judging** | score, evaluate, rate, rank, assess, compare, review                     | Criteria definition, rubric precision, bias mitigation, score anchoring    |
| **Other / Hybrid**       | does not match any single category above, or equally spans 3+ categories | See Hybrid Handling below                                                  |

**Multi-category prompts:** Identify the primary category driving the core deliverable, apply its strategy first, then layer the secondary category's enhancements.

**Hybrid Handling — when no single category fits:**

A prompt is Hybrid when it spans three or more categories with no clear primary, or when none of the 12 categories captures its core intent.

Steps:

1. List the categories present and the percentage of the output each drives
2. Assign primary to whichever category accounts for the largest share of the deliverable (even a plurality counts)
3. If truly equal (e.g. a prompt that is simultaneously Analysis + Writing + Business at equal weight): treat **Research** as primary (it imposes the most rigorous quality standards), then layer all other categories as secondaries
4. If the prompt is genuinely novel and no category's enhancement strategy applies usefully: fall back to the universal Quality Baseline from `enhancements.md` — specificity, completeness, consistency, actionability — and apply Balanced mode unless overridden

---

## Optimization Modes

| Mode           | Auto-trigger                                                   | Behavior                                                                                                                                |
| -------------- | -------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| **Concise**    | Low complexity, single-step, narrow scope                      | Minimal expansion. Clarify structure, remove filler, sharpen the ask. No new sections unless critical.                                  |
| **Balanced**   | Medium complexity, some ambiguity, standard deliverable        | Domain engine + moderate reasoning depth. Add constraints, output format, and edge cases where absent.                                  |
| **Exhaustive** | High complexity, production context, multi-system, high stakes | Full enhancement taxonomy. Adversarial checks, assumption validation, failure modes, comprehensive constraints.                         |
| **Agentic**    | Multi-step execution, tool use, autonomous decision-making     | Workflow structure from [Workflow Patterns](workflow.md). Execution flow, state management, tool usage, verification, failure recovery. |

User overrides auto-selection with `--mode concise|balanced|exhaustive|agentic`.

---

## Complexity Assessment

| Level          | Signals                                                                   | Default Mode |
| -------------- | ------------------------------------------------------------------------- | ------------ |
| **Low**        | Single-step, unambiguous intent, narrow output, no branching              | Concise      |
| **Medium**     | Multi-faceted, moderate ambiguity, some judgment required                 | Balanced     |
| **High**       | Multi-system, conflicting constraints, production use, high-stakes output | Exhaustive   |
| **Autonomous** | Multi-step execution, tool calls, decision loops, human handoff points    | Agentic      |

---

## Context Type

Identify which context type the rewritten prompt will operate in — this affects placement and structure decisions in Phase 4.

| Context Type                | Implication                                                       |
| --------------------------- | ----------------------------------------------------------------- |
| **Single-turn task**        | Full instruction set in one prompt; output format critical        |
| **Multi-turn conversation** | System prompt carries persistent context; user turn carries task  |
| **Agentic pipeline**        | Stage boundaries, state handoffs, and tool specs must be explicit |
| **Evaluation / judge**      | Rubric and scoring schema must be defined before any examples     |
| **Few-shot template**       | Example structure must be consistent; format must be enforced     |
