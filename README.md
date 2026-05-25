# codegraph 文件站

[![Built with Starlight](https://astro.badg.es/v2/built-with-starlight/tiny.svg)](https://starlight.astro.build)

**codegraph**](https://colbymchenry.github.io/codegraph) 是一個本地優先的程式碼智慧工具，能將任何程式碼庫轉換成可查詢的知識圖譜，供 AI 程式碼代理人使用。

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

## 參考資料

- [Astro](https://astro.build/) — 內容驅動的網頁框架
- [Starlight](https://starlight.astro.build/) — Astro 文件主題
- [codegraph](https://colbymchenry.github.io/codegraph/) — 程式碼知識圖譜工具
