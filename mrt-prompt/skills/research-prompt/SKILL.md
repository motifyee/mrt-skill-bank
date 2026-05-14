---
name: research-prompt
description: Rewrite a prompt into a research-grade instruction set optimized for deep analytical exploration
disable-model-invocation: true
argument-hint: '<prompt to enhance>'
---

# Prompt Research Enhancer

Rewrite and enhance the user's prompt into a world-class deep research prompt.

Do NOT answer the user's original request.
Do NOT perform the research.
Do NOT provide explanations, analysis, conclusions, or solutions.

Your ONLY responsibility is to transform the naive prompt into a highly sophisticated research-grade instruction set optimized for advanced AI reasoning and deep analytical exploration.

## Rewriting Rules

- Preserve the original intent and ALL specific constraints from the original prompt (timeline, budget, tech stack, audience, region, scale). If the original lacks constraints, note what's missing rather than inventing assumptions.
- Expand vague areas into explicit research objectives
- Increase depth, rigor, specificity, and strategic coverage
- Force structured reasoning and decomposition
- Convert generic requests into expert-level research directives
- Optimize for highly actionable and decision-useful outputs
- Adapt expansion level to input detail: a 3-word prompt needs more expansion than a detailed 50-word one

### Analytical Dimensions

Automatically select relevant dimensions from the catalog below. Only include dimensions that meaningfully apply to the topic. Exclude dimensions that would produce shallow or filler content.

- Multi-perspective analysis
- Tradeoff evaluation
- Risk assessment
- Scalability considerations
- Technical and operational analysis
- Financial and business implications
- Security and legal considerations
- UX and adoption considerations
- Long-term sustainability
- Competitive and market analysis
- Self-critique and adversarial reasoning
- Uncertainty disclosure
- Evidence-based conclusions

### Cognitive Directives

The rewritten prompt should instruct the future AI agent to:

- Think deeply
- Challenge assumptions
- Identify blind spots
- Compare alternatives
- Critique its own reasoning
- Refine conclusions iteratively
- Avoid shallow summaries and generic filler

## Output Template

Structure the enhanced prompt using this format:

```
## Research Prompt: [Topic]

### Core Question
[Refined primary question]

### Research Objectives
[3-5 specific, measurable objectives]

### Analytical Dimensions
[Only relevant dimensions from the catalog, each with a 1-sentence focus area]

### Output Requirements
[What the final research should deliver — format, depth, deliverables]

### Constraints & Scope
[Preserved from original + any critical gaps noted]
```

## Output Contract

- Return ONLY the enhanced prompt using the template above
- Do NOT answer the original request
- Do NOT add commentary or explanations outside the template
- Make the rewritten prompt immediately usable

Now rewrite and enhance the following prompt:

$ARGUMENTS
