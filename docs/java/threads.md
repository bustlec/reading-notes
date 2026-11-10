# 2. 執行緒（Q11–17）

## 行程（Process）vs. 執行緒（Thread）
- 行程是執行中的應用程式，擁有獨立的記憶體空間；執行緒是行程內的執行序列，同一行程的執行緒共享記憶體。執行緒又叫輕量級行程。

## 建立執行緒的方式
1. 繼承 `Thread`
2. 實作 `Runnable`（**較推薦**：不佔用唯一的繼承名額，任務與執行緒分離）
3. 用 `Executor` 框架／執行緒池（實務首選，重用執行緒、效率高）

!!! warning "補充"

    還有實作 **`Callable` + `Future`**（有回傳值、可丟受檢例外），以及 Java 8 的 `CompletableFuture`、Java 21 的**虛擬執行緒（Virtual Threads）**。

## 執行緒狀態（⚠️ 原文非官方分類）
原文列出就緒、執行中、等待、睡眠、I/O 阻塞、同步阻塞、死亡。面試請改答 `Thread.State` 的 **6 種官方狀態**：

| 狀態 | 何時進入 |
|---|---|
| `NEW` | `new Thread()` 後、`start()` 前 |
| `RUNNABLE` | 呼叫 `start()` 後（包含就緒與實際執行中） |
| `BLOCKED` | 等待取得 `synchronized` 監視器鎖 |
| `WAITING` | `wait()`、`join()`、`LockSupport.park()`，無限期等待 |
| `TIMED_WAITING` | `sleep(ms)`、`wait(ms)`、`join(ms)` |
| `TERMINATED` | `run()` 執行完畢 |

## 同步（synchronized）與監視器（Monitor）
- 每個物件都有一把鎖（監視器）。`synchronized` 可加在**方法**（粗粒度，鎖 `this` 或 Class 物件）或**程式碼區塊**（細粒度，可指定鎖物件，鎖的範圍較小、效能較好）。
- 監視器確保同一時間只有一個執行緒能進入同步區塊；拿到鎖之前不能執行。

## 死鎖（Deadlock）
- 兩個（以上）**執行緒**互相等待對方持有的鎖，全部無限等待。
- **避免方法**：**固定取得鎖的順序**，所有執行緒都依同一順序加鎖。

!!! warning "補充"

    死鎖的 4 個必要條件：互斥、持有並等待、不可剝奪、循環等待，破壞任一即可。其他手段有 `tryLock(timeout)`、縮小鎖範圍。執行緒延伸題常考：`volatile`（可見性、禁止重排序，不保證原子性）、`ReentrantLock` vs `synchronized`、`ThreadLocal`、執行緒池 7 個參數、`wait()` 與 `sleep()` 的差別（`wait` 會釋放鎖，`sleep` 不會）。
