---
title: 安裝
description: 安裝 CodeGraph 並設定你的 AI 程式碼代理人。
---

## 1. 執行安裝程式

```bash
npx @colbymchenry/codegraph
```

安裝程式將會：

- 詢問要設定哪些代理人 — 從 **Claude Code**、**Cursor**、**Codex CLI**、**opencode** 和 **Hermes Agent** 中自動偵測已安裝的代理人。
- 提示是否將 `codegraph` 安裝到你的 `PATH`（讓代理人能啟動 MCP 伺服器）。
- 詢問設定是套用到所有專案還是僅限目前專案。
- 為每個選定的代理人寫入 MCP 伺服器設定及說明檔案（例如 `CLAUDE.md`、`.cursor/rules/codegraph.mdc`、`~/.codex/AGENTS.md`）。
- 當 Claude Code 是目標之一時，設定自動允許權限。
- 初始化目前專案（僅限本地安裝）。

## 非互動模式（腳本 / CI）

```bash
codegraph install --yes                              # 自動偵測代理人，全域安裝
codegraph install --target=cursor,claude --yes       # 指定目標清單
codegraph install --target=auto --location=local     # 偵測到的代理人，專案本地
codegraph install --print-config codex               # 列印片段，不寫入檔案
```

| 旗標 | 值 | 預設 |
|---|---|---|
| `--target` | `auto`、`all`、`none` 或 csv（`claude,cursor,…`） | 提示 |
| `--location` | `global`、`local` | 提示 |
| `--yes` | （布林值） | 每步驟提示 |
| `--no-permissions` | （布林值）跳過 Claude 自動允許清單 | 啟用權限 |
| `--print-config <id>` | 輸出單一代理人的片段並退出 | — |

## 2. 重新啟動你的代理人

重新啟動你的代理人（Claude Code / Cursor / Codex CLI / opencode / Hermes Agent）以載入 MCP 伺服器。

## 3. 初始化專案

```bash
cd your-project
codegraph init -i
```

這會建立每個專案的知識圖譜索引，並連接所有專案本地的代理人介面，讓單次全域 `codegraph install` 在你開啟的每個專案中都能運作。

## 支援的平台

每個版本都附帶獨立建置（內建 Node 執行環境 — 無需編譯），支援三大桌面作業系統，x64 和 arm64 皆有：

| 平台 | 架構 | 安裝方式 |
|---|---|---|
| Windows | x64, arm64 | PowerShell 安裝程式或 npm |
| macOS | x64, arm64 | shell 安裝程式或 npm |
| Linux | x64, arm64 | shell 安裝程式或 npm |

## 解除安裝

改變主意了？一個指令從所有設定的代理人中移除 CodeGraph：

```bash
codegraph uninstall
```

這會反轉安裝程式的操作 — 從每個已設定的代理人移除 CodeGraph 的 MCP 伺服器設定、說明和權限。你的專案索引（`.codegraph/`）不會受影響；使用 `codegraph uninit` 逐專案移除。使用 `--target` 從特定代理人移除，或使用 `--yes` 以非互動模式執行。
