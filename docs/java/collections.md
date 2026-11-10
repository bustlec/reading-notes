# 3. 集合框架（Q18–34）

## 核心介面
- `Collection`：一組物件的根介面
  - `List`：有序、可重複
  - `Set`：不可重複
  - `Queue`：佇列
- `Map`：key → value 對應，key 不可重複（**不繼承 Collection**）

**為何集合介面不實作 `Cloneable`／`Serializable`？** 複製與序列化的語意跟具體實作相關，應由實作類別（如 `ArrayList`）自己決定。

## Iterator 相關

| | Iterator | ListIterator | Enumeration |
|---|---|---|---|
| 適用 | Set、List 等所有 Collection | 只有 List | Vector、Hashtable 等舊類別 |
| 方向 | 只能往前 | 前後皆可 | 只能往前 |
| 修改 | 可 `remove()` | 可 add / set / remove，可取得前後索引 | 不可刪除 |

!!! warning "補充"

    原文說「Iterator 比 Enumeration 安全，因為其他執行緒不能修改集合」不精確。Iterator 並不阻止修改，而是**偵測**到修改時丟例外（fail-fast）。

## fail-fast vs. fail-safe
- **fail-fast**（`java.util` 大多數集合）：遍歷時集合被結構性修改（`modCount` 改變）就丟 `ConcurrentModificationException`。
- **fail-safe**（`java.util.concurrent`）：在副本或弱一致的視圖上遍歷，不丟這個例外。
  - `CopyOnWriteArrayList`：遍歷快照副本。
  - `ConcurrentHashMap`：**弱一致性**迭代器（不是複製）。
- 遍歷中要刪除元素，請用 `iterator.remove()` 或 `removeIf()`。

## HashMap 原理（★ 最高頻）
- 以 key-value 儲存；`put()` 時用 key 的 `hashCode()` 算出索引（桶位），碰撞時用 `equals()` 比對；key 已存在就覆蓋 value。
- 重要參數：容量（capacity）、負載因子（load factor）、擴容門檻（threshold = capacity × load factor）。

!!! warning "補充（原文太淺，面試要能講到這層）"

    - 預設容量 **16**，負載因子 **0.75**；容量永遠是 2 的次方，索引 = `(n-1) & hash`。
    - hash 擾動：`h ^ (h >>> 16)`，讓高位也參與運算。
    - **Java 8**：陣列 + 鏈結串列 + **紅黑樹**。鏈長 ≥ 8 且陣列長度 ≥ 64 時轉成紅黑樹，≤ 6 時退回鏈結串列。
    - 擴容為 2 倍；Java 8 改成**尾插法**（Java 7 頭插法在並行擴容時可能形成環狀鏈結、造成死迴圈）。
    - HashMap **非執行緒安全**，並行環境請用 `ConcurrentHashMap`。

## hashCode() 與 equals()
- HashMap 依靠這兩個方法定位與比對 key，也用來判斷重複元素。
- **規則**：`equals()` 相等 → `hashCode()` 必須相等；反之不一定。**覆寫 `equals()` 就必須覆寫 `hashCode()`**，否則 HashMap／HashSet 會出錯。

## HashMap vs. Hashtable

| | HashMap | Hashtable |
|---|---|---|
| null | key、value 都允許 null | 都不允許 |
| 執行緒安全 | 否 | 是（方法全 `synchronized`） |
| 遍歷 | Iterator（fail-fast） | Enumeration |
| 地位 | 主流 | 遺留類別 |

!!! warning "補充"

    原文說「Hashtable 適合多執行緒」已過時。多執行緒請用 **`ConcurrentHashMap`**：Java 7 用分段鎖，Java 8 改成 CAS + `synchronized` 鎖單一桶位，效能遠勝 Hashtable。

## Array vs. ArrayList
- Array：可裝基本型別與物件，**長度固定**。
- ArrayList：只能裝物件（基本型別要自動裝箱，較慢），**長度動態**，方法多（`addAll`、`removeAll`、`iterator`…）。
- 長度固定且事先知道時，用 Array。

## ArrayList vs. LinkedList

| | ArrayList | LinkedList |
|---|---|---|
| 底層 | 動態陣列 | 雙向鏈結串列 |
| 隨機存取 `get(i)` | O(1) | O(n) |
| 中間插入／刪除 | O(n)（要搬移元素） | 找到位置後 O(1) |
| 記憶體 | 較省 | 較耗（每個節點多存前後兩個參考） |

!!! warning "補充"

    「LinkedList 插入刪除比較快」的前提是**已經拿到節點位置**，找位置本身是 O(n)。加上 CPU 快取友善度，實務上 ArrayList 幾乎總是較好的預設選擇。ArrayList 擴容為 **1.5 倍**。

## Comparable vs. Comparator

| | Comparable | Comparator |
|---|---|---|
| 套件 | `java.lang` | `java.util` |
| 方法 | `compareTo(T o)` | `compare(T o1, T o2)` |
| 實作位置 | 類別本身（自然排序） | 外部獨立的比較器（自訂排序，可有多種） |
| 回傳值 | 負／0／正 = 小於／等於／大於 | 同左 |

## PriorityQueue
- 基於**優先權堆積（heap）**的無界佇列，預設依自然順序，也可傳入 Comparator。
- 不允許 `null`、非執行緒安全；入隊／出隊 **O(log n)**（`peek` 為 O(1)）。

## 時間複雜度與資料結構選擇
- 大 O 描述資料量增加時，最差情況下的效能。
- **有序陣列**：查找 O(log n)（二分搜尋），插入 O(n)。
- **無序陣列**：查找 O(n)，插入 O(1)。
- **HashSet**（雜湊表，無序）：add / remove / contains 為 O(1)。
- **TreeSet**（紅黑樹，有序）：上述操作為 O(log n)。

## 集合最佳實踐
1. 依需求選對集合（固定大小用 Array）。
2. 能估計數量時，**設定初始容量**，避免反覆擴容與 rehash。
3. **一律使用泛型**：型別安全，也避免執行期的 `ClassCastException`。
4. 用 JDK 的**不可變類別**（如 `String`、`Integer`）當 Map 的 key。
5. **針對介面寫程式**（`List<X> list = new ArrayList<>()`）。
6. 沒有資料時**回傳空集合**，不要回傳 `null`。
