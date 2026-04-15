# Scenario 3: -er/-Manager suffix and Primitive Obsession

## Context
The user has code with typical naming smells: -Manager classes, -er suffixes, primitive obsession.

## Pressure
- Names look "professional" and conventional
- Temptation to keep -Manager pattern since it's common
- Primitive obsession is deeply embedded

## Code Under Test

```java
public class DocumentManager {
    public void handleDocument(String fileName, String content, 
                                String author, String createdAt,
                                boolean isPublished, String category) {
        // validation
        if (fileName == null || fileName.length() > 255) 
            throw new IllegalArgumentException("invalid filename");
        if (content == null) 
            throw new IllegalArgumentException("content required");

        String normalizedName = fileName.toLowerCase().replaceAll("[^a-z0-9.]", "_");
        String storagePath = "/docs/" + category + "/" + normalizedName;
        
        saveToStorage(storagePath, content);
        
        if (isPublished) {
            notifySubscribers(author, fileName, category);
            updateSearchIndex(normalizedName, content, category);
        }
    }
}
```

## User Prompt
"このクラスの名前が良くない気がするんだけど、どう改善すればいい？"

## What to evaluate
1. Does the agent identify DocumentManager as a naming smell (-Manager)?
2. Does the agent recognize primitive obsession (fileName, author, createdAt as separate strings)?
3. Does the agent suggest incremental improvement or wholesale renaming?
4. Does the agent suggest extracting Value Objects (Document, Author)?
5. Does the agent follow the process levels or jump to Domain Abstraction?
6. Does the agent first make names Honest before restructuring?
