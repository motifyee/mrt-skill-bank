# Output Standards

The rewritten prompt must be immediately usable — paste it into any capable AI model and get a materially better result than the original without any further editing.

---

## Self-Validation Checklist

Run every check before outputting. If any check fails, revise — do not output a failing draft.

### Intent

- [ ] Does the rewrite achieve the user's original goal?
- [ ] Has the core task been preserved — not changed, narrowed, or expanded without justification?

### Constraints

- [ ] Are all hard constraints present and unmodified?
- [ ] Have any new constraints been introduced that might conflict with user intent?

### Ambiguity

- [ ] Has all destructive ambiguity been resolved?
- [ ] Has strategic ambiguity been preserved and scaffolded (not eliminated)?
- [ ] Are all inferred assumptions stated explicitly in the prompt?

### Context Engineering

- [ ] Is the role/persona defined with depth (not just "you are an expert")?
- [ ] Is the output format specified precisely — structure, length, null handling?
- [ ] Are hard constraints placed near the top, not buried in a later section?
- [ ] If examples are warranted, are they present, correctly formatted, and consistent?
- [ ] If the prompt will process long input, do instructions precede the input?

### Reasoning

- [ ] Is the reasoning depth appropriate for the complexity level?
- [ ] For complex tasks, is a CoT scaffold or verification trigger included?
- [ ] Are negative instructions present where failure modes are predictable?

### Mode Compliance

- [ ] Is the output depth appropriate for the selected mode?
  - Concise: focused, no added sections unless essential
  - Balanced: domain engine applied, moderate detail
  - Exhaustive: full taxonomy, adversarial checks included
  - Agentic: workflow structure, state management, recovery paths

### Executability

- [ ] Can the recipient AI follow every instruction without guessing?
- [ ] Is every instruction actionable — no vague terms, no unresolvable ambiguity?
- [ ] Is every section necessary? (Remove anything that doesn't improve output quality)

### Anti-Patterns

- [ ] No generic filler (_"consider all relevant factors"_)
- [ ] No tautologies (_"write clean code"_, _"be accurate"_)
- [ ] No contradictions between sections
- [ ] No redundant constraints or repeated requirements
- [ ] No over-specification that limits useful flexibility
- [ ] No shallow persona (_"You are an expert"_ without depth)

---

## Structure Selection

Match output structure to context type and complexity. Use only the sections relevant to the specific prompt.

### Simple Task

```
[Role / Persona]
[Objective]
[Context + Constraints]
[Steps or Method]
[Output Format]
```

### Complex Task

```
[Role / Persona — deep]
[Objective — specific, measurable]
[Context — audience, platform, stakes]
[Hard Constraints — must/must-never]
[Method / Approach]
[Steps — numbered, sequential]
[Verification — what to check before finalizing]
[Output Format — structure, length, schema, null handling]
```

### Agent / Automation

→ See full structure in [Workflow Patterns](workflow.md)

### Creative / Exploratory

```
[Role / Perspective]
[Objective — what success looks like]
[Exploration Scope — what to range across]
[Guardrails — what to avoid]
[Output Criteria — how to evaluate the generated ideas]
[Output Format]
```

### Analysis / Research

```
[Role]
[Question — specific, answerable]
[Methodology — how to approach the analysis]
[Data / Source Requirements]
[Analysis Framework — what dimensions to cover]
[Bias / Completeness Checks]
[Output Format — sections, length, citation style]
```

### Extraction / Parsing

```
[Role]
[Objective]
[Output Schema — field names, types, required vs optional]
[Null / Miss Handling — what to do when a field is absent]
[Edge Case Rules — what to do with ambiguous inputs]
[Negative Guards — what NOT to infer or fabricate]
[Output Format — exact structure with example]
```

### Evaluation / Judging

```
[Role]
[Objective — what is being evaluated and why]
[Evaluation Criteria — listed individually]
[Rubric — score anchors per criterion]
[Bias Mitigation — what to evaluate independently]
[Scoring Method — per-dimension then aggregate, or holistic]
[Output Format — scores, justifications, summary]
```

---

## Formatting Rules for the Rewritten Prompt

- Use headers to separate major sections
- Numbered lists for sequential steps
- Bullet lists for requirements, constraints, and options
- **Bold** for hard constraints and critical instructions
- Tables for rubrics, schemas, and structured comparisons
- No meta-commentary about what was changed
- No preamble (_"Here is your improved prompt:"_)
- No closing remarks (_"Let me know if you'd like further refinements"_)
- Stated assumptions (from inference) go at the top of the relevant section, not in a separate meta-section

---

## Length Calibration

The rewritten prompt should be as long as necessary and no longer.

| Mode       | Length Guidance                                                             |
| ---------- | --------------------------------------------------------------------------- |
| Concise    | Shorter than or equal to original unless critical content was missing       |
| Balanced   | 1.5–2x original is typical; more if output format or context was absent     |
| Exhaustive | Length serves correctness; no cap, but every section must earn its place    |
| Agentic    | As long as the workflow requires; incomplete specs are worse than long ones |

A long rewrite is not a better rewrite. Every added sentence must improve output quality for the recipient AI — not demonstrate thoroughness to the user.
