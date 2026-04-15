# Improve Mode — Interactive Single-Target Improvement

## Purpose

Take ONE identifier and walk it through the naming process, one transition at a time. Pause at phase boundaries so the user can decide whether to invest in structural changes.

## Workflow

1. **Confirm the target.** Restate which identifier you're improving. If the user named multiple, ask which one to start with.
2. **Diagnose current step.** Use the diagnosis table in `reference.md`. State the step + one-line evidence.
3. **Read the user's depth signal:**
   - "急いでる" / "bug fix" / "とりあえず" → stop at Honest
   - "改善して" / "rename" with no other context → finish Phase 1 (Honest and Complete)
   - "リファクタリング" → Phase 1 + Phase 2 (Does the Right Thing)
   - "設計から見直したい" / "ドメイン的に整理したい" → all phases
4. **Walk Phase 1 continuously.** Apply transitions through Honest and Complete WITHOUT pausing for each micro-step. Phase 1 is declared safe (no structural change). Show each transition with an inline commit message.
5. **Pause before Phase 2.** State that the next step requires structural refactoring and ask:
   > 次は構造的リファクタリング（メソッド/クラス分割）が必要です。`X`, `Y`, `Z` を切り出して進めますか?
6. **Pause before Phase 3.** State that you need to read call sites / consider domain language and ask:
   > 次は呼び出し元を読んで意図を抽出します。コードベース全体の文脈が必要ですが、進めますか?
7. **Stop on user signal.** If user says "ここまでで" / "OK" / "十分" → stop and confirm final state.

## Pause Protocol

**Inside Phase 1: don't pause.** Per-step pauses (Missing → applesauce → Honest → Honest and Complete) feel like noise. Walk through them in one response, showing each commit point.

**At Phase boundaries: always pause.** The user must explicitly authorize structural changes (Phase 2) and design-level changes (Phase 3). Do not infer permission from a generic "改善して" prompt.

**Override:** If the user pre-authorized depth in their initial prompt ("リファクタリング", "全部やって", "Phase 2まで進めて"), you may cross the boundary they authorized without pausing. Pause only at the next un-authorized boundary.

## Output Shape

Each transition shows:
- The before / after of the name
- A one-line `commit:` message
- The new current step

Example:

```
1. 診断: `process(String s)` → 現在 Missing (generic verb + 1-letter param)
2. Missing → Nonsense:  process → applesauce
   commit: rename process to applesauce
3. Nonsense → Honest:   applesauce → probably_parseAndStoreFlight_AndStuff
   commit: rename applesauce to honest name
4. Honest → Honest and Complete:
   probably_parseAndStoreFlight_AndStuff → parseXmlAndStoreFlightToDatabase
   commit: complete the name with all responsibilities

到達: Honest and Complete (Phase 1 終了)

---
次は Phase 2 (Does the Right Thing) です。
このメソッドは parseXml / storeFlightToDatabase の2つの責務を持つことが名前から明らかです。
構造的リファクタリングで2つのメソッドに分割しますか?
```

## Anti-Patterns — Do NOT Do These

| Temptation | Why it's wrong |
|------------|----------------|
| Cross Phase 2 boundary without asking | Structural change touches behavior. Always ask. |
| Pause at every micro-step inside Phase 1 | Phase 1 is safe. Excessive pausing kills flow. |
| Audit other identifiers "while I'm here" | Improve-mode is single-target. For broad scans, use audit-mode. |
| Skip applesauce for misleading names like `-Manager` | The applesauce step forces letting go of false comfort. Don't skip it. |
| Use applesauce for a one-letter variable like `d` | Applesauce is for misleading names and extracted chunks. A nameless `d` can go directly to Honest. |
| Run all 7 steps when user said "急いでる" | Honor the depth signal. Stop at Honest. |
| Suggest a Value Object when only Phase 1 was authorized | Note the opportunity, don't do the work. |

## When to Defer to Audit Mode

If the user starts asking about multiple identifiers ("`r` も `cnt` も `s` も") or about general code quality ("このクラス全体を見直したい"), suggest switching to audit-mode for the broader survey. Improve-mode loses focus when the target multiplies.
