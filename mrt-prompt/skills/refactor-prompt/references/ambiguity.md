# Ambiguity Intelligence

Ambiguity analysis determines whether to clarify, infer, or preserve. Mishandling any type degrades output quality. Do not treat all ambiguity the same.

---

## Ambiguity Types

### 1. Destructive Ambiguity → MUST RESOLVE before rewriting

Missing or contradictory information that will produce wrong, low-quality, or misaligned output.

**Signals:**

- Conflicting goals (_"make it detailed but brief"_ — which wins?)
- Missing audience (_"explain this"_ — to whom? what background level?)
- Underspecified deliverable (_"analyze it"_ — what question? what depth? what format?)
- Contradictory constraints (_"be comprehensive"_ + _"keep it under 200 words"_)
- Ambiguous success criteria (_"make it better"_ — better by what measure?)

**Action:** Ask targeted questions (Phase 3). Do NOT proceed without resolution.

---

### 2. Strategic Ambiguity → PRESERVE

Intentional openness that produces richer output when left open.

**Signals:**

- Creative tasks (_"explore possible approaches"_, _"generate innovative ideas"_)
- Discovery-oriented framing (_"what might we be missing?"_)
- Divergent thinking prompts (_"brainstorm X without constraints"_)

**Action:** Keep the openness. Add structure _around_ the exploration (format, output count, evaluation criteria) without constraining the exploration itself.

---

### 3. Contextual Ambiguity → INFER SILENTLY

Gaps that domain knowledge fills reliably. Asking wastes the user's time.

**Examples:**

- _"Build a REST API"_ → infer standard HTTP verbs, JSON, status codes
- _"Write unit tests"_ → infer arrange-act-assert, mocking, coverage of happy/edge/error paths
- _"Design a dashboard"_ → infer KPI-first hierarchy, chart type conventions, responsive grid

**Action:** Apply domain-standard defaults silently. Note non-obvious assumptions in the rewritten prompt as stated assumptions only if they materially affect the output.

---

### 4. Technical Ambiguity → RESOLVE WITH DEFAULTS

Under-specified technical details where conventions provide reasonable answers.

**Examples:**

- _"Use a database"_ → infer relational vs. document from surrounding context
- _"Handle errors"_ → infer try/catch, logging, user-facing message vs. silent failure based on context

**Action:** Apply the best-fit default. Only ask if two valid interpretations exist with substantially different outcomes.

---

### 5. Scope Ambiguity → INFER FROM COMPLEXITY, ASK IF HIGH-STAKES

How much to cover, how deep to go, what to include vs. exclude.

**Examples:**

- _"Summarize this"_ — paragraph or bullets? one page or one sentence?
- _"Analyze the architecture"_ — high-level overview or production-grade critique?

**Action:** Infer from surrounding context and complexity signals. Ask only if a scope mismatch would cause significant rework.

---

### 6. Format Ambiguity → INFER, ENFORCE IN REWRITE

Output structure, length, medium, or delivery form is unspecified.

**Examples:**

- No mention of output format in a data extraction prompt
- No length guidance for a document generation prompt
- No structure specified for a comparison prompt

**Action:** Infer the most useful format from the domain and context type. Specify it explicitly in the rewritten prompt — do not leave format open-ended when it matters.

---

## Resolution Decision Tree

```
Is there destructive ambiguity?
  YES → Can it be reliably inferred from domain/context?
          YES → Infer silently, note if non-obvious
          NO  → Ask (Phase 3). Max 1 round.
  NO  → Is ambiguity strategic?
          YES → Preserve + scaffold
          NO  → Infer (contextual/technical/scope/format)
```

---

## Clarification Question Standards

When asking is necessary, the quality of the question determines the quality of the answer.

**Prioritize by impact:**

1. Audience / consumer of output — who or what receives this? what do they need?
2. Success criteria — what does a correct, complete output look like?
3. Hard constraints — what must never appear, be violated, or be assumed?
4. Scope ceiling — what is explicitly out of scope?
5. Format / length — only ask if format is the primary purpose of the prompt

**Formulation rules:**

- Offer your inferred default: _"I'll assume the audience is a non-technical executive — is that right, or is this for engineers?"_
- Prefer closed questions (yes/no, choose A or B) over open-ended
- Open-ended only when the answer space can't be enumerated
- One question per ambiguity — don't double-barrel
- Batch all questions into a single message

**Hard limit:** One clarification round. If ambiguity persists after the user's response, make reasoned assumptions, state them explicitly in the rewritten prompt, and proceed.

---

## Anti-Pattern: Over-Clarification

Do NOT ask when:

- Intent is clear enough to write a high-quality prompt
- Domain conventions provide sufficient defaults
- The ambiguity is strategic — asking would constrain it harmfully
- The question is about tone, style, or format when those can be inferred
- Asking would slow the user without materially improving the output

Over-clarification signals: asking about things the original prompt already implies, asking more than 3 questions in one round, asking two rounds in a row.
