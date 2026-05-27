---
title: CI 中的受影響測試
description: 只執行實際被變更影響的測試。
---

`codegraph affected` 遞迴追蹤匯入相依關係，找出哪些測試檔案受到一組變更的原始碼檔案影響——讓 CI 只執行相關的測試。

```bash
codegraph affected src/utils.ts src/api.ts          # 以引數傳入檔案
git diff --name-only | codegraph affected --stdin    # 從 git diff 透過管道傳入
codegraph affected src/auth.ts --filter "e2e/*"      # 自訂測試檔案模式
```

## 選項

| 選項 | 說明 | 預設值 |
|---|---|---|
| `--stdin` | 從 stdin 讀取檔案清單 | `false` |
| `-d, --depth <n>` | 最大相依遍歷深度 | `5` |
| `-f, --filter <glob>` | 自訂 glob 用於識別測試檔案 | 自動偵測 |
| `-j, --json` | 以 JSON 格式輸出 | `false` |
| `-q, --quiet` | 只輸出檔案路徑 | `false` |

## CI / hook 範例

```bash
#!/usr/bin/env bash
AFFECTED=$(git diff --name-only HEAD | codegraph affected --stdin --quiet)
if [ -n "$AFFECTED" ]; then
  npx vitest run $AFFECTED
fi
```
