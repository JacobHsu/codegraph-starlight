---
title: 為專案建立索引
description: 完整索引、增量同步與檔案監控。
---

## 初始化並建立索引

```bash
cd your-project
codegraph init -i      # 初始化 + 完整索引
```

`init` 建立 `.codegraph/` 目錄；`-i`/`--index` 立即開始建立索引。若要先初始化、稍後再建立索引，可省略此旗標，之後執行 `codegraph index`。

## 完整索引 vs. 增量同步

```bash
codegraph index           # 對整個專案建立完整索引
codegraph index --force   # 從頭重新建立索引
codegraph sync            # 增量同步——只處理已變更的檔案
```

`sync` 速度快，因為它只重新解析有變動的部分。適合在切換分支或批次編輯後使用。

## 自動保持最新

當 MCP 伺服器執行中時，CodeGraph 使用原生作業系統的檔案事件在背景監控專案並進行同步——經過去抖動處理，且只針對原始碼檔案。在 agent 工作期間，你不需要手動執行 `sync`。

## 查看狀態

```bash
codegraph status
```

顯示節點、邊、檔案數量、目前使用的 SQLite 後端以及日誌模式。

## 哪些檔案會被索引

副檔名對應至[支援語言](/codegraph/zh/reference/languages/)的所有檔案，排除 `.gitignore` 中的路徑以及超過 1 MB 的檔案。詳見[設定](/codegraph/zh/getting-started/configuration/)。
