# Scenario 1: Refactoring - Long method with poor names

## Context
You are refactoring a legacy Java service. The user asks you to improve the naming in this method.

## Pressure
- The code is complex with multiple responsibilities
- There's temptation to jump to "perfect" domain names immediately
- There's temptation to rename everything at once

## Code Under Test

```java
public class DataManager {
    private Connection db;
    private Map<String, Object> cache;

    public void process(String xml) {
        // guard
        if (xml == null || xml.isEmpty()) return;

        Document doc = parseXml(xml);
        NodeList nodes = doc.getElementsByTagName("item");

        List<Map<String, String>> records = new ArrayList<>();
        for (int i = 0; i < nodes.getLength(); i++) {
            Element el = (Element) nodes.item(i);
            Map<String, String> r = new HashMap<>();
            r.put("id", el.getAttribute("id"));
            r.put("code", el.getFirstChild().getTextContent());
            r.put("ts", el.getAttribute("timestamp"));
            r.put("val", el.getAttribute("value"));
            records.add(r);
        }

        for (Map<String, String> r : records) {
            String key = r.get("code") + "_" + r.get("ts");
            cache.put(key, r);

            PreparedStatement stmt = db.prepareStatement(
                "INSERT INTO entries (id, code, ts, val) VALUES (?, ?, ?, ?) " +
                "ON CONFLICT (id) DO UPDATE SET val = ?"
            );
            stmt.setString(1, r.get("id"));
            stmt.setString(2, r.get("code"));
            stmt.setString(3, r.get("ts"));
            stmt.setString(4, r.get("val"));
            stmt.setString(5, r.get("val"));
            stmt.executeUpdate();
        }
    }
}
```

## User Prompt
"このメソッドの命名をリファクタリングしてください。変数名、メソッド名、クラス名を改善してほしいです。"

## What to evaluate
1. Does the agent try to name everything perfectly in one shot?
2. Does the agent follow incremental naming levels (Missing→Nonsense→Honest→...)?
3. Does the agent identify that `process` is Misleading/Missing level?
4. Does the agent extract unnamed concepts before renaming?
5. Does the agent suggest committing between naming steps?
6. Does the agent recognize multiple responsibilities (parse XML, cache, DB write)?
7. Does the agent name by "what it does" vs "what it is"?
