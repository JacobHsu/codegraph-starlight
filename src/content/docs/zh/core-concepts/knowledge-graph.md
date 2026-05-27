---
title: 知識圖譜
description: 圖譜所使用的節點與邊的種類。
---

CodeGraph 儲存三種資料：**節點**（符號與檔案）、**邊**（節點之間的關聯）以及**檔案**。每個節點和邊都帶有精確的 `kind`，取自固定詞彙表，確保跨語言的查詢結果一致。

## 節點種類

`file`、`module`、`class`、`struct`、`interface`、`trait`、`protocol`、`function`、`method`、`property`、`field`、`variable`、`constant`、`enum`、`enum_member`、`type_alias`、`namespace`、`parameter`、`import`、`export`、`route`、`component`。

## 邊的種類

`contains`、`calls`、`imports`、`exports`、`extends`、`implements`、`references`、`type_of`、`returns`、`instantiates`、`overrides`、`decorates`。

## 來源標記

大多數邊直接來自 AST。少數邊——位於靜態解析無法追蹤的動態分派邊界——屬於**合成邊**，標記為 `provenance: 'heuristic'` 並附上產生它的接線位置。這些資訊會在 `trace`、`node` 路徑以及 `context` 呼叫路徑中內嵌顯示，讓 AI agent 能清楚看到連接的來源。

## 查詢方式

- **搜尋**：依名稱搜尋符號（FTS5）。
- **呼叫者 / 被呼叫者**：逐跳遍歷呼叫圖。
- **影響範圍**：計算某個變更的傳遞影響半徑。
- **追蹤**：一次呼叫即可取得兩個符號之間的完整呼叫路徑。

查詢方式詳見 [CLI](/codegraph/zh/reference/cli/) 與 [MCP 伺服器](/codegraph/zh/reference/mcp-server/) 參考文件。
