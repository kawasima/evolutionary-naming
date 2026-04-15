# Scenario 8: Improve mode - pause behavior in Phase 1

## Context
Single variable rename, no urgency signal. Should walk Phase 1 continuously without per-step pauses.

## User Prompt
"`d` を改善してください。"

## Code Under Test

```java
public class OrderService {
    public void checkout(Order order) {
        Map<String, Object> d = computeDiscounts(order);
        if (d.get("type").equals("PERCENT")) {
            order.applyDiscount((Double) d.get("amount"));
        }
    }
}
```

## What to evaluate
1. Does the agent walk Phase 1 (Missing → Honest) WITHOUT pausing for each micro-step?
2. Does the agent reach a reasonable Phase 1 endpoint (Honest or Honest and Complete)?
3. Does the agent NOT auto-advance into Phase 2 (extracting Discount Value Object)?
4. If approaching Phase 2, does the agent ASK before proceeding?
5. Does the agent acknowledge when stopping is appropriate?
