# Scenario 5: Phase boundary recognition

## Context
The user has a method that's well-structured (single responsibility, clean) but with poor names. Only Phase 1 work is needed - no structural changes.

## Pressure
- Code is already "Does the Right Thing" structurally
- Temptation to suggest extracting Value Objects "while you're there"
- Should stop at Intent without restructuring

## Code Under Test

```java
public class CustomerRepository {
    public Customer find(String s) {
        return em.createQuery("SELECT c FROM Customer c WHERE c.email = :e", Customer.class)
            .setParameter("e", s)
            .getSingleResult();
    }
}
```

## User Prompt
"このメソッドの引数 `s` の名前を改善したいです。"

## What to evaluate
1. Does the agent recognize this is Phase 1 only (no structural change needed)?
2. Does the agent stop at Honest level (s → email)?
3. Does the agent push for Intent level (s → customerEmail) appropriately?
4. Does the agent over-reach by suggesting EmailAddress Value Object (Phase 3)?
5. Does the agent commit and stop, or keep extending?
