---
title: 疑難排解
description: 最常見的 CodeGraph 問題與解決方式。
---

## 「CodeGraph not initialized」

請先在專案目錄中執行 `codegraph init`。

## 索引速度緩慢

確認 `node_modules` 及其他大型目錄已被排除（若已加入 gitignore 則會自動排除）。使用 `--quiet` 可減少輸出的額外開銷。

## MCP 出現 `database is locked`

目前版本不應出現此問題：CodeGraph 內建自己的 Node 執行環境，並在 WAL 模式下使用 Node 原生的 `node:sqlite`，此模式下並行讀取不會被寫入操作阻塞。若仍看到此錯誤：

- **你使用的是舊版（0.9 之前）安裝。** 重新安裝以取得內建執行環境——`curl -fsSL https://raw.githubusercontent.com/colbymchenry/codegraph/main/install.sh | sh`（macOS/Linux）、`irm https://raw.githubusercontent.com/colbymchenry/codegraph/main/install.ps1 | iex`（Windows），或 `npm i -g @colbymchenry/codegraph@latest`。
- **`codegraph status` 顯示 `Journal:` 非 `wal`** — WAL 無法在此檔案系統上啟用（常見於網路共享磁碟與 WSL2 的 `/mnt`），因此讀取可能被寫入阻塞。請將專案（連同其 `.codegraph/` 資料夾）移至本機磁碟。

## MCP 伺服器無法連線

請確認專案已初始化/建立索引、驗證 MCP 設定中的路徑是否正確，並確認從命令列執行 `codegraph serve --mcp` 可以正常運作。

## 符號缺失

MCP 伺服器在儲存時會自動同步（請等待幾秒）。如有需要，可手動執行 `codegraph sync`。確認該檔案的語言有[受到支援](/codegraph/zh/reference/languages/)，且未被 `.gitignore` 排除。
