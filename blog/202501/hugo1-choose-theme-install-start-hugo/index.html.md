# [Hugo自架站-1]挑選合適的主題，在本地安裝並啟動 Hugo

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202501/hugo1-choose-theme-install-start-hugo/
- 發布：2025-01-16
- 主題：技術教學
- 系列：Hugo 自架站
- 標籤：Hugo、Web、自架站、教學
- 摘要：如果你跟筆者一樣選擇用靜態網站來經營個人品牌，強烈建議收藏這系列用 Hugo 架站的文章。我會從挑選主題、安裝 Hugo 開始，一步步帶你客製化網頁參數，最終將網站部署到 GitHub Pages，並加上 Google Analitycs 來追蹤流量！

如果你跟筆者一樣選擇用靜態網站來經營個人品牌，強烈建議收藏這系列用 Hugo 架站的文章。我會從挑選主題、安裝 Hugo 開始，一步步帶你客製化網頁參數，最終將網站部署到 GitHub Pages，並加上 Google Analitycs 來追蹤流量！

### 什麼是 GitHub Pages？

[GitHub](https://github.com/) 是一個程式碼版本控制的平台，而 GitHub Pages 則是它的一個副產物。

如果你上傳一個副檔名為「.html」的檔案，GitHub Pages 就會把它轉換成漂亮的網頁，並且這個網頁是可以透過 Google 搜尋找到的。

如果你是程式小白也不用擔心，因為筆者接下來會帶大家會「Hugo」這個工具，來產生個人品牌網站的 html 程式碼。

### 什麼是 Hugo？

「The world’s fastest framework for building websites」

進入 [Hugo 官網](https://gohugo.io/)後，你會看到這段宣傳標語，它號稱自己是**全世界最快的個人網站架設工具**。

### 挑選自己喜歡的主題（Theme）

Hugo 有提供豐富的主題供大家選擇，大家只要點擊「Themes」便可瀏覽各主題的預覽圖。

因為筆者的目標是建立個人品牌的部落格，所以選擇旁邊「Blog」的分類，然後就可以像是逛街般挑選自己喜歡的主題了。

看到心儀的主題後，在下載（Download）前，你可以先往下滑。

部分的主題有提供 Demo，可以觀看[這個網站](https://demo.stack.jimmycai.com/)具體呈現的效果如何。

因為對個人品牌來說，「文章」是最大重點，所以可以多關注「封面、文字、圖片、程式碼、目錄、標題」等資訊呈現的方式。

因為挑來挑去沒找到合適的，所以筆者選擇了跟[古古課程](https://hahow.in/courses/5de8fec16117240026540b9c)相同的主題（[liva](https://github.com/gethugothemes/liva-hugo)）。

### 安裝 Hugo，從零開始建立自己的個人網站

下面的操作步驟為 [Mac 環境](https://gohugo.io/installation/macos/)，使用 Windows 的朋友請參考[官方文件的操作步驟](https://gohugo.io/installation/windows/)

**STEP 1**：打開終端機（Terminal）。

**STEP 2**：安裝 [Homebrew](https://brew.sh/)

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

```

**STEP 3**：用 Homebrew 安裝 Hugo

```
brew install hugo

```

安裝完成後，輸入 `hugo version` 確認自己有順利安裝

**STEP 4**：安裝 Hugo 的 liva 主題

```
git clone https://github.com/gethugothemes/liva-hugo.git

```

**STEP 5**：調整資料夾結構，並建立放置主題的 themes 資料夾

```
mv liva-hugo/exampleSite brandSite
mkdir brandSite/themes
mv liva-hugo brandSite/themes

```

### 啟動 Hugo

執行下面指令就可啟動 Hugo

```
cd brandSite
hugo server

```

進入 http://localhost:1313/liva/examplesite/ ，如果看到下面的畫面，就恭喜你成功嚕（想關閉用 Ctrl + C 即可）！

### 排除 Hugo 啟動時的錯誤

但如果讀者遇到這段錯誤也不要害怕：「Error: command error: failed to load modules: failed to download modules: binary with name “go” not found in PATH」，安裝 Go 即可。

```
brew install go

```

### 結語

做到這邊就完成 Hugo 的初始化了，但現在網頁的內容都是主題預設的；筆者會在[下一篇文章分享設定部落格網站的基礎參數（自我介紹、圖片、網站 Logo、Email、社群媒體…）的技巧](https://deanlin.net/blog/202501/hugo2-setup-hugo-site-parameters/)。

在文章的最後，要特別感謝[古古的後端筆記](https://www.facebook.com/kucw.io)，許多操作步驟與設計邏輯，筆者都是參考她在 Hahow 開設的「[Github 免費架站術！輕鬆打造個人品牌](https://hahow.in/courses/5de8fec16117240026540b9c)」這堂課，如果想知道更詳細的操作細節，歡迎到 Hahow 上支持選購。

我是林鼎淵，如果覺得這邊文章對你有幫助，歡迎追蹤筆者的 [Facebook](https://www.facebook.com/deanlinbao) 與 [YouTube](https://www.youtube.com/@dlcorner)，我會持續分享實用的新知識 💡

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202501/hugo1-choose-theme-install-start-hugo/
