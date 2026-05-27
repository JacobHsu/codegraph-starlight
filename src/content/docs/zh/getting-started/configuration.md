---
title: 設定
description: CodeGraph 是零設定的 — 這在實務上代表什麼。
---

不需要任何設定 — CodeGraph 是**零設定**的。它索引所有副檔名對應到[支援語言](/codegraph-starlight/zh/reference/languages/)的檔案，並**遵守你的 `.gitignore`**：在 git repo 中透過 git 本身，在非 git 專案中則直接讀取 `.gitignore` 檔案（根目錄和巢狀，與 git 相同的方式）。

## 實務上代表什麼

- 任何 git 忽略的內容 — `node_modules`、建置輸出、`.env` 中的機密 — 都不會被索引。**若要讓某些內容不進入圖譜，將它加到 `.gitignore`。**
- 不需要撰寫或同步設定檔，也不需要為每種語言進行設定：支援是從副檔名自動判斷的。
- 大於 1 MB 的檔案會被跳過（生成的 bundle、壓縮的 JS、廠商 blob）— 它們消耗解析預算但沒有有用的符號。

:::note
已提交且未被 gitignore 的檔案*會*被索引，即使在 `vendor/` 或已提交的 `dist/` 下也是。如果你提交了不想進入圖譜的相依套件或建置目錄，將它加到 `.gitignore`。
:::

## 資料儲存在哪裡

每個專案的資料存放在專案根目錄的 `.codegraph/` 目錄中，包含 SQLite 資料庫（`codegraph.db`）。資料不會離開你的機器。
