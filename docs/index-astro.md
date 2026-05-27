# 首頁解析：`src/pages/index.astro`

`src/pages/index.astro` 是整個文件站的唯一首頁（landing page）。在 Astro 框架中，`src/pages/` 目錄下的每個檔案自動對應一個 URL 路由 — `index.astro` 就是網站根目錄 `/`。

從知識圖譜的連結數量來看，它是 **fan-out 最高的節點之一**：直接依賴 4 個其他模組，也是所有 UI 元件的最終組裝點。

---

## 檔案結構：三個區塊

`.astro` 格式由三個部分組成，以 `---` 分隔：

```
───────────────────
  Frontmatter（伺服器端 JS）  ← 第 1–15 行
───────────────────
  HTML 模板                  ← 第 17–457 行
  ├── <style>
  └── <script>
───────────────────
```

### 1. Frontmatter（伺服器端，建置時執行）

```astro
import '../styles/theme.css';
import GraphDiagram from '../components/GraphDiagram.astro';
import { getStarsLabel } from '../lib/github';

const stars = await getStarsLabel();
```

這段程式碼**在建置期執行**，不會進入瀏覽器。關鍵點：

- `await getStarsLabel()` — 呼叫 GitHub API，在**建置時**就抓取 star 數，結果直接寫入靜態 HTML，使用者不會感受到任何延遲
- 所有 `import` 在這裡聲明，像普通 JavaScript 模組一樣

### 2. HTML 模板

整個 `<html>` 文件結構，分成四大區塊：

| 區塊 | 內容 |
|---|---|
| `<head>` | SEO meta、favicon、頁面標題、OG tags |
| `.nav` | 頂部導覽列：品牌文字、文件連結、GitHub、Star 數 |
| `<main>` | **Hero 區塊** + **Feature 卡片** × 3 |
| `.foot` | 頁尾連結列（Docs / GitHub / npm / MIT） |

**Hero 區塊**是頁面的核心，採用兩欄 CSS Grid 排版：

```
┌─────────────────────┬─────────────────────┐
│  hero-left（1.05fr）│  hero-right（0.95fr）│
│  標題               │                     │
│  副標題             │  <GraphDiagram />   │
│  CTA 按鈕           │  （SVG 知識圖譜）    │
│  安裝指令列         │                     │
└─────────────────────┴─────────────────────┘
```

**Features 區塊**有三個特色卡片，各以行內 SVG 圖示搭配標題與說明，分別介紹：
1. Tree-sitter parsing
2. MCP server
3. Impact analysis

### 3. `<script>`（客戶端，瀏覽器執行）

這是唯一執行在瀏覽器中的 JavaScript，功能極簡 — 複製安裝指令到剪貼簿：

```javascript
await navigator.clipboard.writeText(wrap.getAttribute('data-install'));
btn.innerHTML = '<span class="copied">Copied</span>';
setTimeout(() => (btn.innerHTML = original), 1400);  // 1.4 秒後還原
```

安裝指令（`npx @colbymchenry/codegraph`）存放在 HTML 的 `data-install` 屬性中，由伺服器端在建置時寫入，再由客戶端 script 讀取。這是「伺服器資料橋接到客戶端」的常見模式。

### 4. `<style>`（Scoped CSS）

佔整個檔案約 **70%** 的篇幅，包含：

- CSS 變數（`var(--cg-ink)`、`var(--cg-paper)`）取自 `theme.css`，是設計系統的 token
- 扁平極簡風格：無陰影、無漸層，只有 border 線條分隔區塊
- **入場動畫**（`prefers-reduced-motion` 保護）：hero-left 子元素依序淡入 + 上滑
- **響應式**：畫面寬度 ≤ 860px 時，兩欄變單欄，次要 nav 項目消失

---

## 外部依賴關係

```
index.astro
  ├── depends_on → GraphDiagram.astro   （hero 右側 SVG 視覺化元件）
  ├── depends_on → github.ts            （GitHub star 數工具）
  ├── depends_on → theme.css            （全站 CSS 設計變數）
  └── calls      → getStarsLabel()      （github.ts 匯出的函數）
```

| 依賴 | 說明 |
|---|---|
| **`GraphDiagram.astro`** | 純展示元件，在 hero 右欄渲染手工排版的 SVG 知識圖譜示意圖，hover 時節點填滿墨色 |
| **`github.ts:getStarsLabel`** | 建置時呼叫 GitHub API，回傳格式化後的 star 數字串（如 `"22k"`），帶記憶體快取與離線 fallback |
| **`theme.css`** | 提供所有 `--cg-*` CSS 自訂屬性，是整個設計系統的基礎 |

> **注意**：`SocialIcons.astro` 與 `SiteTitle.astro` **並非**由首頁引用。這兩個元件是透過 `astro.config.mjs` 注入到 Starlight 文件頁面的，首頁自己管理導覽列。

---

## 資料流

```
建置期間：
  GitHub API → getStarsLabel() → stars 變數 → 寫入 HTML <span>

瀏覽器執行：
  使用者點擊 [複製] → Clipboard API → 按鈕文字變 "Copied" → 1.4s 後還原
```

整個頁面是**靜態優先**的設計：star 數在建置時就定稿，唯一的動態行為是複製按鈕。這讓頁面可以零 JS 依賴地展示，完全相容 GitHub Pages 靜態部署。

---

## 值得注意的設計模式

| 模式 | 說明 |
|---|---|
| **Build-time data fetching** | `await` 在 frontmatter 中直接使用，star 數在 HTML 生成時內嵌，無需客戶端 API 呼叫 |
| **CSS Variables design token** | `--cg-ink`、`--cg-paper` 等 token 讓主題切換（明/暗模式）集中在 `theme.css` 一處 |
| **`data-*` 橋接** | `data-install={install}` 把伺服器端字串安全傳遞到客戶端 script，避免 XSS |
| **Progressive enhancement** | 入場動畫使用 `prefers-reduced-motion`，對動態敏感的使用者自動停用動畫 |
| **Self-contained page** | 此頁面不使用 Starlight 的 layout wrapper，完全自管 HTML 結構，讓首頁視覺風格獨立於文件頁 |
