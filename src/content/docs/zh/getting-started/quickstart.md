---
title: 快速開始
description: 幾秒鐘內啟動並執行 CodeGraph。
---

幾秒鐘內啟動並執行 CodeGraph。

## 不需要 Node.js — 一個指令取得適合你作業系統的版本

```bash
# macOS / Linux
curl -fsSL https://raw.githubusercontent.com/colbymchenry/codegraph/main/install.sh | sh

# Windows (PowerShell)
irm https://raw.githubusercontent.com/colbymchenry/codegraph/main/install.ps1 | iex
```

## 已有 Node.js？改用 npm（任何版本皆可）

```bash
npx @colbymchenry/codegraph        # 零安裝，或：
npm i -g @colbymchenry/codegraph
```

CodeGraph 內建自己的執行環境 — 無需編譯、無原生建置，在任何地方都能一致運作。互動式安裝程式會自動設定你的代理人 — Claude Code、Cursor、Codex CLI、opencode、Hermes Agent。

## 初始化專案

```bash
cd your-project
codegraph init -i
```

就這樣 — 當 `.codegraph/` 目錄存在時，你的代理人會自動使用 CodeGraph 工具。

下一步：建立[你的第一個圖譜](/codegraph-starlight/zh/getting-started/your-first-graph/)，或查看完整的[安裝](/codegraph-starlight/zh/getting-started/installation/)選項。
