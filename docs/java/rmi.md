# 9. RMI（Q78–90）— 少考，序列化仍會考

- **RMI**：Java 版的物件導向 RPC，讓程式呼叫另一個 JVM 上物件的方法，對呼叫者是位置透明的。
- **基本原則**：行為的**定義**（遠端介面）與**實作**分離，可以在不同 JVM 上執行。
- **三層架構**：
  1. **Stub/Skeleton 層**：攔截客戶端呼叫，轉給遠端服務。
  2. **遠端參考層（Remote Reference Layer）**：解析與管理對遠端物件的參考。
  3. **傳輸層（Transport Layer）**：基於 TCP/IP 連接兩個 JVM。
- **遠端介面**：標示哪些方法可被遠端呼叫，須繼承 `java.rmi.Remote`。
- **`java.rmi.Naming`**：在 Registry 中註冊與查找遠端物件（URL 格式字串）。`bind()` 綁定名稱；`rebind()` 重新綁定，會覆蓋舊的。
- **執行步驟**：編譯 → `rmic` 產生 stub → 啟動 `rmiregistry` → 啟動伺服器 → 執行客戶端。
- **Stub 的角色**：遠端物件的本地代理。流程：建立連線 → 序列化參數 → 等待結果 → 反序列化回傳值或例外 → 回傳給呼叫者。
- **DGC（分散式垃圾回收）**：用**引用計數**管理遠端物件。
- **RMISecurityManager**：沒有設定時，RMI 不會從遠端下載類別。

## 序列化（★ 仍常考）
- **Marshalling／Demarshalling**：把記憶體中的物件轉成可傳輸或可儲存的格式，以及反向還原。
- **Serialization／Deserialization**：Java 把物件轉成位元組序列（含資料與型別資訊）存到磁碟或網路，再還原成物件。

!!! warning "補充"

    類別需實作 `Serializable`；`transient` 欄位不會被序列化；要宣告 `serialVersionUID` 避免版本不相容。Java 原生反序列化有**安全漏洞風險**，實務多改用 JSON 或 Protobuf。RMI 本身已少用：`rmic` 在 Java 15 移除，RMI Activation 在 Java 17 移除，現在多用 REST 或 gRPC。
