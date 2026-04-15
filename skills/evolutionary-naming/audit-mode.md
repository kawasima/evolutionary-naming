# Audit Mode — Exhaustive Naming Detection

## Purpose

Scan provided code and produce a **structured report** of all naming improvement opportunities. Classify every identifier by its current step. **Do not execute changes.** The output is an actionable list, not a refactor.

## Scope

**Audit only what the user provides:** pasted code, named files, named directories. Do NOT recursively scan the broader codebase unless explicitly invited (e.g. "リポジトリ全体を見て").

If the user provides a path: read those files. If the user pastes code: audit that snippet. If both: audit the union.

## Workflow

1. **List every identifier** in scope: classes, methods, fields, parameters, local variables, map/dict keys.
2. **Diagnose each** using the table in `reference.md` (Diagnosing Current Step).
3. **Group findings by phase:**
   - Phase 1 candidates (current step = Missing/Misleading/Nonsense/Honest) → safe to fix without permission
   - Phase 2 candidates (current step = Honest and Complete, needs structural change) → requires user permission
   - Phase 3 candidates (current step = Does the Right Thing or Intent, needs domain context) → note only
4. **Order within each phase** by lowest current step first (most leverage first).
5. **Output the table.** Then stop.

## Output Format

```markdown
## Naming Audit: <file or scope name>

### Phase 1 candidates (safe — pure naming, no structural change)
| Identifier | Kind | Current step | Evidence | Suggested next step |
|------------|------|--------------|----------|---------------------|
| `OrderManager` | class | Missing (misleading) | `-Manager` suffix; body parses, persists, notifies | → applesauce, then Honest |
| `process(s, x, f)` | method | Missing | generic verb, single-letter params | → applesauce |
| `r` | local var | Missing | one-letter | → record (Honest) |

### Phase 2 candidates (requires structural permission)
| Identifier | Current step | Why it's stuck | Suggested change |
|------------|--------------|----------------|------------------|
| `parseAndStoreAndNotify()` | Honest and Complete | name lists 3 concerns | extract 3 methods (Phase 2) |

### Phase 3 candidates (note only — requires domain context)
| Pattern | Observation |
|---------|-------------|
| `(id, code, ts, val)` travel together as Map keys | Possible Value Object; revisit after structural cleanup |

---
Found N Phase-1 issues, M Phase-2 candidates, K Phase-3 patterns.
**To execute:** ask "Improve `<identifier>`" to enter improve-mode for any row.
```

## Anti-Patterns — Do NOT Do These

| Temptation | Why it's wrong |
|------------|----------------|
| "Here's the audit, and while I'm here, here's the refactored code" | Audit is classification only. Refactor is improve-mode. Stop after the table. |
| Skip identifiers that "look fine" | Audit means exhaustive. Even reasonable names get a row showing their current step. |
| Suggest names instead of next-step labels | Don't propose `orderRepository` for `OrderManager`. The "Suggested next step" column is the transition (e.g., "→ applesauce, then Honest"), not a finished name. |
| Audit the broader codebase silently | Stay within the scope the user provided. If unsure, ask. |
| Group by file or by kind instead of by phase | Phase grouping reflects required permission level — that's what the user needs to plan work. |

## Diagnosis Shortcuts

For fast classification (full criteria in `reference.md`):

| Sign | Step |
|------|------|
| 1-letter name (`d`, `r`, `s`) | Missing |
| Abbreviation (`cnt`, `ts`, `val`) | Missing/Nonsense |
| `-Manager`, `-Handler`, `-Util`, `-er` | Misleading → Nonsense candidate |
| Generic verb (`process`, `handle`, `do`) | Misleading |
| Type as name (`String string`) | Missing |
| Map keys as untyped strings | Missing (each key is its own identifier) |
| Long name with `And` | Honest and Complete (Phase 2 candidate) |
| Domain term, single-purpose | Intent or Domain Abstraction |

## Handoff to Improve Mode

When the user picks one identifier from the audit ("X だけ直して", "Improve OrderManager"):
- Switch to improve-mode for that single target.
- Do NOT re-audit. The diagnosis is already in your output.
- Carry forward the current-step classification you assigned.
