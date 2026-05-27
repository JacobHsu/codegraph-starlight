---
title: MCP 伺服器
description: CodeGraph 透過 MCP 提供給 AI agent 的工具。
---

CodeGraph 以 [Model Context Protocol](https://modelcontextprotocol.io/) 伺服器的形式執行。啟動方式：

```bash
codegraph serve --mcp
```

由安裝程式設定的 agent 會自動啟動此伺服器。當 `.codegraph/` 索引存在時，agent 即可使用以下工具。

## 工具

| 工具 | 用途 |
|---|---|
| `codegraph_search` | 依名稱在整個程式碼庫中搜尋符號 |
| `codegraph_context` | 為任務建構相關程式碼上下文——一次呼叫即整合搜尋、節點、呼叫者與被呼叫者 |
| `codegraph_trace` | 一次呼叫即追蹤兩個符號之間的呼叫路徑（「X 如何到達 Y」）——每一跳均附帶函式主體，並跟蹤 grep 無法追蹤的動態分派跳躍（回呼、React 重新渲染、介面 → 實作） |
| `codegraph_callers` | 找出呼叫某函式的來源 |
| `codegraph_callees` | 找出某函式所呼叫的目標 |
| `codegraph_impact` | 分析變更某符號會影響哪些程式碼 |
| `codegraph_node` | 取得特定符號的詳細資訊（可選擇附帶原始碼） |
| `codegraph_explore` | 一次呼叫即取得多個相關符號的原始碼（依檔案分組）與關聯圖 |
| `codegraph_files` | 取得已索引的檔案結構（比掃描檔案系統更快） |
| `codegraph_status` | 查看索引健康狀態與統計資料 |

## Agent 的使用建議

CodeGraph *本身就是*預建的搜尋索引。對於「X 如何運作？」、架構分析、追蹤或「X 在哪裡？」等問題，agent 應透過少量 CodeGraph 呼叫直接回答——通常**零次檔案讀取**——而非透過 `grep` + `Read` 重新推導答案。直接使用 CodeGraph 只需幾次呼叫；grep/read 式探索則需要數十次。

安裝程式會自動將此使用建議寫入各 agent 的指令檔。
