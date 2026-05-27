---
title: 整合
description: 支援的 agent 清單與手動 MCP 設定方式。
---

互動式安裝程式會自動偵測並設定每個支援的 agent——接通 MCP 伺服器並寫入其指令檔。

## 支援的 agent

- **Claude Code**
- **Cursor**
- **Codex CLI**
- **opencode**
- **Hermes Agent**

執行 `npx @colbymchenry/codegraph` 並選擇你的 agent；非互動式旗標詳見[安裝](/codegraph/zh/getting-started/installation/)。

## 手動設定

如果你想自行接通，請先全域安裝：

```bash
npm install -g @colbymchenry/codegraph
```

將 MCP 伺服器加入 `~/.claude.json`：

```json
{
  "mcpServers": {
    "codegraph": {
      "type": "stdio",
      "command": "codegraph",
      "args": ["serve", "--mcp"]
    }
  }
}
```

可選擇在 `~/.claude/settings.json` 中自動允許唯讀工具：

```json
{
  "permissions": {
    "allow": [
      "mcp__codegraph__codegraph_search",
      "mcp__codegraph__codegraph_context",
      "mcp__codegraph__codegraph_callers",
      "mcp__codegraph__codegraph_callees",
      "mcp__codegraph__codegraph_impact",
      "mcp__codegraph__codegraph_node",
      "mcp__codegraph__codegraph_status",
      "mcp__codegraph__codegraph_files"
    ]
  }
}
```

:::tip
Cursor 啟動 MCP 子程序時會使用錯誤的工作目錄。安裝程式會自動注入 `--path` 引數來處理這個問題；若你手動設定 Cursor，請明確傳入專案路徑。
:::
