# 面試速記：最高頻 15 題

1. **Java 只有值傳遞**（傳物件時傳的是參考的副本）
2. **`==` vs. `equals()`、覆寫 equals 必須覆寫 hashCode**
3. **Override vs. Overload**（簽名、回傳型別、存取權限、綁定時機）
4. **介面 vs. 抽象類別**（記得補 Java 8 的 default 方法）
5. **HashMap 原理**：16 / 0.75 / 2 倍擴容 / 鏈長 8 轉紅黑樹 / 非執行緒安全
6. **HashMap vs. Hashtable vs. ConcurrentHashMap**
7. **ArrayList vs. LinkedList**（加上實務上的選擇）
8. **fail-fast vs. fail-safe**（`ConcurrentModificationException`）
9. **建立執行緒的方式**（Thread、Runnable、Callable、執行緒池）
10. **執行緒 6 種狀態**、`wait()` vs. `sleep()`
11. **synchronized、死鎖的 4 個條件與避免方式**
12. **GC**：可達性分析、分代、Metaspace 取代永久代、G1
13. **checked vs. unchecked 例外、throw vs. throws、finally 的陷阱**
14. **Servlet 生命週期、forward vs. redirect、Cookie vs. Session**
15. **PreparedStatement 防 SQL Injection、連線池**
