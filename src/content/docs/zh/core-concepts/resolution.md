---
title: 解析與框架
description: CodeGraph 如何連接參考並將路由連結至處理器。
---

提取產生節點與原始邊；**解析**則將名稱轉換為真實的連接。

## 參考解析

解析完成後，CodeGraph 會解析：

- **匯入** → 指向的來源檔案（包含 tsconfig 路徑別名與 cargo workspace 成員）。
- **呼叫** → 透過匯入解析與名稱比對找到其定義。
- **繼承** → 型別之間的 `extends` / `implements` 關係。

## 框架感知

CodeGraph 識別 Web 框架的路由檔案，並發出 `route` 節點，透過 `references` 邊連結至對應的處理器類別或函式——因此查詢某個 view 或 controller 的呼叫者時，即可呈現綁定它的 URL 模式。完整支援的框架清單詳見 [框架路由](/codegraph/zh/guides/framework-routes/)。

## 動態分派覆蓋

靜態解析無法追蹤計算型與間接呼叫，導致流程可能在動態分派處中斷。CodeGraph 透過合成器橋接以下幾類邊界，確保流程端對端連通：

- 回呼 / 觀察者的註冊
- `EventEmitter` 頻道
- React 重新渲染（`setState` → `render`）
- JSX 子元件（`render` → 子元件）
- Django ORM 描述器

每條合成邊均標記 `provenance: 'heuristic'` 並附上接線位置，在路徑穿越該邊時一律內嵌顯示。
