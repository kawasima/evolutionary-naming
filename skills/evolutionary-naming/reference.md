# Evolutionary Naming — Reference

Shared knowledge for both audit-mode and improve-mode. Diagnosis criteria, transitions, anti-patterns.

Based on Arlo Belshee's "Naming as a Process" (CC BY 3.0). Updated edition: digdeeproots.com.

## Diagnosing Current Step

| Signal | Current Step |
|--------|--------------|
| Concept exists in code but has no name (embedded in a long method/class) | Missing |
| Name is misleading or tells nothing (`process`, `handle`, `data`, `manager`, `s`, `d`, `r`) | Missing/Misleading → treat as Nonsense candidate |
| Name tells one true thing but not everything (`doSomethingToDatabase`) | Honest |
| Name lists everything but is very long (`parseXmlAndStoreAndCacheAndNotify`) | Honest and Complete |
| Each piece has single responsibility but names describe mechanics | Does the Right Thing |
| Names express purpose but don't form a shared vocabulary | Intent |
| Names form a shared domain vocabulary with Value Objects | Domain Abstraction |

## Step Transitions

### Missing/Misleading → Nonsense (applesauce)

**Look at:** Long methods, long classes, long parameter lists, long expressions. Find a chunk that belongs together.

**Do:**
1. Find a chunk: a block of statements, an unclear expression, parameters that travel together.
2. Extract it (Extract Method, Introduce Variable, Introduce Parameter Object).
3. Name it `applesauce`. Obviously nonsense — nobody will mistake it for a real name.
4. For misleading names (`PageLoad`, `DataManager`): rename to `applesauce` directly. Yes, even names that LOOK reasonable. `-Manager`, `-Handler`, `process()` are misleading because they promise meaning they don't deliver.
5. **Commit immediately.** Don't batch with the next step.

Only one `applesauce` in scope at a time. If you need another, first promote the current one to Honest.

**Don't skip this step** even when a name seems "partially honest." `DocumentManager` feels like it says something, but it doesn't — it's just as misleading as `process()`.

**Exception:** A single nameless variable like `d` whose meaning is obvious from one line of context can go directly to Honest. Applesauce is for misleading names and extracted chunks.

### Nonsense → Honest

**Look at:** The body of the method/class. Find system components (`database`, `screen`, `network`), return value sources, repeated variables.

**Do:**
1. Identify one true thing about what this code does.
2. Rename to express that truth. Be specific, not generic.
3. Mark uncertainty: `probably_` prefix for things you're not sure about, `_AndStuff` suffix for unknown remaining behavior.
4. **Commit.**

```
// BAD: too generic
applesauce() → handleFlightInfo()

// GOOD: specific, honest about uncertainty
applesauce() → probably_doSomethingEvilToTheDatabase_AndStuff()
```

### Honest → Honest and Complete

**Look at:** The body. Find everything the code does that isn't yet captured in the name.

**Do iteratively:**
- **Expand the known:** Find one more action/effect not in the name → add it.
- **Narrow the unknown:** Find one specific thing the `_AndStuff` part does NOT do → make the suffix more specific.

**Goal:** Remove `probably_` (add tests to confirm) and `_AndStuff` (track all data effects). The name should let you email someone just the name and they could reconstruct exactly what the code does.

```
probably_doSomethingEvilToTheDatabase_AndStuff()
→ parseXmlAndStoreFlightToDatabaseAndLocalCacheAndBeginBackgroundProcessing()
```

Long names are GOOD here. Forget naming conventions about length. Completeness is the goal. **Commit after each addition.**

### Honest and Complete → Does the Right Thing (Phase 2)

**Look at:** Only the name. Ignore the code body and call sites.

**Do:**
1. Find a part of the name unrelated to the rest, or a concern you want to encapsulate.
2. Extract that responsibility via structural refactoring (Extract Method, Split Class, Introduce Parameter Object).
3. Distribute the complete name across the new pieces.
4. Each new piece keeps an Honest and Complete name.
5. **Commit.**

```
parseXmlAndStoreFlightToDatabaseAndLocalCacheAndBeginBackgroundProcessing()
→ parseXml() + storeFlightToDatabaseAndLocalCache() + beginBackgroundProcessing()
```

**Name by "what it does", not "what it is."** This motivates splitting.

### Does the Right Thing → Intent (Phase 3)

**Look at:** Call sites and usage context. NOT the body.

**Do:**
1. Read every place this method/class/variable is used.
2. Understand its role in the larger orchestration.
3. Rename from "what it does" to "why it exists" — its purpose.
4. **Commit.**

```
storeFlightToDatabaseAndLocalCache() → beginTrackingFlight()
```

**Danger: falling back to Nonsense.** Naming by WHEN it's called (`onPageLoad`) or by initial conditions is Nonsense, not Intent.

### Intent → Domain Abstraction (Phase 3)

**Look at:** A set of methods/classes that share something in common. Look for primitive obsession patterns.

**Signals of missing abstractions:**
- Parameters that travel together across methods
- Fields always used together
- Similar name prefixes/suffixes (`flightId`, `flightCode`, `flightStatus`)
- Names ending in `-er` (action not object), `-Manager`, `-Util`
- `firstName: String, lastName: String, ssn: String` (primitives that are a concept)

**Do:**
1. Identify the missing Value Object / Whole Value.
2. Extract it: Introduce Parameter Object → promote to class → move related methods in.
3. Name the new concept in domain language.
4. **Commit.**

## Universal Red Flags

| About to do | What's wrong |
|-------------|--------------|
| Rename `process()` to `importFlightData()` in one step | Jumped from Missing to Intent. Go through Honest and Complete first. |
| Propose 5+ renames in one batch | Each rename is a separate insight. Commit each one. |
| Name a class without reading its full body | You can't be Honest about what you haven't read. |
| Skip `probably_` / `_AndStuff` because it looks unprofessional | Misleading "professional" names cause bugs. Honest uncertainty is better. |
| Rename to shorter name at Honest and Complete | Long names are correct here. Shortening comes at Intent level. |
| Skip applesauce because the name "already says something" | `-Manager`, `-Handler`, `process()` say nothing useful. Misleading, not Honest. |
| Naming by when it's called (`onInit`, `preLoad`) | Name by what it does or why it exists. |
| Variable named same as type (`GridSquare gridSquare`) | Name by what distinguishes this instance. |
| Using CS terms (`Transformer`, `Processor`, `Handler`) at Intent level | Use domain terms the business understands. |

## Why Commit at Every Step

Each naming improvement makes the code strictly better. Committing locks in that gain. If the next step fails, you roll back to a better state than before. Don't batch commits. The naming process IS the commit history.

```
rename process to applesauce
rename applesauce to parseXmlAndStuff (honest)
expand name to parseXmlAndSaveToDatabaseAndCache (complete)
extract parseItemsFromXml (single responsibility)
rename to importEntries (intent)
```
