---
name: doc-optimizer
description: Transform documentation into AI-optimized structure with hierarchical indexes and focused files. Only invoked via /doc-optimizer command.
disable-model-invocation: true
user-invocable: true
---

# AI-Optimized Documentation

Transform docs for maximum AI context efficiency.

## Invocation

```
/doc-optimizer [path]
```

- `[path]` defaults to `./docs` if not provided
- Auto-detects mode based on existing content

## Mode Detection

| Condition | Mode |
|-----------|------|
| No `[path]` directory exists | CREATE at `./docs` |
| `[path]` exists, has `.md` files | REFACTOR |
| `[path]` exists, no `.md` files | CREATE at `[path]` |

## Size Targets

**Principle:** Files should cover one coherent idea/approach/principle. Be flexible with line counts—clarity over arbitrary limits.

| Type | Target | When to Split |
|------|--------|---------------|
| Topic | 200-500 | When covering multiple distinct ideas |
| Index | 100-200 | When navigation becomes unclear |
| Reference | 500-1000 | When mixing unrelated references |

## Structure

```
ROOT/
├── INDEX.md           # Entry point + global nav (concise)
└── [category]/
    ├── INDEX.md       # Category nav + structure (detailed)
    └── NN-topic.md    # 01-name.md format
```

## Root INDEX.md Template

```markdown
# [Project] - Documentation Index

Brief one-line description of what this project does.

## Quick Navigation
| Folder | Description | Read When |
|--------|-------------|-----------|
| path/  | One-line summary | "I need to X" |

## Getting Started
1. [First thing to read](path/file.md)
2. [Second thing](path/file.md)
```

## Category INDEX.md Template

```markdown
# [Category] - Index

Brief description of this category's purpose.

## Topics
| Document | Description | Key Points |
|----------|-------------|------------|
| 01-topic.md | What it covers | Main takeaways |

## Reading Guides
### For [Role]
Start: 01-topic.md → Deep: 02-advanced.md

### By Task
| Task | Read |
|------|------|
| Do X | 01-topic.md#section |

## Structure
[ASCII tree of files in this category]
```

**Note:** Use clean headers (alphanumeric + hyphens only) for predictable anchor links. Avoid special characters.

## Topic File Template

```markdown
# [Title]

**Parent:** [Category](../INDEX.md)
**Updated:** YYYY-MM
**Status:** Active | Draft | Deprecated

## Overview
[Brief intro - what and why]

## Content
[Main content organized by sections]

## Related
- [Related doc](path.md) - why it's relevant
```

## CREATE Workflow

1. **Plan**: Scope, audiences, categories
2. **Index First**: Root INDEX.md (concise)
3. **Write Core**: Important topics first (200-500 lines)
4. **Category Indexes**: Detailed navigation per category
5. **Verify**: Run verification checklist

## REFACTOR Workflow

**CRITICAL: Launch sub-agents for every step with max parallelization.**

### Phase 1: Parallel Audit (launch simultaneously)
Launch one agent per source file/category:
- Count lines, identify boundaries, extract structure
- Each agent outputs: `{file}: {lines} lines, topics: [list]`

### Phase 2: Parallel Split (launch simultaneously)
Launch one agent per oversized file:
```
Split [FILE] into [FOLDER]/:
- 01-[name].md: [topic from audit]
- 02-[name].md: [topic from audit]
Preserve ALL content. Add headers + cross-refs.
Create [FOLDER]/INDEX.md.
```

### Phase 3: Parallel Index (launch simultaneously)
- One agent: ROOT/INDEX.md (concise, high-level)
- One agent per category: [category]/INDEX.md (detailed)

### Phase 4: Parallel Verify (launch simultaneously)
- One agent: Link check across all files
- One agent: Size check (flag only if unclear boundaries, not just line count)
- One agent: Content preservation spot-check

## Parallel Execution Note

To launch agents simultaneously, invoke all Agent tools in a single message block. Each agent works independently without waiting for others.

## Verification Checklist

- [ ] All content preserved
- [ ] Files have clear, coherent focus (flexible on size)
- [ ] Every folder has INDEX.md
- [ ] Root index is concise, category indexes are detailed
- [ ] Links work (bidirectional)
- [ ] Updated dates present
- [ ] Headers use clean characters for anchor links

## Quick Commands

```bash
# Find potentially oversized files (review manually for coherence)
# Handles spaces in filenames, strips wc whitespace
find docs -type f -name "*.md" | while read -r file; do
  lines=$(wc -l < "$file" | tr -d ' ')
  if [ "$lines" -gt 800 ]; then
    echo "$file: $lines lines"
  fi
done

# Check index coverage - only actual directories
find docs -type d | while read -r dir; do
  if [ ! -f "$dir/INDEX.md" ]; then
    echo "Missing: $dir/INDEX.md"
  fi
done
```

## Anti-Patterns

❌ Files covering multiple unrelated ideas (should split)
❌ Nested > 3 levels
❌ Duplicate content (link instead)
❌ Vague names (misc.md, other.md)
❌ Missing indexes
❌ Orphan files (not linked from index)
❌ Using line count as sole split trigger (coherence > arbitrary limits)
❌ Over-splitting single coherent ideas just to meet line targets
