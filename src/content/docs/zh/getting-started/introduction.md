---
title: 簡介
description: CodeGraph 是什麼，以及它如何讓 AI 程式碼代理人更快、更省錢。
---

CodeGraph 是一個**本地優先的程式碼智慧工具**。它使用 [tree-sitter](https://tree-sitter.github.io/) 解析你的程式碼庫，將所有符號、邊、檔案儲存在本地 SQLite 資料庫中，並透過 [Model Context Protocol (MCP)](/codegraph-starlight/zh/reference/mcp-server/)、CLI 和 TypeScript 函式庫，將結果以可查詢的**知識圖譜**形式提供。

它的目的是讓 AI 程式碼代理人 — Claude Code、Cursor、Codex CLI、opencode 和 Hermes Agent — **無需掃描檔案即可回答結構性問題**。代理人不再需要透過 `grep`、`glob`、`Read` 來重建程式碼的架構，而是直接查詢預建的索引，幾個呼叫就能得到答案。

## 為什麼重要

當代理人探索程式碼庫時，大部分預算花在*探索*上 — 在讀取檔案之前先找到正確的檔案。CodeGraph 省去了這個步驟：符號關係、呼叫圖和程式碼結構已經建立索引。

在 7 個真實的開源程式碼庫上測試（每組中位數 4 次執行），給代理人使用 CodeGraph 平均：

- **省 35% 費用**
- **少 57% tokens**
- **快 46%**
- **少 71% 工具呼叫**

效益隨程式碼庫大小擴大 — 在大型 repo 上，代理人完全從索引回答，**零檔案讀取**。

## 圖譜包含什麼

- **符號** — 函式、類別、方法、型別、路由、元件等。
- **邊** — 呼叫、引入、繼承、參考及框架特定關係。
- **檔案** — 結構加上全文搜尋（FTS5）。

提取是**確定性的** — 從 AST 衍生，不使用 LLM 摘要。

## 100% 本地

資料不會離開你的機器。無需 API 金鑰，無外部服務 — 只有 `.codegraph/` 裡的一個 SQLite 資料庫。

準備好了嗎？前往[快速開始](/codegraph-starlight/zh/getting-started/quickstart/)。
