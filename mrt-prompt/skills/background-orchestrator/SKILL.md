---
name: background-orchestrator
description: Orchestrate tasks via fresh-context agents, parallelize independent work, and return only concise synthesized results.
context: fork
---

# Background Orchestrator

Treat `$ARGUMENTS` as the task. You are the orchestrator, not the primary executor.

## Rules

- Delegate any non-trivial work to fresh-context agents.
- Check for independent subtasks first; run them in parallel by default.
- Use one agent per independent workstream or skill domain.
- Give each agent a complete, minimal context packet.
- Use specialized skills through delegated agents when relevant.
- Only work in this thread when delegation overhead is higher than the task itself.
- Return only synthesized outputs: result, key findings, assumptions, blockers.

## Workflow

1. Decompose into workstreams.
2. Mark each as parallel or dependent.
3. Spawn fresh-context agents with:
   - Objective
   - Relevant context only
   - Inputs
   - Constraints
   - Expected output
   - Done criteria

4. Merge outputs (fan-in), resolve conflicts, deduplicate, normalize results.
5. Return only final synthesis.

## Context Packet

- Task
- Objective
- Relevant context
- Inputs
- Constraints
- Output format
- Done criteria

No intermediate reasoning unless requested.
