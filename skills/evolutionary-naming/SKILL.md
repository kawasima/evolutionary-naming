---
name: evolutionary-naming
description: Use when refactoring code with poor names, when asked to improve naming, or when a user struggles to name a class/method/variable. Symptoms include -Manager/-Util suffixes, single-letter variables, process/handle/do verbs, primitive obsession, god methods with multiple responsibilities.
when_to_use: |
  Two operating modes:
  - audit-mode: broad naming review, scan, list opportunities ("命名レビュー", "改善余地ある?", "全部教えて")
  - improve-mode: a specific identifier named for improvement ("`d` を改善", "DocumentManager をリファクタリング")
  Ask the user which mode if both could apply (large code paste with no specific target).
allowed-tools: Read, Grep, Glob
---

# Evolutionary Naming

## Overview

**Naming is a process, not a single step.** Names evolve through 7 progressive steps grouped into 3 phases. Match the depth of work to the user's actual story — don't run the full pipeline every time.

Based on Arlo Belshee's "Naming as a Process" (CC BY 3.0). Updated edition: digdeeproots.com.

## The Three Phases

| Phase | Steps | Nature |
|-------|-------|--------|
| **Phase 1: Insight → Name** | Missing → Nonsense → Honest → Honest and Complete | Universal. Pure naming. No structural change. Safe to walk continuously. |
| **Phase 2: Name → Structure** | Honest and Complete → Does the Right Thing | Codebase-specific. Requires structural refactoring. **Ask permission.** |
| **Phase 3: Combine for Design** | Does the Right Thing → Intent → Domain Abstraction | Requires reading call sites and domain context. **Ask permission.** |

## Choose Your Mode

This skill operates in two modes. Pick the one that matches the user's request.

| User signal | Mode | File |
|-------------|------|------|
| Specific identifier named ("`d` を改善", "DocumentManager をどうにか", "this method") | **improve-mode** | `improve-mode.md` |
| One clear target inside a code block + "リファクタリング" / "改善" | **improve-mode** | `improve-mode.md` |
| "全体の命名を見直したい", "改善余地ある?", "命名レビュー", "scan", "audit", "リストだけほしい" | **audit-mode** | `audit-mode.md` |
| Audit followup: "X だけ直して" after audit output | **improve-mode** (target = X) | `improve-mode.md` |
| Ambiguous (large code paste, no specific target, no "全体" language) | **Ask the user**: "全体を監査しますか、特定の識別子を改善しますか?" |

**Mode = audit:** Read `audit-mode.md`. Produce a structured table of all naming opportunities. Do not execute changes.

**Mode = improve:** Read `improve-mode.md`. Walk one identifier through phase transitions. Pause at phase boundaries.

Both modes share diagnostic criteria, transition mechanics, and red flags from `reference.md`.

## When to Stop (improve-mode depth signals)

| User signal | Stop at |
|-------------|---------|
| "急いでる" / "bug fix" / "とりあえず" | Honest (mid Phase 1) |
| "改善して" / generic rename | Honest and Complete (end of Phase 1) |
| "リファクタリング" | Does the Right Thing (Phase 2) |
| "設計から見直したい" / "ドメイン的に整理" | Intent or Domain Abstraction (Phase 3) |

> "You take the minimum steps to get the name to meet your need for the current story." — leaving names incomplete for colleagues to extend is a feature, not a flaw.

## Files in This Skill

- `SKILL.md` — this file. Router and overview.
- `reference.md` — shared 7-step diagnosis table, transitions, red flags, applesauce/probably_/AndStuff guidance.
- `audit-mode.md` — exhaustive scan workflow + output template.
- `improve-mode.md` — interactive single-target workflow + pause protocol.

Read the mode file you selected, plus `reference.md` for diagnosis details.
