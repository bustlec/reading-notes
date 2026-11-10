# 4. 垃圾回收 GC（Q35–42）

- **目的**：找出並回收程式不再使用的物件，釋放記憶體。
- **何時可被回收**：物件變得**不可達（unreachable）**時。
- `System.gc()`／`Runtime.gc()`：只是**建議** JVM 回收，何時執行由 JVM 決定。
- 物件設為 `null` **不會立即釋放**，要等下一次 GC。
- **堆積（Heap）**：執行期資料區，所有物件實例與陣列都在這裡配置，JVM 啟動時建立，由 GC 管理。
- **Serial vs. Throughput 收集器**：Serial 單執行緒，適合小型應用；Throughput（Parallel）用多執行緒回收新生代，適合中大型應用。
- **永久代（PermGen）**：存放類別中繼資料；滿了會觸發 Full GC，所以大小要設對。

!!! warning "補充（原文停在 Java 7）"

    - **Java 8 移除永久代，改為 Metaspace（元空間）**，使用本機記憶體。
    - **判斷存活用「可達性分析」（從 GC Roots 出發）**，不是引用計數（引用計數無法處理循環引用）。
    - 堆積分代：**新生代（Eden + 2 個 Survivor）**／**老年代**；新生代用複製演算法（Minor GC），老年代用標記-清除／標記-整理。
    - **Java 9 起預設收集器是 G1**；低延遲可選 ZGC、Shenandoah。CMS 在 Java 14 已移除。
    - 四種參考：強、軟（SoftReference）、弱（WeakReference）、虛（PhantomReference）。

## finalize()
- 原文：GC 回收物件前會呼叫 `finalize()`，可用來釋放資源。

!!! warning "更正"

    `finalize()` 執行時間不確定、甚至可能不執行，**Java 9 已標為過時，Java 18 標記為將移除（JEP 421）**。釋放資源請用 **try-with-resources**（`AutoCloseable`）或 `Cleaner`。
