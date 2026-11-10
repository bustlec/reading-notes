# 7. Swing / AWT（Q57–71）— 少考

| 題目 | 答案要點 |
|---|---|
| Choice vs. List | Choice 是下拉選單、只能單選；List 同時顯示多項、可多選 |
| 版面管理器（Layout Manager） | 在容器中排列元件，不綁絕對位置，跨平台一致 |
| BorderLayout | 依東、西、南、北、中五個區域擺放 |
| GridBagLayout | 網格排版，元件可跨多行多列，行列大小可不同 |
| Scrollbar vs. JScrollPane | Scrollbar 是元件；ScrollPane 是容器，自己處理捲動事件 |
| Window vs. Frame | Frame 繼承 Window，是可以有選單列的主視窗 |
| MenuItem vs. CheckboxMenuItem | 後者繼承前者，多了可勾選狀態 |
| 裁剪（Clipping） | 把繪圖限制在指定區域；重繪時裁剪區設為需要重繪的區域 |
| 事件監聽器 vs. 事件轉接器 | 轉接器（Adapter）為監聽器介面提供空的預設實作，只覆寫需要的方法 |
| Swing 執行緒安全的方法 | `repaint()`、`revalidate()`、`invalidate()` |
| **Swing 用的設計模式** | **MVC（Model-View-Controller）** ← 這題最值得記 |
