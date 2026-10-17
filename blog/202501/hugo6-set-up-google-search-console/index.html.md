# [Hugo自架站-6]設定 Google Search Console，讓自己的網站可以被搜尋

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202501/hugo6-set-up-google-search-console/
- 發布：2025-01-21
- 主題：技術教學
- 系列：Hugo 自架站
- 標籤：Hugo、Web、自架站、教學
- 摘要：這篇文章要教大家如何設定 Google Search Console，以及上傳網站的 Sitemap（網站地圖），讓自己的網站可以被搜尋。

上篇文章分享了如何[在網站安裝 Google Analytics（GA）來分析網站的瀏覽數據](https://deanlin.net/blog/202501/hugo5-install-google-analytics-to-track-website-traffic/)，而這篇文章要教大家如何設定 Google Search Console，以及上傳網站的 Sitemap（網站地圖），讓自己的網站可以被搜尋。

### 驗證自己的網站是否能被 Google 搜尋

在 Google Search 的搜尋框輸入 `site:你的網址`，ex: site:dean9703111.github.io

如果搜尋結果為「請使用 Google Search Console」，就代表你的網站尚未被 Google 收錄，別人也無法從 Google 找到你的網站。

### 設定 Google Search Console

點擊「請使用 Google Search Console」便會進入如下頁面：

因為目前的網域是使用 GitHub 的，所以要選擇「網址前置字元」，並貼上自己 GitHub 的網址。

如果你有跟著[上一篇文章乖乖設定 GA](https://deanlin.net/blog/202501/hugo5-install-google-analytics-to-track-website-traffic/)，就會直接驗證通過。

### 上傳 Sitemap 到 Google Search Console

點擊「前往資源」後，選擇左側的「Sitemap」，直接貼上 `sitemap.xml` 點擊提交就好了！

Hugo 專案的 Sitemap（網站地圖）通常是放在「public/sitemap.xml」這個位置，裡面會存放網站所有可點擊的路徑。

剛提交的 sitemap 可能會看到如下的畫面，狀態顯示「無法擷取」。

關於這個問題，Google 官方有給了一些[解決方案](https://support.google.com/webmasters/answer/7451001?hl=zh-Hant)，網路上也看到有遇到相同問題的人。但如果你的 Sitemap 結構沒有擅自更改，通常是 Google 的奇怪 Bug，可能等一段時間什麼事都沒做就自動恢復了。

但因為筆者有購買自己的網域，所以沒有等他到的狀態轉為成功；由於 SEO 是跟著網址走的，所以這邊是「強烈」建議購買一個自己的網域，相對來說比較不會遇到這些奇怪的狀況（**下圖是筆者換成自己的網域，試一次就成功了**）。

### 驗證能否在 Google 搜尋到自己的網站

在 Google Search 的搜尋框輸入 `site:你的網址`，如果搜尋得到，就代表你設定成功了！

把 Sitemap 提交後，可能要等一段時間文章才會被 Google 收錄；你可以稍微等等，又或是手動把網址輸入到 Google Search Console 上，並要求他「建立索引」（檢索是有可能發生錯誤的，可以多提交幾次嘗試看看）。

### 結語

以上就是 Hugo 架站的完整教學，做到這邊就完成一個 60 分的網站了。

之所以會說是 60 分，是因為還有許多可以優化的地方，下面舉幾個案例：

- **網站外型優化**：目前套用的是預設模板，可能有些設計跟你的期待不符，但這塊優化就需要一些網頁前端（Frontend）的基礎能力了，ChatGPT 是你成長的好夥伴。
- **Open Graph**：如果你把部落格文章分享給朋友，會發現連結沒有縮圖；我們需要增加 OG 標籤，才能讓網站在社群上被漂亮分享。
- **canonical URL**：接下來筆者打算將 Medium 文章搬到自己的部落格，為了讓 Google 了解相同內容的狀況下哪篇文章才是本尊，就需要設定這個參數，否則你部落格的流量可能會被其他網站搶走。

接下來會找時間繼續分享這些細節，不想錯過的朋友可以先追蹤[筆者的 FB](https://www.facebook.com/deanlinbao) 喔！

我是林鼎淵，如果覺得這邊文章對你有幫助，歡迎追蹤筆者的 [Facebook](https://www.facebook.com/deanlinbao) 與 [YouTube](https://www.youtube.com/@dlcorner)，我會持續分享實用的新知識 💡

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202501/hugo6-set-up-google-search-console/
