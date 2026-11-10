# 8. JDBC（Q72–77）

- **JDBC**：存取資料庫的抽象層，讓 Java 程式不必理會各家資料庫的細節。
- **Driver（驅動程式）**：資料庫廠商對 JDBC API 的實作，需實作 `Connection`、`Statement`、`PreparedStatement`、`CallableStatement`、`ResultSet`、`Driver`。
- **`Class.forName()`**：載入驅動類別。

!!! warning "補充"

    **JDBC 4.0（Java 6）起會透過 SPI 自動載入驅動**，不需要再寫 `Class.forName()`。

## Statement 三兄弟

| | 用途 | 特色 |
|---|---|---|
| `Statement` | 執行靜態 SQL | 字串拼接 → **有 SQL Injection 風險** |
| `PreparedStatement` | 參數化 SQL（`?` 佔位） | **預先編譯、可重用、效能較好、防 SQL Injection** ★ |
| `CallableStatement` | 呼叫預存程序 | 用 `connection.prepareCall()` 建立 |

!!! warning "補充"

    原文漏了 PreparedStatement 最重要的優點：**防止 SQL Injection**，面試一定要講。

## 資料庫連線池
- 建立／關閉連線成本高，所以在應用啟動時先建立一批連線放進池中；需要時從池裡取，用完歸還。
- 常見實作：**HikariCP**（Spring Boot 預設）、Druid。
