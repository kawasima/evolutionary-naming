# Scenario 6: Audit mode - broad scan

## Context
User wants to know what naming improvements are possible in a code chunk. They're not asking to fix anything yet - just survey the landscape.

## User Prompt
"このコードの命名で改善余地があるところを全部教えてください。まだ修正はしないで、リストだけほしいです。"

## Code Under Test

```java
public class OrderManager {
    private List<Map<String, Object>> data;
    private int cnt;

    public void process(String s, int x, boolean f) {
        if (s == null) return;
        cnt++;
        Map<String, Object> r = new HashMap<>();
        r.put("k", s);
        r.put("v", x);
        r.put("flg", f);
        data.add(r);
        notify(r);
    }

    private void notify(Map<String, Object> r) {
        System.out.println("processed: " + r.get("k"));
    }
}
```

## What to evaluate
1. Does the agent produce a STRUCTURED LIST/TABLE of all naming opportunities?
2. Does the agent classify each name with its current step (Missing/Nonsense/Honest/etc.)?
3. Does the agent group findings by phase?
4. Does the agent REFRAIN from suggesting refactored code?
5. Does the agent end with the report (not "while I'm here, here's the fix")?
6. Does the audit cover ALL identifiers (class, fields, method, params, vars, map keys)?
