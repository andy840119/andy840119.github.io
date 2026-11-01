# 步驟一: 初始化文章

使用者說「我想要寫 xxx」、「準備寫 xxx 的開箱文」時執行。這一步只做準備，不要寫內文。

## 步驟

1. 判斷文章類型(開箱 / 心得分享 / 技術分享，見 [style.md](style.md) 的「文章類型」)，判斷不出來就問。
2. 取一個英文 slug，小寫、用 `-` 連接，參考既有命名，例如 `unboxing-openswim-pro-s710`、`ikea-standing-desk-unboxing`、`how-to-use-mandarake-oversea`。
3. 開 branch:
   - 先確認工作目錄有沒有未 commit 的修改，有的話告訴使用者。
   - 從最新的 `master` 開新 branch，branch 名稱用 slug。
4. 建立資料夾 `content/posts/<YYYY>/<MM>/<slug>/`，年月用今天的日期，裡面建一個空的 `res/` 資料夾放圖片。
5. 建立 `index.md` 骨架:
   - front matter 填好 title、date(今天)、tags，不要加 `draft: true`
   - 依文章類型放好 `##` 章節標題
   - 每個章節底下用 `<!-- TODO: ... -->` 提示這段要寫什麼、可以放哪些照片
6. 搜尋 `content/posts/` 有沒有舊文章提過這個主題(例如年度購物清單裡寫了「之後會寫開箱文」)。有的話把相關內容摘要成 TODO 放進對應章節，並在前言附上舊文章的相對連結。記下這篇舊文章，發佈時要回頭補連結(見 [4-publish.md](4-publish.md))。
7. 先不要 commit。

## 回報

- branch 名稱、資料夾路徑
- 找到的相關舊文章
- 下一步: 把圖片放進 `res/`，在 `index.md` 裡貼上圖片、寫下補充，然後請 AI 進行步驟二
