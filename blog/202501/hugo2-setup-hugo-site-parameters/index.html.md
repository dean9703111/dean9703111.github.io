# [Hugo自架站-2]設定部落格網站的基礎參數（自我介紹、圖片、網站 Logo、Email、社群媒體...）

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202501/hugo2-setup-hugo-site-parameters/
- 發布：2025-01-17
- 主題：技術教學
- 系列：Hugo 自架站
- 標籤：Hugo、Web、自架站、教學
- 摘要：這篇文章將帶你完成 Hugo 網站的基礎設定，包括安裝 VSCode 外掛、調整網站參數、自訂 Logo 與菜單 Menu 等實用技巧，帶你一步步打造更符合個人風格的網站！

如果還沒安裝好 Hugo 的朋友，可以參考上一篇文章「[挑選合適的主題，在本地安裝並啟動 Hugo](https://deanlin.net/blog/202501/hugo1-choose-theme-install-start-hugo/)」

這篇文章將帶你完成 Hugo 網站的基礎設定，包括安裝 VSCode 外掛、調整網站參數、自訂 Logo 與菜單 Menu 等實用技巧，帶你一步步打造更符合個人風格的網站！

### 安裝 VSCode 外掛（Extension）

Hugo 的基礎設定，大多都在附檔名為「.toml」的設定檔中調整。但預設的字體顏色都一樣，這會增加閱讀與編輯的難度。

建議讀者可以先去 VSCode 的「Extension」搜尋「Toml」，然後找一個使用量最高的外掛來安裝。

安裝完畢後，附檔名為「.toml」檔案的都有分色了。

### 調整網頁的網址、名稱

打開「config/_default/hugo.toml」，將下面的參數調整成自己的：

- **baseURL**: 放上自己 Github 的網址，ex: [https://xxx.github.io](https://xxx.github.io)
- **languageCode**: 原本是英文，我調整為中文（zh-tw）
- **title**: 調整為自己的網頁名稱，ex: 林鼎淵

調整完後重啟 Hugo，因為在 Local 開發，因此進入的網址會變成「http://localhost:1313」，且網頁上面名稱改變。

### 將 Logo、作者名稱、Email 調整成自己的

打開「hugo.toml」，搜尋「default parameters」，將下面的參數調整成自己的：

- **logo**: Hugo 圖片默認放在「assets」資料夾下，到「assets/images/logo.png」，將 logo.png 替換成自己的。
- **author**: 填上自己的名字，ex: 林鼎淵
- **description**: 這是網址時，網頁預覽會出現的文字描述，你可以簡述這個部落格會分享的內容
- **email**: 放上自己的聯絡資訊

另外我這邊將「mobile、location」註解了。

接著搜尋「social site」的區塊，你可以在「link」放上想要導流的社群媒體連結，而不需要的直接移除就好（我僅保留 Facebook、GitHub、Linkedin）。

調整完後，點擊「Contact」可以看到剛剛的調整。

### 關閉 Newsletter 的功能

Newsletter 是提供使用者訂閱的功能，而這個功能需要與資料庫串接，而靜態網站並沒有資料庫，所以我們要關閉這個功能。

打開「hugo.toml」，搜尋「news letter」，把 enable 變成 false

回到剛剛的頁面，就會發現「Newsletter」的區塊移除了。

### 調整語言資料夾

打開「config/_default/languages.toml」，你會發現 Hugo 預設有多國語言的設計，一開始默認文章放在「content/english」底下。

你可以沿用這個多國語言的設計，但筆者估計自己沒有這麼多的心力，所以把 contentDir 的「english」這層直接移除了；並將 languageCode 調整為「zh-tw」，設定如下：

```
# --------------------------
# English language
# --------------------------
[en]
languageName = "En"
languageCode = "zh-tw"
contentDir = "content"
weight = 1

```

### 調整自我介紹的內容（About Me）

因為我們上一步將「english」這層資料夾移除，所以要把下面的資料往「content」移，這樣相對路徑才能順利抓到檔案。

接著打開「content/about/_index.md」，調整下面的設定：

- **image**: 到「assets/images/author.png」，將 author.png 替換成自己的。
- **description**: 這是網頁預覽時會出現的文字描述。
- 把最下方的文字調整成自己的自我介紹。

調整完後，點擊「About」可以看到剛剛的調整。

### 調整 or 隱藏自我介紹下方的 My Gallery 相簿集

自我介紹下的「My Gallery」設定路徑在「data/gallery.yml」，你可以將裡面的照片調成為自己的宣傳照，或是將 enable 改為 false 來隱藏這個區塊。

### 調整、移除不需要的菜單（Menu）

打開「config/_default/menus.en.toml」後，會看到設定會分成 main 跟 footer 兩塊：

- **name**: 網頁顯示的名稱
- **URL**: 網頁的路徑
- **weight**: 排序權重（從左到右）

這邊我就將 main menu 調整如下（移除 contact、name 改為中文、weight 調整）:

```
# main menu
[[main]]
name = "關於我"
URL = "about/"
weight = 2

[[main]]
name = "文章"
URL = "blog/"
weight = 1

```

### 調整網頁的 Logo

如果想使用自己的 Logo，可以輸入以下指令建立對應的靜態文件資料夾：

```
mkdir static/images

```

然後在裡面放上自己的「favicon.png」就好了，**Logo 僅能用「.png」的檔案喔**！

如果發現刷新沒有反應，可能是因為 Cache 問題，[安裝 Chrome 外掛 Clear Cache 能有效解決](https://chromewebstore.google.com/detail/clear-cache/cppjkneekbjaeellbfkmgnhonkkjfpdn?hl=zh-TW&sid=h32QM2)。

### 結語

成功將網站資訊調整為自己的後，下一步就要開始準備發表文章了。

Hugo 是使用 Markdown 語法來撰寫文章。下一篇文章將跟大家分享如何[用 Markdown 撰寫文章，並掌握分類、標籤文章的技巧](https://deanlin.net/blog/202501/hugo3-write-first-post-hugo-markdown-tags/)。

我是林鼎淵，如果覺得這邊文章對你有幫助，歡迎追蹤筆者的 [Facebook](https://www.facebook.com/deanlinbao) 與 [YouTube](https://www.youtube.com/@dlcorner)，我會持續分享實用的新知識 💡

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202501/hugo2-setup-hugo-site-parameters/
