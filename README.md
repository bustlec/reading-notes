# 學習筆記

讀書心得與技術筆記的重點整理。

**網站：** https://bustlec.github.io/reading-notes/

## 內容

### 讀書筆記（`docs/books/`）

| 書名 | 分類 | 筆記 |
|---|---|---|
| 《心流：高手都在研究的最優體驗心理學》 | 心理與成長 | [flow.md](docs/books/flow.md) |
| 《反脆弱：脆弱的反義詞不是堅強，是反脆弱》 | 思維與決策 | [antifragile.md](docs/books/antifragile.md) |
| 《執行長日記：關於事業與人生的33條法則》 | 商業與職涯 | [diary-of-a-ceo.md](docs/books/diary-of-a-ceo.md) |

### Java 面試（`docs/java/`）

100 IT 名企 Java 面試真題重點，依主題分成 12 頁，加上[最高頻 15 題](docs/java/top-15.md)速記。題庫停在 Java 6/7 時代，各頁的「補充」「更正」提示框標出了跟現代 Java（8～21+）不一樣的地方。

## 專案結構

```
docs/
├── index.md                  網站首頁（兩個分類的入口）
├── books/
│   ├── index.md              書單
│   ├── <slug>.md             各書筆記，一本一頁
│   └── assets/               封面圖（<slug>-cover.jpg）
└── java/
    ├── index.md              Java 首頁（主題總表）
    ├── top-15.md             最高頻 15 題
    └── <topic>.md            各主題（oop、basics、threads、collections…）
mkdocs.yml                    網站設定與導覽（nav）：上方分頁、左側目錄
requirements.txt              建置套件：mkdocs-material、jieba（中文搜尋斷詞）
.github/workflows/deploy.yml  push 到 main 後自動建置並發佈到 GitHub Pages
```

網站使用 [MkDocs Material](https://squidfunk.github.io/mkdocs-material/) 建置。

## 新增一本書

1. 把筆記放到 `docs/books/<slug>.md`，封面圖放到 `docs/books/assets/<slug>-cover.jpg`。`<slug>` 用英文小寫加 `-`，例如 `atomic-habits`。
2. 在 `mkdocs.yml` 的 `nav` → `讀書筆記` 底下，加到對應分類；沒有適合的分類就新增一個。
3. 在 `docs/books/index.md` 的書單表格加一列，並更新本 README 的書單。
4. push 到 `main`，GitHub Actions 會在一兩分鐘內自動更新網站。

要新增其他技術主題時，比照 `docs/java/` 開一個新資料夾，並在 `nav` 加一個新的分頁。

筆記裡的提示框寫法：

```markdown
!!! abstract "一句話總結"

    內容要縮排 4 個空白。
```

常用類型：`abstract`（摘要）、`info`（說明）、`tip`（提示）、`warning`（補充、更正）。

## 本機預覽

```bash
pip install -r requirements.txt
mkdocs serve
```

然後打開 http://127.0.0.1:8000 。發佈前可以先執行 `mkdocs build --strict`，有連結失效就會直接報錯。
