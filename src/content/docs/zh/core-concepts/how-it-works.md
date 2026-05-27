---
title: 運作原理
description: 提取、儲存、解析與自動同步的流程說明。
---

CodeGraph 透過四個階段將原始碼轉換為可查詢的圖形。

```
檔案 → 提取（tree-sitter）→ 資料庫（節點 / 邊 / 檔案）
              ↓
        解析（匯入、名稱比對、框架模式）
              ↓
        圖形查詢（呼叫者、被呼叫者、影響範圍）
              ↓
        上下文建構（供 AI 使用的 Markdown / JSON）
```

## 1. 提取

[tree-sitter](https://tree-sitter.github.io/) 將原始碼解析為 AST。語言專屬查詢會提取**節點**（函式、類別、方法、型別⋯）與**邊**（呼叫、匯入、繼承、實作）。繁重的解析工作在主執行緒之外進行。

## 2. 儲存

所有資料存入本地的 SQLite 資料庫（`.codegraph/codegraph.db`），並啟用 FTS5 全文搜尋。CodeGraph 優先使用原生的 `better-sqlite3`，若不可用則自動改用 WASM 後端；`codegraph status` 可查看目前使用哪個。

## 3. 解析

提取完成後，會進行參考解析：函式呼叫 → 定義、匯入 → 來源檔案、類別繼承關係，以及框架專屬模式。部分動態分派的邊界（回呼、觀察者、React 重新渲染、JSX 子元件）透過合成器橋接，使流程得以端對端連通。詳見 [解析與框架](/codegraph/zh/core-concepts/resolution/)。

## 4. 自動同步

MCP 伺服器使用原生作業系統的檔案事件（FSEvents / inotify / ReadDirectoryChangesW）監控專案。變更經過去抖動處理，並過濾為僅限原始碼檔案後進行增量同步——當你撰寫程式碼時，圖形始終保持最新，無需任何設定。
