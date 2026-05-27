---
title: 你的第一個圖譜
description: 建立索引並對它執行第一個查詢。
---

安裝 CodeGraph 後，只需三個指令就能建立並探索圖譜。

## 索引專案

```bash
cd your-project
codegraph init -i      # 一步完成初始化 + 索引
```

`init` 建立 `.codegraph/` 目錄；`-i`（或 `--index`）立即建立完整索引。對於已存在的專案，你可以隨時重新索引：

```bash
codegraph index          # 完整索引
codegraph sync           # 對已變更檔案進行增量更新
```

## 確認是否成功

```bash
codegraph status
```

這會回報節點/邊/檔案數量、使用中的 SQLite 後端和日誌模式 — 快速確認索引已就緒的健康檢查。

## 執行查詢

```bash
codegraph query UserService          # 依名稱尋找符號
codegraph callers handleRequest      # 什麼呼叫了某個函式
codegraph callees handleRequest      # 某個函式呼叫了什麼
codegraph impact AuthMiddleware      # 變更會影響什麼
codegraph context "fix the login flow"   # 建立任務導向的上下文
```

每個指令都接受 `--json` 以取得機器可讀的輸出。查看完整的 [CLI 參考](/codegraph-starlight/zh/reference/cli/)。

## 交給你的代理人

當 `.codegraph/` 目錄存在且代理人已設定（參見[安裝](/codegraph-starlight/zh/getting-started/installation/)），你的代理人會自動使用 [MCP 工具](/codegraph-starlight/zh/reference/mcp-server/) — 不需要額外步驟。
