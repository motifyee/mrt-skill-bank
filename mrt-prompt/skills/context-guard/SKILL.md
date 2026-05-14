---
name: context-guard
description: >
  Prevents context bloat by delegating file reads, URL fetches, research tasks,
  and MCP resource access to isolated background agents. Returns only the scoped
  answer — never source content or summaries. Use when reading any file not already
  in context, fetching a URL, querying an MCP resource, researching a topic, or
  executing a scoped task — especially files over 150 lines, long documents, or
  when multiple resources are needed.
disable-model-invocation: true
---

# Context Guard

## Verdict

**Never load an unread resource into the active session.
Delegate to a background agent — but inject the context it needs first.
A context-starved agent produces generic slop. The handoff block is not optional.**

---

## Trigger

| Signal                                       | Action                                        |
| -------------------------------------------- | --------------------------------------------- |
| File / URL / MCP resource not yet in context | Delegate — always                             |
| Resource > 150 lines or > 500 words          | Delegate — always                             |
| Research task (web, docs, codebase scan)     | Delegate — always                             |
| Execution task where agent makes decisions   | Delegate — always, with full decision context |
| ≥ 2 resources needed                         | One agent call per scoped question            |
| Content already visible in context           | Read directly — do NOT delegate               |

---

## Context Handoff Block — Mandatory

Every agent instruction must open with this block, populated by the main session before delegating. Vague slots produce divergent output.

```
## SESSION CONTEXT
Project: [stack, framework, naming conventions — 1-3 sentences]
Current task: [what the main session is solving right now]
Decisions already made: [locked choices the agent must treat as immutable]
Constraints: [what the agent must NOT invent, change, suggest, or deviate from]
Your role: [read and report | research and report | execute]
```

**Examples of populated slots:**

- `Project`: "Nuxt 3 + .NET 9. Vue components use `<script setup>`. PostgreSQL via EF Core. No other ORMs."
- `Decisions already made`: "Auth is ZITADEL. No custom JWT. Multi-tenant by schema. Do not reopen these."
- `Constraints`: "Do not suggest alternative libraries. Do not restructure files. Return only what is asked."

If the agent's output diverges (ignores conventions, invents patterns, produces generic output) — discard it, strengthen this block, re-spawn.

---

## Delegation Protocol

Use the `Agent` tool with `run_in_background: true`. Three delegation modes — pick the one that matches the task:

### Mode 1 — Read / Analyze / Process

For: loading a file, reading a spec, scanning a directory, extracting a value.

```
## SESSION CONTEXT
Project: [...]
Current task: [...]
Decisions already made: [...]
Constraints: [do not summarize, do not restructure, return only what is asked]
Your role: read and report

Read [exact path or URL — resolve before delegating, never vague].
Question: [one specific fact you need]
Return format: [value | yes/no | list | line number | identifier]
No source content. No summaries. No surrounding context.
```

### Mode 2 — Research

For: web search, reading documentation, scanning unfamiliar parts of the codebase.

```
## SESSION CONTEXT
Project: [...]
Current task: [... and why research is needed]
Decisions already made: [agent must not re-open settled choices]
Constraints: [scope the search — do not explore beyond the question]
Your role: research and report

Research: [exact question or topic]
Scope: [web | specific URL | directory | file pattern]
Question: [one specific fact you need]
Return format: [value | list | short prose only if unavoidable]
No background context. No related findings. No unsolicited alternatives.
```

### Mode 3 — Execute

For: writing code, editing files, running commands based on a decision from the main session.
**Highest-risk mode — inject more context, not less.**

```
[No compression — execution requires unambiguous instructions]

## SESSION CONTEXT
Project: [stack, conventions, architectural decisions — be thorough]
Current task: [exact problem the main session is solving]
Decisions already made: [all locked choices — agent treats these as immutable]
Style and patterns in use: [naming, file structure, code patterns already established]
Constraints: [explicit fences — what agent must not touch, invent, or change]
Your role: execute

Do: [exact, scoped action]
File(s): [exact paths]
Must follow: [specific pattern to replicate]
Must not: [explicit prohibitions]
Return: [diff summary | confirmation | specific value]
```

**Hard fence:** if the agent encounters an architectural decision it cannot resolve within the given constraints, it must stop and return the question to the main session — not resolve it independently.

---

## Output Compression

All agent responses use compressed format unless an Auto-Clarity exception applies.

**Pattern:** `[thing] [action] [reason]. [next step].`
**Drop:** articles (a/an/the), filler (just/really/basically/actually/simply), pleasantries (sure/certainly/of course), hedging.
**Keep:** technical terms exact, code blocks unchanged, errors quoted exact.

| ❌                                                                             | ✅                                                                         |
| ------------------------------------------------------------------------------ | -------------------------------------------------------------------------- |
| "Sure! I'd be happy to help. The issue is likely caused by a missing token..." | "Auth middleware missing token. `expiresAt` check uses `<` not `<=`. Fix:" |

### Auto-Clarity Exceptions

Drop compression temporarily for:

- Security warnings or permission issues
- Destructive / irreversible operations
- Multi-step sequences where fragment order risks misread
- Agent returns error or unexpected result
- Execute mode instructions (always uncompressed)

Resume compression after the critical section.

---

## Question Scoping

| ❌ Weak — produces prose                          | ✅ Strong — extracts a fact                                                   |
| ------------------------------------------------- | ----------------------------------------------------------------------------- |
| "Tell me about the data model in `schema.prisma`" | "Does `User` have `deletedAt`? Return: yes/no"                                |
| "Summarize `PRD.md`"                              | "Primary user persona? Return: one noun phrase"                               |
| "Research auth options"                           | "Does ZITADEL support per-tenant JWKS endpoints? Return: yes/no + source URL" |

**Rule:** if your question contains _summarize / overview / explain / describe_ — rewrite it. Extract the specific fact instead. One concern per call. Three questions = three calls.

---

## Integration

After any agent returns:

- Use only its output — do not re-expand, paraphrase, or quote the source.
- Insufficient answer → spawn another scoped call. Never fall back to loading the full resource.
- Divergent output → strengthen the Context Handoff Block and re-spawn.

---

## Hard Rules

| Rule                                                                               |     |
| ---------------------------------------------------------------------------------- | --- |
| Never load an unread resource directly into this session                           | ❌  |
| Never spawn an agent without a fully populated Context Handoff Block               | ❌  |
| Never use: summarize / overview / explain / describe in a scoped question          | ❌  |
| Never let a scoped question span > 1 concern                                       | ❌  |
| Never let an execute-mode agent resolve architectural ambiguity independently      | ❌  |
| Always resolve exact path / URL before delegating                                  | ✅  |
| Always declare return format in the agent instruction                              | ✅  |
| Compress output by default; drop only for Auto-Clarity exceptions and execute mode | ✅  |
| One question per agent call                                                        | ✅  |
| Divergent output → strengthen handoff block → re-spawn                             | ✅  |
