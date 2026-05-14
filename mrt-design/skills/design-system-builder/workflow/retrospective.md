# Retrospective Workflow

Phase 6 post-delivery review. Run after the user has received and reviewed the output.
Purpose: capture learnings that improve the next system, not re-evaluate the last one.

---

## When to Run

Trigger this workflow when:
- The user reviews the delivered system and provides feedback (positive or corrective)
- The evaluation Agent H produces a result outside the normal pass/warn range
- A pattern failure recurs across multiple projects (same slop, same gap)
- The user explicitly asks for a retrospective

Do NOT run this workflow:
- As a routine checklist after every delivery (it's for learning, not ceremony)
- When no meaningful feedback exists
- Before the user has actually used or reviewed the output

---

## Phase 6: Retrospective

### Step 1 — Gather Signals

Collect feedback from three sources:

1. **User feedback** — explicit corrections, preferences, and surprises from the user's review
2. **Evaluation report** — which items from `schemas/evaluation_checklist.md` failed or
   received warnings, and at which quality tier the system landed
3. **Your own observations** — what creative decisions were second-guessed, what
   synthesis steps felt uncertain, what information was missing during generation

### Step 2 — Classify Each Signal

For each piece of feedback or observation, classify it into one of four types:

| Type | Description | What to do |
|------|-------------|-----------|
| **Creative miss** | The design direction, signature, or character rules missed the brand intent | Record in `LEARNINGS.md` as a refined direction preference |
| **Technical miss** | A token, validation, or accessibility issue that evaluation should have caught | Update `schemas/evaluation_checklist.md` or `validation/` file |
| **Process gap** | The interview didn't gather a critical piece of information | Update `workflow/interview_framework.md` discovery dimensions |
| **Context loss** | A constraint or preference was lost between interview and generation | Update `schemas/packet_schema.md` to add a field, or strengthen a mandatory field note |

### Step 3 — Update LEARNINGS.md

Create or update `LEARNINGS.md` in the project root with the retrospective findings.

```markdown
# Design System Learnings

Captured preferences and patterns from past generations.
Read this file at the start of each new session to avoid re-learning established patterns.

---

## [Date] — [Project Name]

### What worked
- [Specific positive finding — be concrete enough to repeat it]

### What to adjust
- [Specific correction — describe the original choice and the preferred alternative]

### Interview gaps found
- [Information that should have been gathered but wasn't — add to interview framework]

### Evaluation gaps found
- [Check that was missing or should have been stronger — add to checklist]
```

**Rules for LEARNINGS.md entries:**
- Be specific. "User preferred warmer colors" is useless. "User preferred terracotta over
  the synthetic red we derived — their audience is craftspeople, not tech consumers" is useful.
- Include the WHY when known. Preferences without rationale can't be extended to new contexts.
- Prune stale entries after 5+ projects. Old learnings that don't match current skill
  conventions should be removed, not accumulated.

### Step 4 — Skill File Updates

If the learning reveals a systematic gap (not just a one-off preference), update the
relevant skill file directly:

| Learning type | Skill file to update |
|---|---|
| Interview gap (missed question) | `workflow/interview_framework.md` |
| Synthesis gap (wrong derivation) | `workflow/generation_flow.md` |
| Evaluation gap (missed check) | `schemas/evaluation_checklist.md` |
| Token gap (missing token category) | `schemas/token_schema.md` |
| Slop pattern (new second-order slop) | `references/ai_slop_detection.md` |
| Aesthetic gap (missing preset/direction) | `references/aesthetic_directions.md` |
| Component gap (missing pattern) | `foundations/component_patterns.md` |
| Dense surface gap (dashboard/settings) | `foundations/product_type_adaptation.md` |

Only update skill files for recurring or systematic issues. One-off user preferences
belong in LEARNINGS.md, not the shared skill library.

### Step 5 — Next-Run Setup

Before closing the retrospective, prepare for the next generation:

1. Confirm `LEARNINGS.md` is updated and readable
2. Confirm any skill file changes are accurate and consistent with the rest of the skill
3. Note any open questions that need to be answered in the next interview
4. If the evaluation report showed multiple Tier 1 (Foundation) patterns, flag one
   specific creative area to focus on improving in the next session

---

## Retrospective Report Format

Deliver to the user as a brief summary (not a full document):

```
RETROSPECTIVE — [Project Name]

What landed well:
- [1-2 concrete positive findings]

What to improve:
- [1-2 specific changes for next time]

Files updated:
- [file name] — [one-line description of change]

Ready for next session.
```

Keep the report under 10 lines. Retrospectives are for learning, not reporting.

---

## Learning Accumulation Over Time

After 3+ projects using this skill, LEARNINGS.md becomes the most valuable
personalization layer — more useful than generic guidance because it's specific
to the user's preferences and product domain.

Signs that retrospectives are working:
- The interview gets shorter because settled preferences don't need re-asking
- The evaluation report shows fewer repeating failures
- Quality tier moves toward Tier 3 more consistently over time

Signs that retrospectives are not working:
- LEARNINGS.md is empty or has vague entries
- The same slop patterns recur across projects
- The user gives the same feedback repeatedly
