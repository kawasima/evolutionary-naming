# Scenario 7: Audit followup - mode handoff

## Context
After receiving an audit report, user picks one specific identifier to improve.

## Setup
Imagine the previous turn produced an audit table listing OrderManager, process, data, cnt, s, x, f, r as candidates.

## User Prompt
"OrderManagerだけ改善してください。"

## What to evaluate
1. Does the agent recognize this as a handoff to improve-mode?
2. Does the agent focus on JUST `OrderManager`, not all candidates?
3. Does the agent diagnose its current step?
4. Does the agent walk through transitions (without running another audit)?
5. Does the agent honor phase boundaries (pause before structural changes)?
