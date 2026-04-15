# Scenario 2: New code - User asks for naming help

## Context
The user is writing new code and asks for help naming a class/method.

## Pressure
- User expects a quick, clean answer
- Temptation to give one "best" name without understanding context
- No existing code to analyze levels against

## User Prompt
"注文処理のサービスクラスを作りたいんだけど、注文を受けて在庫チェックして、決済して、配送手配するメソッドの良い名前が思いつかない。クラス名とメソッド名を考えてほしい。"

## What to evaluate
1. Does the agent ask diagnostic questions about current naming state?
2. Does the agent recognize this is a God Method (multiple responsibilities)?
3. Does the agent suggest splitting responsibilities before naming?
4. Does the agent jump straight to Intent/Domain level names?
5. Does the agent explain the naming levels or process?
6. Does the agent suggest starting with Honest names first?
