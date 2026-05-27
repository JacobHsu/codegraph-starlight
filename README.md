# codegraph 文件站

[![Built with Starlight](https://astro.badg.es/v2/built-with-starlight/tiny.svg)](https://starlight.astro.build)

[**codegraph**](https://colbymchenry.github.io/codegraph) 是一個本地優先的程式碼智慧工具，能將任何程式碼庫轉換成可查詢的知識圖譜，供 AI 程式碼代理人使用。

本站是 codegraph 的中文文件網站，涵蓋快速入門、核心概念、使用指南與 API 參考，以 [Astro](https://astro.build/) + [Starlight](https://starlight.astro.build/) 建置並部署於 GitHub Pages。

## 專案結構

```
.
├── public/
├── src/
│   ├── assets/
│   ├── content/
│   │   └── docs/
│   └── content.config.ts
├── astro.config.mjs
├── package.json
└── tsconfig.json
```

文件放在 `src/content/docs/` 目錄，支援 `.md` / `.mdx` 格式，檔名即路由。

## 常用指令

在專案根目錄執行：

| 指令 | 說明 |
| :--- | :--- |
| `pnpm install` | 安裝相依套件 |
| `pnpm dev` | 啟動開發伺服器 `localhost:4321` |
| `pnpm build` | 建置正式版本至 `./dist/` |
| `pnpm preview` | 本機預覽正式建置結果 |

## 部署

push 到 `main` branch 後，GitHub Actions 自動建置並部署至：

```
https://jacobhsu.github.io/codegraph-starlight/
```

## 效益

[CodeGraph](https://github.com/colbymchenry/codegraph)，讓 AI 代理（如 Claude Code）透過知識圖譜查詢程式碼結構，取代大量的 grep + 讀檔操作。

根據 CodeGraph 官方測試數據，使用知識圖譜後：

- 平均節省 **35% token 費用**
- 減少 **57% token 使用量**
- 減少 **71% tool calls**


## 前置需求

安裝 CodeGraph CLI：

```bash
npm install -g codegraph
```

確認安裝：

```bash
codegraph --version
```

## 建立索引

在專案根目錄執行：

```bash
codegraph index
```

索引完成後會產生 `.codegraph/codegraph.db`（SQLite 知識圖譜）。

> 本專案的 `.codegraph/codegraph.db` 已建立完成。

## 與 Claude Code 整合

本專案已在根目錄建立 `.mcp.json`，內容如下：

```json
{
  "mcpServers": {
    "codegraph": {
      "command": "codegraph",
      "args": ["serve", "--mcp"]
    }
  }
}
```

### 運作機制

```
Claude Code 啟動
      ↓
讀取 .mcp.json，執行 codegraph serve --mcp
      ↓
codegraph MCP server 啟動，自動注入使用指令到 system prompt
      ↓
Claude 在每次對話中看到指令，主動使用 codegraph 查詢
（無需使用者下指令）
```

索引由背景 **file watcher** 維護，檔案變更後約 1 秒自動更新，不需手動觸發。

### 啟用步驟

1. 開啟 Claude Code（重新啟動以讀取 `.mcp.json`）
2. 出現 MCP server 授權對話時，選擇：
   - **「Use this and all future MCP servers in this project」** — 永久允許，不再詢問（推薦）
   - 「Use this MCP server」— 僅本次 session 啟用

## CodeGraph 提供的查詢工具

Claude Code 啟用後可使用以下 9 個工具：

| 工具 | 用途 |
|------|------|
| `codegraph_search` | 依名稱搜尋 symbol（函數、類別、型別） |
| `codegraph_context` | 取得 symbol 完整上下文（含呼叫者與被呼叫者） |
| `codegraph_trace` | 追蹤兩個 symbol 之間的呼叫路徑 |
| `codegraph_callers` | 查詢誰呼叫了這個函數 |
| `codegraph_callees` | 查詢這個函數呼叫了哪些函數 |
| `codegraph_impact` | 分析修改某個 symbol 會影響哪些程式碼 |
| `codegraph_node` | 取得 symbol 詳細資料（可附原始碼） |
| `codegraph_explore` | 探索相關 symbol，依檔案分組 |
| `codegraph_files` | 取得已索引的檔案結構（比掃描檔案系統快） |

## 重新索引

當程式碼有大幅更動時，重新執行索引：

```bash
codegraph index
```

## 注意事項

- `.codegraph/codegraph.db` 已加入 `.codegraph/.gitignore`，不會被 commit 進 git
- 每位開發者需在本機各自執行 `codegraph index` 建立自己的索引

## 參考資料

- [Astro](https://astro.build/) — 內容驅動的網頁框架
- [Starlight](https://starlight.astro.build/) — Astro 文件主題
- [codegraph](https://colbymchenry.github.io/codegraph/) — 程式碼知識圖譜工具
