---
title: CLI
description: CodeGraph 所有指令及其接受的旗標。
---

```bash
codegraph                         # 執行互動式安裝程式
codegraph install                 # 執行安裝程式（明確指定）
codegraph uninstall               # 從你的 agent 移除 CodeGraph（install 的反向操作）
codegraph init [path]             # 在專案中初始化（--index 同時建立索引）
codegraph uninit [path]           # 從專案移除 CodeGraph（--force 跳過提示）
codegraph index [path]            # 完整索引（--force 重新索引，--quiet 減少輸出）
codegraph sync [path]             # 增量更新
codegraph status [path]           # 顯示統計資料
codegraph query <search>          # 搜尋符號（--kind、--limit、--json）
codegraph files [path]            # 顯示檔案結構（--format、--filter、--max-depth、--json）
codegraph context <task>          # 為 AI 建構上下文（--format、--max-nodes）
codegraph callers <symbol>        # 找出呼叫某函式/方法的來源（--limit、--json）
codegraph callees <symbol>        # 找出某函式/方法的呼叫目標（--limit、--json）
codegraph impact <symbol>         # 分析變更某符號的影響範圍（--depth、--json）
codegraph affected [files...]     # 找出受變更影響的測試檔案
codegraph serve --mcp             # 啟動 MCP 伺服器
```

## 查詢指令

`query`、`callers`、`callees` 和 `impact` 均支援 `--json` 輸出機器可讀格式。

```bash
codegraph query UserService --kind class --limit 10
codegraph callers handleRequest --json
codegraph impact AuthMiddleware --depth 3
```

## affected

遞迴追蹤匯入相依關係，找出哪些測試檔案受到變更的原始碼檔案影響。選項與 CI 範例詳見 [CI 中的受影響測試](/codegraph/zh/guides/affected-tests/)。
