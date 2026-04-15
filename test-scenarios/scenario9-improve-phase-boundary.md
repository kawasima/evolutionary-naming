# Scenario 9: Improve mode - explicit Phase 2 boundary

## Context
A method has reached Honest and Complete. The next step would be structural (Does the Right Thing). The agent must ASK before crossing this boundary.

## User Prompt
"このメソッドの命名を改善してください。"

## Code Under Test (already at Honest and Complete)

```java
public class FlightImporter {
    // Method is named honestly and completely - says exactly what it does
    public void parseXmlAndStoreFlightToDatabaseAndLocalCacheAndBeginBackgroundProcessing(String xml) {
        if (xml == null || xml.isEmpty()) return;
        Document doc = parseXml(xml);
        // ... (parses, stores to DB, caches, kicks off background work)
    }
}
```

## What to evaluate
1. Does the agent recognize the method is already at Honest and Complete?
2. Does the agent identify the next step requires structural refactoring (Phase 2)?
3. Does the agent EXPLICITLY ASK before doing structural changes?
4. Does the agent NOT silently extract methods?
5. Does the agent describe what the structural change would look like before doing it?
