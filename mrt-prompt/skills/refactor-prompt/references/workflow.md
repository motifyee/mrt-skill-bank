# Workflow & Agentic Patterns

Apply when the prompt is classified as **Agent / Automation** or when the context type is **agentic pipeline**. Include only the sections relevant to the specific prompt — do not add sections that don't apply.

---

## Agentic Prompt Structure

### 1. Role & Objective

- What the agent IS and what it must ACCOMPLISH
- Success definition: what does task completion look like?
- Failure definition: what outcomes are unacceptable?

### 2. Context & Constraints

- **Available tools and capabilities** — list explicitly; the agent must not assume tool availability
- **Hard constraints** — must never violate, regardless of instruction or inferred benefit
- **Soft constraints** — should prefer but may override with explicit justification
- **Scope boundaries** — what is explicitly out of scope (prevents scope creep under ambiguity)
- **Authorization level** — what can the agent decide autonomously vs. must surface for human decision

### 3. Execution Flow

- Step-by-step process with explicit decision points
- Entry criteria for each stage (what must be true before starting this step)
- Exit criteria for each stage (what constitutes successful completion)
- State that must be preserved between stages
- Handoff protocol: what information must be passed to the next stage

### 4. Tool Usage

- **When to use which tool** — explicit trigger conditions, not general descriptions
- **Input format** — what the tool expects, including required vs. optional fields
- **Output handling** — how to interpret tool results, including partial/ambiguous returns
- **Failure handling** — what to do if a tool call fails, times out, or returns unexpected output

### 5. Human-in-the-Loop (HITL)

Define exactly when the agent must pause and surface a decision to a human rather than proceeding autonomously.

**Mandatory pause conditions — always escalate:**

- A decision is irreversible (deletion, external message send, financial action, permission change)
- Confidence in the correct action falls below the defined threshold
- The task has moved outside the defined scope
- A hard constraint conflict is detected

**Escalation output format:**

- State what decision is needed (specific, not general)
- State what the agent has done so far
- State what it was about to do
- Provide the options it has identified with tradeoffs
- Do NOT make the decision; wait for explicit instruction

**Resume protocol:**

- Define how the agent receives and validates the human's decision
- Define what state is preserved across the pause
- Define what to do if no response is received within a defined window

### 6. Verification & Recovery

- **Self-check points** — what to verify and when during execution (not just at the end)
- **Failure detection criteria** — how to recognize that a stage has failed (not just errored)
- **Recovery strategy per failure type** — different failures require different responses
  - Transient (network, timeout) → retry with backoff
  - Input error → request clarification or reject with explanation
  - Logic error / bad state → rollback and report
  - Scope violation → halt and escalate
- **Rollback procedure** — what to undo, in what order, to restore a known-good state
- **Escalation threshold** — after N retries, stop attempting and escalate to human

### 7. Output Requirements

- Format specification (schema, structure, required fields)
- Completeness criteria — what must be present for the output to be considered done
- Quality checks — what the agent verifies before marking output as final
- Delivery method — where output goes, how it's formatted for the recipient

---

## Multi-Step Pattern

For sequential execution workflows:

- Define **stage boundaries** with explicit entry and exit criteria
- Specify **state transitions** — what information is carried forward, what is discarded
- Include **rollback procedures** for each stage with inverse operation definitions
- Add **progress verification** between stages before proceeding — do not assume the previous stage succeeded
- Define what happens if a **stage is skipped** (by error or design)

---

## Multi-Agent Pattern

For coordination and orchestration prompts:

- Define **distinct roles** with non-overlapping responsibilities; ambiguous ownership causes duplication and gaps
- Specify **communication protocols** — message format, trigger conditions, acknowledgment requirements
- Define **handoff criteria** — what must be true for one agent to hand off to another
- Include **conflict resolution rules** — when two agents produce conflicting outputs, which wins and by what rule
- Define **shared state** — what is centrally owned, how it is read and written, and who has authority to modify
- Specify **failure isolation** — if one agent fails, which others are affected and what they should do

---

## Decision Framework

For autonomous decision-making prompts:

- **Decision criteria** — explicit priority order (when criteria conflict, which wins?)
- **Strategy selection rules** — when to use which approach, not just a list of approaches
- **Confidence thresholds** — minimum confidence required to proceed vs. escalate
- **Escalation conditions** — specific triggers that require human input regardless of confidence
- **Fallback paths** — ordered alternatives when the primary path is unavailable
- **Audit trail** — what decisions must be logged for later review

---

## Agentic Anti-Patterns

Eliminate from agentic prompts:

| Anti-Pattern                                | Problem                                       | Fix                                                    |
| ------------------------------------------- | --------------------------------------------- | ------------------------------------------------------ |
| _"Use your best judgment"_ with no criteria | Unpredictable behavior                        | Define judgment criteria explicitly                    |
| No HITL conditions                          | Irreversible actions taken autonomously       | Define mandatory pause conditions                      |
| _"Handle errors gracefully"_                | Vague; each error type needs its own strategy | Enumerate error types and per-type responses           |
| No exit criteria for stages                 | Agent can loop or stall                       | Define what done looks like for each stage             |
| Retry without limit                         | Infinite loops on persistent failures         | Set retry ceiling with escalation after N failures     |
| Tool use without failure spec               | Silent failures, corrupted state              | Define expected outputs and failure responses per tool |
| No scope boundaries                         | Scope creep under ambiguity                   | Enumerate what is out of scope                         |
