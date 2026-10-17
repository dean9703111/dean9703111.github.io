# [Hugo自架站-4]將網站部署到 GitHub Pages

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202501/hugo4-deploy-hugo-github-pages-add-google-analytics/
- 發布：2025-01-19
- 主題：技術教學
- 系列：Hugo 自架站
- 標籤：Hugo、Web、自架站、教學
- 摘要：設定好網站的基礎資訊，以及準備好第一篇部落格文章後。只要跟著教學操作，就可以將 Hugo 網站部署到 GitHub Pages 了！

設定好網站的基礎資訊，以及準備好第一篇部落格文章後，跟著教學的步驟操作，就可以將網站部署到 GitHub Pages 了！

### 在 GitHub 新增一個自己名字的 Repository

登入 [GitHub](https://github.com/) 後，用 `自己的帳號名稱.github.io` 來新增一個 Repository。

以筆者來說，就是用 `dean9703111.github.io`，這將會是你日後個人品牌的部落格網址；另外記得要選擇「Public」才能讓大家看得到。

> 小提醒：如果你帳號建立時跟筆者一樣沒想太多，導致字數超長，這在日後也是可以更新成自己的網域喔！

### 把 Hugo Public 下的資料夾搬到剛剛建立的 Repository 下

先前我們都是用 `hugo server` 來啟動網站，現在只需要執行 `hugo` 即可。

然後直接將生成的「public」資料夾下的內容搬到剛剛建立的 Repository 下即可。

### 將變更 Push 到 GitHub 上面

接下來可以使用任何你習慣的版控工具來將變更推到 GitHub 上面，下面筆者是用 VSCode 來輸入 commit（描述調整），以及做 `git push`。

### 到 GitHub 確認確認部署狀況

回到 GitHub 網頁，如果右下角的「Depoyments」是綠色勾勾，基本上就沒問題了！

接者瀏覽網址（這是筆者的）: [https://dean9703111.github.io](https://dean9703111.github.io)

就可以順利看到自己的網站與發表的文章嚕！給自己拍拍手！現在可以將個人網站分享給其他人嚕～

### 結語

儘管看起來很美好，但其實還有如下細節需要設定:

1. [**Google Analytics**: 用來追蹤網站流量成效](https://deanlin.net/blog/202501/hugo5-install-google-analytics-to-track-website-traffic/)
2. [**Google Search Console**: 讓自己的網站在 Google 可以被搜尋到](https://deanlin.net/blog/202501/hugo6-set-up-google-search-console/)
3. **個人網域**: 購買個人網域可以讓品牌看起來更專業，且轉換平台時 SEO 不會被綁架

這些細節，筆者也會在[後續的文章](https://deanlin.net/blog/202501/hugo5-install-google-analytics-to-track-website-traffic/)與大家分享。

> 我是林鼎淵，如果覺得這邊文章對你有幫助，歡迎追蹤筆者的 [Facebook](https://www.facebook.com/deanlinbao) 與 [YouTube](https://www.youtube.com/@dlcorner)，我會持續分享實用的新知識 💡

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202501/hugo4-deploy-hugo-github-pages-add-google-analytics/
