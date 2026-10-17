# [Hugo自架站-5]安裝 Google Analytics 追蹤網頁的流量與成效

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202501/hugo5-install-google-analytics-to-track-website-traffic/
- 發布：2025-01-20
- 主題：技術教學
- 系列：Hugo 自架站
- 標籤：Hugo、Web、自架站、教學
- 摘要：如果想知道訪客的行為和網站的成效，就要安裝 Google Analytics（GA）。Google Analytics 是一個功能強大的免費工具，可以讓你了解訪客的來源、行為，以及網站上的哪些內容最受歡迎。如果你想持續優化網站的 SEO，GA 就像一個指南針，告訴你正確的執行方向！

[將網站部署到 GitHub Pages 後](https://deanlin.net/blog/202501/hugo4-deploy-hugo-github-pages-add-google-analytics/)，如果想知道訪客的行為和網站的成效，就要安裝 Google Analytics（GA）。

Google Analytics 是一個功能強大的免費工具，可以讓你了解訪客的來源、行為，以及網站上的哪些內容最受歡迎。

如果你想持續優化網站的 SEO，GA 就像一個指南針，告訴你正確的執行方向！

### 註冊 Google Analytics 帳號

可以用 Google 帳號登入 Google Analytics: [https://analytics.google.com/](https://analytics.google.com/)

接下來會要你填寫一些資訊，下面是參考：

- **帳戶名稱**: 隨意填
- **屬性名稱**: 可以填 GitHub 帳號

另外產業類別等其他選項，依照自己實際狀況填寫即可，到最後一步的時候，我們直接選擇「網站」來做設定。

網址的部分選擇貼上先前建立的 GitHub Pages 的網址，然後給他取一個名字就好

### 取得 Google Analytics 提供的追蹤碼

這邊我們直接點擊剛剛建立的資源。

然後選擇「查看代碼操作說明」。

點擊右側的「手動安裝」，然後將下方程式碼複製下來。

### 將 Google Analytics 追蹤碼放到 Hugo 模板

然後把畫面切到 Hugo 專案，打開「themes/liva-hugo/layouts/partials/head.html」，滑到最下方，在 `</head>` 標籤上方貼上程式。

然後將原本 google analitycs 的程式刪除。

然後打開「hugo.toml」，搜尋「Google Analitycs」，將這兩行刪掉，這樣就能移除舊版 GA 的設定了。

### 測試 Google Analytics 是否設定成功

使用 `Hugo` 指令產生新的 public 資料夾，然後將資料夾底下的檔案拖到 GitHub Pages 的 Repository，如果對步驟不熟悉，可以[先回顧上一篇文章](https://deanlin.net/blog/202501/hugo4-deploy-hugo-github-pages-add-google-analytics/)。

將版本更新推送到 GitHub 後，先前往自己部署的 GitHub Pages 的網頁刷新一下。

接著回到 GA 管理的頁面，點擊「首頁」，如果看到「已啟用資料收集功能」就大功告成了！你會在下方看到活躍的使用的人數！

有時 GA 的反應沒那麼快，可能會顯示「尚未收到資料」；你可以多重新整理頁面幾次，通常等個 5 分鐘就會收到資料了。

### 結語

完成上面的設定後，你就可以透過 GA 的報表來觀察部落格網頁的流量了～

但現在網站還無法在 Google 被搜尋到，所以[下一篇文章要分享如何設定 Google Search Console](https://deanlin.net/blog/202501/hugo6-set-up-google-search-console/)。

我是林鼎淵，如果覺得這邊文章對你有幫助，歡迎追蹤筆者的 [Facebook](https://www.facebook.com/deanlinbao) 與 [YouTube](https://www.youtube.com/@dlcorner)，我會持續分享實用的新知識 💡

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202501/hugo5-install-google-analytics-to-track-website-traffic/
