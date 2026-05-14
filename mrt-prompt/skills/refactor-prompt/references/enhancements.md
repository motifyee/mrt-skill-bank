# Enhancement Taxonomy

Apply enhancements calibrated to classification and mode. Not every enhancement is appropriate for every prompt. Start with the domain engine, then layer additionals.

---

## Execution Order

1. **Domain engine** for primary category (see Domain Engines section)
2. **Reasoning calibration** based on complexity level
3. **Context engineering** — placement, persona, few-shot, format enforcement
4. **Secondary category enhancements** if multi-domain
5. **Anti-pattern elimination** as final pass

---

## I. Reasoning Calibration

Scale reasoning depth to complexity. Over-scaffolding simple prompts adds noise; under-scaffolding complex prompts produces shallow output.

### Low Complexity

- Sequential structure: clear step order
- Explicit output specification

### Medium Complexity

- Decomposition: break task into sub-tasks before executing
- Tradeoff framing: consider alternatives, not just the obvious path
- Prioritization: if multiple outputs, rank by importance

### High Complexity

- Self-critique pass: _"Review your answer for gaps before finalizing"_
- Adversarial reasoning: _"What assumption in this analysis might be wrong?"_
- Blind-spot detection: _"What important consideration is not yet addressed?"_
- Edge-case analysis: enumerate boundary conditions explicitly
- Assumption surfacing: state assumptions before proceeding

### Agentic Complexity

- State-aware reasoning: decisions must account for current state, not just task spec
- Failure-mode analysis: for each step, what can go wrong and how to detect it
- Rollback logic: define what constitutes a failed state and how to recover
- Verification checkpoints: explicit self-check before proceeding to next stage

---

## II. Context Engineering

Context engineering controls _where_, _how_, and _in what order_ information is placed. This is often the highest-leverage improvement available.

### Placement Priority

Content closer to the top of a prompt receives higher model weight. Structure accordingly:

1. **Role + core objective** — top, always
2. **Hard constraints** — immediately after role; cannot be buried
3. **Output format specification** — early; the model should know the target before processing
4. **Task instructions** — body
5. **Examples** — after instructions, before the actual input
6. **Background / supplementary context** — bottom
7. **The actual input to process** — last

For system/user prompt split contexts:

- **System prompt:** role, persistent constraints, output format, behavioral guardrails
- **User prompt:** task-specific input, dynamic variables, this-turn instructions

### Persona Engineering

Avoid shallow persona (_"You are an expert"_) — it adds no information. Use deep persona:

**Deep persona structure:**

- **Role:** what this entity IS (_"You are a senior infrastructure engineer"_)
- **Perspective:** how they frame problems (_"who evaluates systems for failure modes first"_)
- **Constraints:** what they would never do (_"and never recommends a solution without specifying its operational cost"_)
- **Audience stance:** how they calibrate to the reader (_"writing for a technical team that will implement your recommendations"_)

### Few-Shot Scaffolding

Use examples when:

- The output format is complex, nuanced, or easily gotten wrong
- Tone or style must be precisely calibrated
- The task involves judgment that words alone under-specify
- Edge-case handling must be demonstrated, not described

**Example count heuristics:**

- **1 example:** anchors format and tone; sufficient for simple tasks
- **2 examples:** establishes a pattern; use when variance matters
- **3+ examples:** drives generalization; use for complex classification, extraction, evaluation

**Example composition:**

- Always include at least one example that exercises an edge case or non-obvious case
- For evaluation/extraction tasks, pair positive examples with a negative example showing what to _not_ do
- Keep example format identical to the target output format — inconsistency confuses the model
- Label examples explicitly: `Example 1:`, `Input:`, `Output:` — never leave structure implicit

### Context Window Management

- Remove redundant context — the same constraint stated twice does not reinforce; it creates noise
- If the prompt will receive long input (documents, data), put processing instructions _before_ the input, not after
- For multi-document prompts, label each document clearly and state which to prioritize if they conflict

---

## III. Output Format Enforcement

Never leave output format implicit when it matters. Specify:

| Dimension               | How to Specify                                                                     |
| ----------------------- | ---------------------------------------------------------------------------------- |
| **Structure**           | Exact section headers, nesting depth, order                                        |
| **Format**              | JSON (with schema), markdown, plain text, table, bullet list                       |
| **Length**              | Word/token range, page count, item count — not "brief" or "detailed"               |
| **Completeness signal** | What indicates the output is done (_"End with a one-sentence summary"_)            |
| **Null handling**       | What to output when information is missing (_"Use 'N/A'"_, _"Omit the field"_)     |
| **Validation marker**   | What a correct output must contain (_"Must include at least 3 concrete examples"_) |

**For structured data extraction:** Always provide a schema or template with field names, types, and null behavior. Do not describe the schema in prose — show it.

**For evaluation tasks:** Provide a scoring rubric with explicit anchors:

- Not: _"Score from 1-5"_
- Yes: _"1 = missing key requirement, 3 = meets requirement with minor gaps, 5 = fully meets requirement with no gaps"_

---

## IV. Chain-of-Thought (CoT) Injection

Add explicit reasoning scaffolds for tasks requiring judgment, inference, or multi-step logic. Do not use for simple retrieval or formatting tasks.

**Patterns by task type:**

| Task Type             | CoT Pattern                                                                                                  |
| --------------------- | ------------------------------------------------------------------------------------------------------------ |
| Analysis              | _"First identify the key factors. Then evaluate each against [criteria]. Then synthesize findings."_         |
| Architecture / design | _"Start with constraints. Then propose options. Then evaluate each option's tradeoffs before recommending."_ |
| Debugging             | _"State your hypothesis first. Then describe what evidence would confirm or refute it. Then verify."_        |
| Evaluation            | _"Score each dimension independently before computing an overall assessment."_                               |
| Planning              | _"Map dependencies before sequencing. Surface risks before finalizing milestones."_                          |

**Scratchpad vs. output-only:**

- If reasoning should appear in the output: _"Show your reasoning step by step"_
- If only the conclusion should appear: _"Think through X internally, then output only Y"_
- For high-stakes outputs: _"Before finalizing, review your answer against [specific criteria]"_

---

## V. Negative Instructions

Specify what the model should NOT do when positive instructions alone leave harmful failure modes open.

**When to add negative instructions:**

- A common failure mode exists for this task type (e.g., hallucinating citations, adding unsolicited advice, over-hedging, breaking format)
- The output will be used in a context where specific patterns are unacceptable
- The prompt is for a recurring task where you know from experience what goes wrong

**Formulation:**

- Target specific behaviors, not broad categories
  - Not: _"Don't make mistakes"_
  - Yes: _"Do not infer values for fields not present in the source — use null"_
- Place negative instructions near their corresponding positive instruction, not in a separate "don'ts" section
- For creative tasks, negative instructions should guard guardrails only — do not use them to over-constrain

**Common failure modes by category:**

| Category   | Typical Negative Guard                                                                  |
| ---------- | --------------------------------------------------------------------------------------- |
| Extraction | _"Do not infer or extrapolate values not present in the source"_                        |
| Evaluation | _"Do not let overall impression bias individual dimension scores"_                      |
| Research   | _"Do not cite sources you cannot verify; mark gaps as unknown"_                         |
| Coding     | _"Do not include placeholder comments; implement or explicitly flag as TODO"_           |
| Writing    | _"Do not add a summary or conclusion unless explicitly requested"_                      |
| Agent      | _"Do not proceed past a verification checkpoint if the check fails — escalate instead"_ |

---

## VI. Self-Consistency and Verification Triggers

For complex, high-stakes, or easily-wrong outputs, instruct the model to verify before finalizing.

**Verification patterns:**

- **Format check:** _"Before outputting, verify your response matches the specified JSON schema"_
- **Completeness check:** _"Confirm all required sections are present before finalizing"_
- **Accuracy check:** _"Review each claim — remove or flag any you cannot support with the provided context"_
- **Criteria check:** _"Re-read the success criteria. Revise if any criterion is not met"_
- **Consistency check:** _"Verify no contradiction exists between your recommendations"_

Add verification triggers when:

- Output format is complex and must be machine-parseable
- The task involves claims that could be fabricated
- Multiple sections must be internally consistent
- The model is performing evaluation and scores must align with rubric anchors

---

## VII. Quality Baseline (Always Apply)

Regardless of mode or category:

- **Specificity:** replace every vague term with a concrete, measurable alternative
- **Completeness:** identify coverage gaps; add what's missing
- **Consistency:** resolve all contradictions before output
- **Actionability:** every instruction must be executable — no instruction should require the recipient to guess its meaning

---

## VIII. Domain Engines

Apply the engine for the primary category. These are sequenced — run steps in order.

| Domain                   | Engine Sequence                                                                                                                                      |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Coding**               | Architecture constraints → error handling strategy → edge cases → testability → maintainability → integration surface                                |
| **Architecture**         | Constraint enumeration → option generation → tradeoff analysis → failure modes → integration points → operational concerns                           |
| **Research**             | Hypothesis framing → evidence criteria → source quality standards → analytical methodology → bias checks → synthesis format                          |
| **Design**               | User context + goals → information hierarchy → interaction states → accessibility requirements → visual refinement                                   |
| **Business**             | Stakeholder map → market/competitive context → risk enumeration → success metrics → decision criteria                                                |
| **Writing**              | Audience profile → purpose + desired action → structural arc → tone calibration → engagement hooks → clarity pass                                    |
| **Agent / Automation**   | Goal + success definition → tool inventory → execution flow → state management → failure recovery → verification checkpoints → escalation conditions |
| **Analysis**             | Scope + question definition → methodology → data requirements → bias awareness → completeness checks → output + visualization format                 |
| **Extraction / Parsing** | Output schema definition → source mapping → null/miss handling → format fidelity → edge case coverage → validation rules                             |
| **Evaluation / Judging** | Criteria definition → rubric + score anchors → bias mitigation → independent dimension scoring → aggregation method → output format                  |
| **Planning**             | Dependency mapping → sequencing → risk identification → milestone criteria → success definition                                                      |
| **Creative**             | Goal + constraint definition → exploration scope → divergence encouragement → evaluation criteria for output selection                               |

---

## Anti-Patterns — Eliminate in Final Pass

Remove from the rewritten prompt:

| Anti-Pattern        | Example                                            | Fix                                                                                  |
| ------------------- | -------------------------------------------------- | ------------------------------------------------------------------------------------ |
| Generic filler      | _"consider all relevant factors"_                  | Enumerate the factors                                                                |
| Tautology           | _"write clean, readable code"_                     | Specify what clean means: _"use descriptive variable names, max 40-line functions"_  |
| Contradiction       | _"be comprehensive but concise"_                   | Choose one, or define the tradeoff: _"cover all failure modes in one sentence each"_ |
| Redundancy          | Same constraint in two sections                    | Remove the duplicate                                                                 |
| Over-specification  | Constraining details that limit useful flexibility | Remove constraints that don't affect quality                                         |
| Under-specification | Critical instructions left vague                   | Add concrete criteria                                                                |
| Shallow persona     | _"You are an expert"_                              | Apply deep persona structure                                                         |
| Format omission     | No output format for structured task               | Add explicit format specification                                                    |
| Unstated assumption | Assuming audience, platform, or context            | State assumptions explicitly                                                         |
