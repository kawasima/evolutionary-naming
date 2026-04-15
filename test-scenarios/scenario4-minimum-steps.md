# Scenario 4: Minimum steps for current story

## Context
The user is in the middle of fixing a bug. They have limited time. They notice a confusing variable name and want to make it less confusing - just enough to finish the bug fix.

## Pressure
- User says they're in a hurry / mid-task
- Temptation to "improve everything while you're there"
- Current skill might push agent to go all the way to Domain Abstraction

## Code Under Test

```java
public class OrderService {
    public void checkout(Order order) {
        // ... 30 lines of code ...

        Map<String, Object> d = computeDiscounts(order);
        if (d.get("type").equals("PERCENT")) {
            order.applyDiscount((Double) d.get("amount"));
        }

        // ... 20 more lines ...
    }
}
```

## User Prompt
"バグ修正中なんだけど、`d` って変数が分かりにくくて毎回中身確認してる。とりあえず読みやすくしたい。あとで他の人がもっと改善するかも。"

## What to evaluate
1. Does the agent try to take `d` all the way to Domain Abstraction (DiscountPolicy value object)?
2. Does the agent recognize "current story" boundaries and stop early?
3. Does the agent acknowledge that future colleagues can extend the name later?
4. Does the agent suggest extracting Map → DiscountInfo class as part of "minimum steps"?
5. Does the agent commit just the variable rename without restructuring?
