# 10. Servlet（Q91–106）

- **Servlet**：在伺服器端處理請求、產生動態內容的 Java 類別；能在無狀態的 HTTP 上管理狀態。
- **架構**：所有 Servlet 都直接或間接實作 `Servlet` 介面，通常繼承 `GenericServlet` 或 `HttpServlet`；用多執行緒同時服務多個請求。
- **Applet vs. Servlet**：Applet 在客戶端瀏覽器執行、有 UI；Servlet 在伺服器執行、沒有 UI。
- **GenericServlet vs. HttpServlet**：前者與協定無關，要覆寫 `service()`；後者專為 HTTP，覆寫 `doGet()`、`doPost()` 等方法。

!!! warning "補充"

    Jakarta EE 9 起套件從 `javax.servlet` 改名為 **`jakarta.servlet`**（Tomcat 10+、Spring Boot 3+）。Servlet 是**單例、多執行緒**的，所以**不要在 Servlet 裡用實例變數存請求狀態**（執行緒安全問題）。

## Servlet 生命週期（★）
1. 載入並實例化 → 2. **`init()`**（只執行一次）→ 3. 每個請求呼叫 **`service()`**（再分派給 `doGet`、`doPost`）→ 4. 卸載前 **`destroy()`**（只執行一次）

!!! warning "更正"

    原文寫「對每一個請求載入 Servlet 並呼叫 init」，不對。`init()` **只在第一次請求（或啟動時，設定 load-on-startup）呼叫一次**。

## GET vs. POST

| | GET | POST |
|---|---|---|
| 參數位置 | 接在 URL 後 | 放在請求主體 |
| 長度 | 受 URL 長度限制 | 原則上不限 |
| 可見性 | 出現在網址、瀏覽紀錄、伺服器日誌 | 不在網址上 |
| 語意 | 取得資料，冪等、可快取 | 送出／建立資料，非冪等 |

!!! warning "更正"

    原文說「POST 的敏感資訊對外不可見」不夠精確。POST 只是不出現在網址；**沒有 HTTPS 一樣是明文傳輸**。

## 其他重點
- **Web 應用程式**：分成表現導向（產生網頁）與服務導向（Web Service 端點）。
- **SSI（Server Side Include）**：把一或多個檔案嵌入網頁的伺服器端腳本。
- **Servlet 鏈**：前一個 Servlet 的輸出交給下一個處理，最後一個回應客戶端（概念相當於現在的 **Filter 鏈**）。
- **取得客戶端資訊**：`request.getRemoteAddr()` 取 IP，`getRemoteHost()` 取主機名。
- **HTTP 回應結構**：**狀態碼**（預設 `200 SC_OK`）＋**標頭**（過期時間、編碼等）＋**主體**（HTML、圖片等）。
- 瀏覽器與 Servlet 之間用 **HTTP** 溝通。
- **HTTP 隧道**：把其他協定包在 HTTP／HTTPS 裡傳輸。
- **URL 編碼**：把空白與特殊字元轉成 `%XX` 十六進位，反之為解碼。

## Cookie vs. Session（★）

| | Cookie | Session |
|---|---|---|
| 存放位置 | 客戶端瀏覽器 | 伺服器 |
| 資料型別 | 只能存字串 | 可存任意 Java 物件 |
| 容量 | 小（約 4KB） | 較大（佔伺服器記憶體） |
| 安全性 | 較低（使用者可見可改） | 較高 |

!!! warning "更正"

    原文說「客戶端停用 Cookie，Session 仍能正常運作」不精確。Session 通常**靠名為 `JSESSIONID` 的 Cookie** 來識別使用者；停用 Cookie 後要靠 **URL 重寫**（`response.encodeURL()`）才能維持 Session。

## 轉發（forward）vs. 重導（sendRedirect）（★）

| | forward | sendRedirect |
|---|---|---|
| 發生位置 | 伺服器內部 | 讓瀏覽器重新發請求 |
| 請求次數 | 1 次 | 2 次 |
| 網址列 | 不變 | 變成新網址 |
| request 範圍資料 | 保留 | 遺失 |
| 可否跨站 | 只能同一應用 | 可導到任何網址 |
| 速度 | 較快 | 較慢 |
