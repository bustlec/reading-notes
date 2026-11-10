# 讀書筆記

每本書整理成一頁重點筆記：全書架構、各章重點與實用心法。

**網站：** https://bustlec.github.io/reading-notes/

## 書單

| 書名 | 分類 | 筆記 |
|---|---|---|
| 《心流：高手都在研究的最優體驗心理學》 | 心理與成長 | [docs/flow.md](docs/flow.md) |
| 《反脆弱：脆弱的反義詞不是堅強，是反脆弱》 | 思維與決策 | [docs/antifragile.md](docs/antifragile.md) |
| 《執行長日記：關於事業與人生的33條法則》 | 商業與職涯 | [docs/diary-of-a-ceo.md](docs/diary-of-a-ceo.md) |

## 專案結構

```
docs/
├── index.md                  網站首頁（書單）
├── flow.md                   各書筆記，一本一頁
├── antifragile.md
├── diary-of-a-ceo.md
└── assets/                   封面圖（<slug>-cover.jpg）
mkdocs.yml                    網站設定與左側目錄（nav）
requirements.txt              建置套件：mkdocs-material、jieba（中文搜尋斷詞）
.github/workflows/deploy.yml  push 到 main 後自動建置並發佈到 GitHub Pages
```

網站使用 [MkDocs Material](https://squidfunk.github.io/mkdocs-material/) 建置。

## 新增一本書

1. 把筆記放到 `docs/<slug>.md`，封面圖放到 `docs/assets/<slug>-cover.jpg`。`<slug>` 用英文小寫加 `-`，例如 `atomic-habits`。
2. 在 `mkdocs.yml` 的 `nav` 加到對應分類底下；沒有適合的分類就新增一個。
3. 在 `docs/index.md` 的書單表格加一列，並更新本 README 的書單。
4. push 到 `main`，GitHub Actions 會在一兩分鐘內自動更新網站。

筆記裡的提示框寫法：

```markdown
!!! abstract "一句話總結"

    內容要縮排 4 個空白。
```

## 本機預覽

```bash
pip install -r requirements.txt
mkdocs serve
```

然後打開 http://127.0.0.1:8000 。發佈前可以先執行 `mkdocs build --strict`，有連結失效就會直接報錯。
