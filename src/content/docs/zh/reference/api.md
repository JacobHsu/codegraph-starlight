---
title: API
description: 以 TypeScript 函式庫方式使用 CodeGraph。
---

CodeGraph 提供 TypeScript API，公開介面為 `CodeGraph` 類別。

```typescript
import CodeGraph from '@colbymchenry/codegraph';

const cg = await CodeGraph.init('/path/to/project');
// 或開啟現有索引：
// const cg = await CodeGraph.open('/path/to/project');

await cg.indexAll({
  onProgress: (p) => console.log(`${p.phase}: ${p.current}/${p.total}`),
});

const results = cg.searchNodes('UserService');
const callers = cg.getCallers(results[0].node.id);
const context = await cg.buildContext('fix login bug', {
  maxNodes: 20,
  includeCode: true,
  format: 'markdown',
});
const impact = cg.getImpactRadius(results[0].node.id, 2);

cg.watch();   // 監控檔案變更並自動同步
cg.unwatch(); // 停止監控
cg.close();
```

## 主要方法

| 方法 | 用途 |
|---|---|
| `CodeGraph.init(path)` / `CodeGraph.open(path)` | 建立或開啟專案索引 |
| `indexAll(opts)` | 完整索引，支援進度回呼 |
| `sync()` | 增量更新 |
| `searchNodes(query)` | 全文符號搜尋 |
| `getCallers(id)` / `getCallees(id)` | 遍歷呼叫圖 |
| `getImpactRadius(id, depth)` | 變更的傳遞影響範圍 |
| `buildContext(task, opts)` | 為 AI 建構 Markdown / JSON 上下文 |
| `watch()` / `unwatch()` | 啟動 / 停止檔案監控 |
| `close()` | 關閉資料庫連線 |
