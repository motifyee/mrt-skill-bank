---
name: context-guard
description: >
  Prevent context bloat from large files, long documents, or verbose resources.
  Trigger whenever a task would require loading, reading, or analyzing any resource
  that is not already in the active session. Delegates reads to a scoped background
  agent (Task tool) and returns only the minimal answer — never the source.
disable-model-invocation: true
---

# VERDICT FIRST

**Never load a large or unread resource into the active session.
Always delegate to a background agent via the Task tool.
Return only the answer — not the source, not a summary of the source.**

---

## Trigger Conditions

Activate this skill when ANY of the following is true:

| Signal                            | Threshold                                    |
| --------------------------------- | -------------------------------------------- |
| File not yet read in this session | Always — regardless of size                  |
| File length                       | > 150 lines                                  |
| Document / spec length            | > 500 words                                  |
| Number of resources needed        | ≥ 2 (one agent call per scoped question)     |
| Answer fits in < 15 words         | Instruct agent to use `/caveman` (see below) |

**Do NOT activate** if the resource content is already visible in the active context window (e.g. the user pasted it, or a prior tool call returned it). Read it directly.

---

## Mechanism

Use the Claude Code **`Task` tool** to spawn a background agent.
Do not attempt to read files inline when the trigger conditions above are met.

The Task tool runs a sub-agent in isolation. Its context never bleeds into the active session. Only the value it returns enters here.

---

## Output Compression — `/caveman`

Default to `/caveman` in every agent instruction. It is a separate installed skill — invoke by name only; do not re-implement its rules here.

**Default template:**

```
/caveman

Read: [exact file path or resource reference]

Answer ONLY this: [one specific, narrow question]

Return format: [value | list | boolean | line number | identifier]
Do not include the file content, a summary, or any surrounding context.
```

**Omit `/caveman`** only when the answer involves any of the following — in these cases the agent must respond in full prose:

| Situation                                                   | Reason                                          |
| ----------------------------------------------------------- | ----------------------------------------------- |
| Destructive or irreversible operation described in the file | Compression risks misread; consequence too high |
| Multi-step sequence where fragment order matters            | Fragments can reorder dangerously               |
| Security or permission warning                              | Must be unambiguous                             |
| Agent returns an error or unexpected result                 | Diagnosis needs full context                    |

After the safe/verbose section, the agent may resume compressed output.

---

## Agent Instruction Template

Three required slots — resolve all before writing the instruction:

```
/caveman  [omit if Auto-Clarity Exception applies]

Read: [exact file path — never vague e.g. "the config file"]

Answer ONLY this: [one concern, one question]

Return format: [value | yes/no | bullet list | line number | identifier]
Do not return file content, summaries, or surrounding context.
```

**Slot rules:**

- `Read` — exact path. Resolve it in the active session before delegating; never pass ambiguity to the agent.
- `Answer ONLY this` — one question per call. Three concerns from one file = three calls.
- `Return format` — explicit shape prevents prose drift. If you can't name the shape, the question is too vague; rewrite it first.

---

## Question Scoping Rules

The agent instruction quality determines output quality. Scope tightly before delegating.

**Weak (will bloat the return):**

> "Read `schema.prisma` and tell me about the data model."

**Strong:**

> "Read `schema.prisma`. Answer ONLY: does the `User` model have a `deletedAt` field? Return: yes or no."

**Weak:**

> "Summarize `PRD.md`."

**Strong (split into focused calls):**

> Call 1: "Read `PRD.md`. Answer ONLY: what is the stated primary user persona? Return: one noun phrase."
> Call 2: "Read `PRD.md`. Answer ONLY: list the out-of-scope items. Return: bullet list."

**Rule:** if the question contains the word "summarize," "overview," "explain," or "describe" — rewrite it before delegating. These verbs produce prose. Extract the specific fact you actually need instead.

---

## Integration Rule

After the agent returns:

- Use only its output in your reply.
- Do not quote, re-expand, or paraphrase the source resource.
- Do not mention what the file contained beyond the scoped answer.
- If the answer is insufficient, spawn another scoped call — do not fall back to loading the full resource.

---

## Hard Rules

| Rule                                                                               |     |
| ---------------------------------------------------------------------------------- | --- |
| Never load an unread resource directly into this session                           | ❌  |
| Never ask an agent to "summarize" or "overview" a resource                         | ❌  |
| Never let a scoped question span more than one concern                             | ❌  |
| Always resolve the exact file path before writing the agent instruction            | ✅  |
| Always declare the return format in the agent instruction                          | ✅  |
| Default to `/caveman` in agent instructions; omit only for Auto-Clarity exceptions | ✅  |
| One scoped question per agent call                                                 | ✅  |
