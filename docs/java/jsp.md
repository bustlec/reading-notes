# 11. JSP（Q107–115）— 少考

- **JSP**：靜態內容（HTML、XML）＋ JSP 元素的文字文件，本質上會被**編譯成 Servlet**。
- **處理流程**：請求 `.jsp` → 容器把 JSP 轉譯、編譯成 Servlet（**只在第一次請求或檔案變更時**）→ 執行 Servlet → 回應。
- **優點**：動態編譯好更新、可預先編譯、容易與靜態模板結合、可自訂標籤庫、邏輯與頁面分離。

## JSP 語法元素

| 類型 | 語法 | 說明 |
|---|---|---|
| 指令 Directive | `<%@ ... %>` | `include`（靜態包含）、`page`（錯誤頁、緩衝區等）、`taglib`（自訂標籤庫） |
| 動作 Action | `<jsp:xxx>` | `include`（動態包含）、`useBean`、`setProperty`、`getProperty`、`forward`、`plugin` |
| Scriptlet | `<% ... %>` | 嵌入的 Java 程式碼 |
| 宣告 Declaration | `<%! ... %>` | 宣告變數或方法 |
| 表達式 Expression | `<%= ... %>` | 值轉成字串輸出 |

## 九大隱含物件（★ 背起來）
`request`、`response`、`session`、`application`、`page`、`pageContext`、`out`、`config`、`exception`

!!! info "記憶法"

    **四個作用域物件** `pageContext` / `request` / `session` / `application`（範圍由小到大）＋ **輸出入** `response` / `out` ＋ **其他** `page` / `config` / `exception`（只在錯誤頁可用）。

!!! warning "補充"

    「靜態 include（指令）」vs.「動態 include（動作）」是 JSP 最常見的比較題。前者在編譯期合併成同一個 Servlet；後者在執行期分別執行再合併輸出。現代專案多改用 Thymeleaf 或前後端分離，JSP 已少見。
