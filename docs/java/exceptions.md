# 5. 例外處理（Q43–47）

## 例外的分類
```
Throwable
├── Error            （程式不應捕捉：OutOfMemoryError、StackOverflowError）
└── Exception        （程式可捕捉處理）
    ├── RuntimeException  → 非受檢（unchecked）：NullPointerException、IndexOutOfBounds…
    └── 其他              → 受檢（checked）：IOException、SQLException…
```

| | 受檢例外 checked | 非受檢例外 unchecked |
|---|---|---|
| 編譯器要求 | **必須** try-catch 或 `throws` 宣告 | 不強制 |
| 代表 | 可預期的外部問題 | 多半是程式 bug |

## 關鍵字比較
- **`throw`**：在程式中**主動拋出**一個例外物件。
- **`throws`**：在方法簽名**宣告**此方法可能拋出、自己不處理的例外，多個用逗號分隔。
- **`finally`**：不論是否發生例外都會執行（即使沒有 catch），用來釋放資源（I/O、資料庫連線）。
- **`finally` vs. `finalize()`**：前者是例外處理的語法區塊；後者是 `Object` 的 protected 方法，GC 前由 JVM 呼叫（已過時）。
- 例外處理完後，Exception 物件會在之後的 GC 被回收。

!!! warning "補充（常考陷阱）"

    `finally` 不執行的情況：`System.exit()`、JVM 當掉、執行緒被殺。若 `try` 裡有 `return`，`finally` 仍會在 return 之前執行；**finally 裡的 return 會覆蓋 try 的 return**（不建議這樣寫）。Java 7 起優先用 **try-with-resources**。
