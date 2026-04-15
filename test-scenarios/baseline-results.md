# Baseline Test Results (WITHOUT skill)

## Common Patterns Observed

### 1. 一発で完璧な名前にジャンプする
3つのシナリオ全てで、エージェントはMissing/Nonsenseレベルの名前を直接Intent/Domain Abstractionレベルに変換した。
- `DataManager` → `ItemXmlImporter` (一発でIntentレベル)
- `process()` → `importFromXml()` (一発でIntentレベル)
- `DocumentManager` → `DocumentPublisher` (一発でDomain Abstractionレベル)
- 新規コードでも `OrderFulfillmentService` を即座に提案

### 2. 段階的プロセスが完全に欠如
- Nonsense(Applesauce)を中間ステップとして使うことは一度もなかった
- Honest → Honest and Complete → Does the Right Thing の段階を踏まなかった
- 名前の進化を見せず、Before→After の2状態しか示さなかった

### 3. 複数責務の同時リネーム
- 全変数・メソッド・クラスを一度にリネームした
- 個別にコミットする提案はなかった
- 段階的に改善して各ステップでチェックインする概念がなかった

### 4. 「何をしているか」を経由せず「目的」に直行
- Honest and Completeレベル（メソッドがしている全てを名前に含める）を完全にスキップ
- `process()` → `parseXmlAndCacheAndUpsertToDatabase()` のようなHonest名が一度も出なかった
- 代わりに `importFromXml()` という抽象化されたIntent名に直行

### 5. 新規コードでは質問なく即答
- ユーザーの現在の命名状態を確認する質問がなかった
- 複数責務メソッドの問題を「追加検討」として触れるだけで、プロセスの核心として扱わなかった
- Scenario 2: OrderFulfillmentService + 4メソッドを即座に提案

### 6. Primitive Obsessionの認識は部分的
- Scenario 3: パラメータオブジェクト導入を「さらなる改善」として後付けで言及
- しかし進化的プロセスの一環としてではなく、Design Tip として扱った
- `-Manager` がアンチパターンであることは認識したが、Nonsenseレベルだという文脈ではなかった

## Key Rationalizations (verbatim patterns)
1. 「最もこのコードの意図に合っている」— 意図が分かる前にIntent名を付けている
2. 「業界標準的な命名」— 標準に頼って段階的理解をスキップ
3. 「推奨セット」— 一括提案、段階的改善なし
4. 「さらなる改善の視点（名前以外）」— 構造改善を命名プロセスの外に追い出す
5. 「チームの好みやドメイン用語に合わせて調整してください」— 判断を丸投げ

## Summary
エージェントは命名を「1ステップの意思決定」として扱い、「プロセス」として扱わなかった。
これは記事の冒頭が指摘する問題そのもの：「多くの人が、1回で最高の命名をしようとする」
