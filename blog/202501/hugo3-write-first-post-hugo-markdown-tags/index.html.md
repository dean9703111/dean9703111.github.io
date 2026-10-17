# [Hugo自架站-3]用 Markdown 撰寫文章，掌握分類、標籤文章的技巧

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202501/hugo3-write-first-post-hugo-markdown-tags/
- 發布：2025-01-18
- 主題：技術教學
- 系列：Hugo 自架站
- 標籤：Hugo、Web、自架站、教學
- 摘要：這篇文章會先簡述常見的 Markdown 語法，讓大家了解如何在文章設定大標題、小標題、引述、圖片...等資訊。然後示範為文章加上「分類（category）、標籤（tag）」，讓我們能輕鬆管理自己部落格的文章。最後分享筆者推薦的檔案命名規則，以及文章資料夾結構。

[完成 Hugo 外觀的基礎建設後](https://deanlin.net/blog/202501/hugo2-setup-hugo-site-parameters/)，接下來就可以開始撰寫部落格文章了。

這篇文章會先簡述常見的 Markdown 語法，讓大家了解如何在文章設定大標題、小標題、引述、圖片…等資訊。

然後示範為文章加上「分類（category）、標籤（tag）」，讓我們能輕鬆管理自己部落格的文章。

最後分享筆者推薦的檔案命名規則，以及文章資料夾結構。

### 使用 Markdown 語法來撰寫文章

過去我們使用 Word，會需要使用上面的工具列來設定文章的標題、列表、粗體、斜體。

而 Markdown 則是用「符號」來設定，範例如下：

```
「#」開頭的就是標題，數量越少，字體越大，層級越高

# 大標題 h1

#### 小標題 h4

這邊就是描述的內容，你可以使用「-」的符號層級:

- 文字的格式
  - 要增加子階層，多兩個空格就可以
  - **粗體**
  - *斜體*
  - ~刪節線~

> 引用的寫法

```

假使想顯示單行程式，可以用 1 個反引號「`」包起來。

`console.log('單行程式');`

如果想在某個區塊展示程式或是文字，可以將他們用 3 個連續的反引號包起來「```」

```
// 可以放程式語言、文字
console.log('程式碼區塊');

```

用 `[超連結](網址)` 的結構，就能在文字插入超連結，比如放上筆者的 [Facebook](https://www.facebook.com/deanlinbao) 連結。

用 `![img](/images/path)` 的結構，則可插入圖片。

讀者可以到「content/blog」底下新增一個「test.md」的文章，測試不同符號對排版的影響。

### 掌握分類、標籤文章的技巧

從「content/blog」底下隨便點開一篇文章，你會發現最上方有些參數可以設定，下面向大家解釋每個參數代表的意義。

```
---
title: "文章標題"

# 文章撰寫的時間（新的文章會優先顯示）
date: 2025-01-16T22:07:47+08:00

# 文章是否為草稿，若設定為 true 就不會顯示在頁面上
draft: false

# 部落格文章封面縮圖
image: "images/post/post-3.jpg"

# 文章的簡短介紹
description: "這是分享文章連結時，會顯示的簡短描述"

# 文章的分類
categories:
  - "Hugo 架站教學" # 方便使用者在右側篩選（若是新增的分類，就會需要重啟 Hugo 才會生效）
tags:
  - "Hugo" # 這裡的標籤，會在首頁呈現（若是新增的標籤，就會需要重啟 Hugo 才會生效）
  - "Web"
  - "自架站"
  - "教學"

# post type
type: "post" # 原則上維持不變，因為目前都是要貼部落格文章
---

```

### 設定 shortscodes 插入 Youtube 影片、IG 貼文

如果想在自己的部落格插入其他社群媒體的元素，可以使用 shortscodes 的方案。

下面是[插入 Youtube 影片](https://gohugo.io/shortcodes/youtube/)，以及[插入 IG 貼文](https://gohugo.io/shortcodes/instagram/)的範例。

- Youtube 原始 URL: `https://www.youtube.com/watch?v=yVfafM70bzY`

[YouTube 影片：YouTube 影片](https://www.youtube.com/watch?v=yVfafM70bzY)

- IG 原始 URL: `https://www.instagram.com/p/Cu9zfv0vMoR/`

      [

            View this post on Instagram

](https://www.instagram.com/p/Cu9zfv0vMoR)

### 用 ChatGPT 生成建議的英文檔名，使用「年月」作為資料夾結構

Hugo 會使用 Markdown 文件的檔名作為網址，如果使用中文命名，那網址就會變得很長。

比如複製這段網址:「http://localhost:1313/blog/中文的網址」，貼上時就會因為編碼變成一長串無法辨識的字「http://localhost:1313/blog/%E4%B8%AD%E6%96%87%E7%9A%84%E7%B6%B2%E5%9D%80/」

如果不知道這篇文章的英文檔名如何取，可以用下面的 Prompt，請 ChatGPT 幫你一把。

```
幫我將下面的文章標題轉為簡短易懂的英文檔名，單字間使用「-」作為區隔:
1. 挑選合適的主題，在本地安裝並啟動 Hugo
2. 設定 Hugo 部落格網站的基礎參數（自我介紹、圖片、網站 Logo、Email、社群媒體...）
3. 在 Hugo 用 Markdown 撰寫第一篇文章，掌握分類、標籤文章的技巧

```

另外 `content` 底下的文章，我建議使用「年月」的方式作為資料夾，比如「202501」。因為文章分類的部分，在「categories、tags」已經有設計了，用年月的結構，反而更容易檢視自己發文的頻率。

另外放置部落格圖片的 `assets/images` 也是一樣的道理，建議使用「年月/文章檔名」的結構來做存放，這樣更方便管理。

**重點：網址的結構在一開始就要決定好，因為網址就像是身分證；一但調整，過去累積的 SEO 就會歸零。**

### 結語

這邊先恭喜大家完成部落格網站的雛形，下一篇文章會帶大家[將網站部署到 GitHub Pages](https://deanlin.net/blog/202501/hugo4-deploy-hugo-github-pages-add-google-analytics/)，完成這個步驟後，就可以把漂亮的網站分享給朋友了！

我是林鼎淵，如果覺得這邊文章對你有幫助，歡迎追蹤筆者的 [Facebook](https://www.facebook.com/deanlinbao) 與 [YouTube](https://www.youtube.com/@dlcorner)，我會持續分享實用的新知識 💡

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202501/hugo3-write-first-post-hugo-markdown-tags/
