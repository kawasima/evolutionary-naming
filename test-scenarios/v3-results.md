# v3 (2-mode split) Test Results

## File Structure

```
~/.claude/skills/evolutionary-naming/
├── SKILL.md          (484 words) — router + phase summary + mode selection
├── reference.md      (1051 words) — shared 7-step diagnosis, transitions, red flags
├── audit-mode.md     (659 words) — exhaustive scan workflow + table output
└── improve-mode.md   (606 words) — interactive single-target + phase-boundary pause
Total: 2800 words (vs 2094 in v2 single-file)
```

## Test Results

| Scenario | Mode | Result | Notes |
|----------|------|--------|-------|
| #6 audit broad | audit | PASS | Structured table by phase, all identifiers classified, no refactored code, ended with handoff footer |
| #7 audit→improve handoff | improve | PASS (minor) | Switched to single-target focus, carried forward classification, asked before Phase 2. Compressed applesauce into commentary instead of literal rename. |
| #8 improve `d` | improve | PASS | Walked Phase 1 continuously, asked before Phase 2, didn't suggest Value Object |
| #9 improve at Honest and Complete | improve | PASS | Diagnosed correctly, did NOT silently restructure, asked permission before Phase 2 |
| #1 regression (full refactoring) | improve | PASS | Recognized "リファクタリング" as Phase 2 pre-authorization, walked all of Phase 1, did structural extraction, stopped before deep Phase 3 |

## Key Improvements vs v2 single-file

1. **Mode is explicit** — agent picks audit vs improve based on user signals from a clear table
2. **Audit output is structured** — consistent table format with phase grouping
3. **Pause discipline** — improve-mode pauses at phase boundaries by default; pre-authorization signals override
4. **Knowledge dedup** — diagnostic table and transitions live once in reference.md

## Known Minor Issue

Scenario 7 compressed the literal `applesauce` rename step into conceptual commentary. The classification and final rename were correct, but the per-step commit visibility was reduced. Not blocking; documented as an optimization opportunity.

## Migration Risk

- Frontmatter `description` unchanged → skill auto-discovery preserved
- All previous test scenarios (1-5) map to improve-mode and continue to behave correctly
- No CI/automation depends on file structure
