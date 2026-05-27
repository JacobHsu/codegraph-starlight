---
title: 框架路由
description: CodeGraph 將 URL 模式連結至對應的處理器。
---

CodeGraph 偵測 Web 框架的路由檔案，並發出 `route` 節點，透過 `references` 邊連結至對應的處理器類別或函式。查詢某個 view 或 controller 的呼叫者時，即可呈現綁定它的 URL 模式。

| 框架 | 識別的形式 |
|---|---|
| **Django** | `urls.py` 中的 `path()`、`re_path()`、`url()`、`include()`（CBV `.as_view()`、點分隔路徑） |
| **Flask** | `@app.route('/path', methods=[…])`、Blueprint 路由 |
| **FastAPI** | `@app.get(…)`、`@router.post(…)`、所有標準方法 |
| **Express** | `app.get(…)`、`router.post(…)` 及中介軟體鏈 |
| **NestJS** | `@Controller` + `@Get/@Post/…`、GraphQL resolver、訊息/事件模式、WebSocket 訂閱 |
| **Laravel** | `Route::get()`、`Route::resource()`、`Controller@action`、tuple 語法 |
| **Drupal** | `*.routing.yml` 路由；`.module`/`.theme`/`.install`/`.inc` 中的 `hook_*` 實作 |
| **Rails** | `get '/x', to: 'users#index'`、hash-rocket 語法 |
| **Spring** | 方法上的 `@GetMapping`、`@PostMapping`、`@RequestMapping` |
| **Gin / chi / gorilla / mux** | `r.GET(…)`、`router.HandleFunc(…)` |
| **Axum / actix / Rocket** | `.route("/x", get(handler))` |
| **ASP.NET** | action 方法上的 `[HttpGet("/x")]` 屬性 |
| **Vapor** | `app.get("x", use: handler)` |
| **React Router** / **SvelteKit** | Route 元件節點 |

路由解析是自動的，無需任何設定。只要框架檔案被識別，其路由在下次索引或同步後就會出現在圖譜中。
