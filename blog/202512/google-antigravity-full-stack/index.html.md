# 【Google Antigravity 教學】把想法化為現實！用 Vibe Coding 完成可部署上線的輕量級全端專案

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202512/google-antigravity-full-stack/
- 發布：2025-12-08
- 主題：AI 實戰
- 系列：Google Antigravity
- 標籤：Antigravity、Google Antigravity、Vibe Coding、AI、Engineering
- 摘要：如果市場上找不到工具，那就自己動手實現！

今天要跟大家分享 Google 最新推出的 Antigravity 這款 AI 程式碼編輯器。

不管你電腦的作業系統是 Windows、Mac，還是 Linux，都可以安裝體驗。

這隻文章我會透過 Vibe Coding，手把手帶你完成一個輕量級的全端專案，透過實戰的方式，帶大家了解他有哪些特色。

如果你是剛接觸 Vibe Coding 的新手，可以了解有哪些實用技巧，能讓 AI 給你更精準的回覆，並根據你的需求進行功能調整、畫面優化。

最後告訴你，如何在確保資訊安全的前提下，把專案部署到線上環境。

完整的 Demo，可以觀看 YT 喔 👉

對 AI 應用感興趣的朋友，歡迎按下訂閱按鈕，未來我計畫分享一系列 AI 工具的應用教學。

[YouTube 影片：YouTube 影片](https://www.youtube.com/watch?v=-FW5DgQEV0M)

## 一、全端架構快速入門

以全端專案來說，通常會包含**前端、後端、資料庫** 3 個部分。

**前端是給使用者瀏覽的畫面**，我選擇了 React 這款知名框架，他有許多現成的元件素材可以讓 AI 使用。

**而後端是處理邏輯判斷的部分**，如果採用傳統的 Python、Node.js、PHP 等程式語言。

對新手來說，我認為難度太高了，就算真的把功能做出來也沒有掌控力。

所以我打算使用 Google Apps Script，這款打開瀏覽器就能使用的工具作為替代方案。

**至於資料庫，我也不使用 MySQL、Postgres、Mongo**；而是使用大家非常熟悉的 Google Sheets，打開就能看到並且編輯資料。

採用這套組合，你不需要伺服器、不用綁信用卡、不需要複雜的前置作業。

就能透過實作理解全端專案的架構，而且最後可以免費把專案放到 GitHub 公開部署。

不管你是要跟朋友炫耀，還是要直接應用到職場上都是可以的喔！

## 二、快速上手 Antigravity

如果使用過 VS Code 或是 Cusor，那他的介面你應該可以無障礙使用。

[目前 Antigravity 只要登入 Google 帳號就可以免費使用](https://antigravity.google/)，至於免費多久，就看 Google 的心情了。

這邊我先新增一個「pixel-game」的資料夾，然後把它拖曳到 Antigravity 中，這樣 AI 才知道他的工作區，在哪個資料夾底下。

接下來跟大家說明一下右側 Agent 可以做的設定。

這個 … 打開後有 3 個選項，分別是「客製化、MCP 伺服器，以及下載診斷」

### ➤ 設定 Rules，讓 AI 更懂你

建議大家可以選擇「Customizations」來設定一下自己的使用偏好，在 Rules 這個地方，我選擇 Global。

這邊我複製貼上一個 Cursor 內部員工的 Rules 範例，其實提示詞都是通用的：[https://x.com/kayladotdev/status/1853272891023872450](https://x.com/kayladotdev/status/1853272891023872450)

```
DO NOT GIVE ME HIGH LEVEL SHIT, IF I ASK FOR FIX OR EXPLANATION, I WANT ACTUAL CODE OR EXPLANATION!!! I DON'T WANT "Here's how you can blablabla"

- Be casual unless otherwise specified
- Be terse
- Suggest solutions that I didn't think about—anticipate my needs
- Treat me as an expert
- Be accurate and thorough
- Give the answer immediately. Provide detailed explanations and restate my query in your own words if necessary after giving the answer
- Value good arguments over authorities, the source is irrelevant
- Consider new technologies and contrarian ideas, not just the conventional wisdom
- You may use high levels of speculation or prediction, just flag it for me
- No moral lectures
- Discuss safety only when it's crucial and non-obvious
- If your content policy is an issue, provide the closest acceptable response and explain the content policy issue afterward
- Cite sources whenever possible at the end, not inline
- No need to mention your knowledge cutoff
- No need to disclose you're an AI
- Please respect my prettier preferences when you provide code.
- Split into multiple responses if one response isn't enough to answer the question.

If I ask for adjustments to code I have provided you, do not repeat all of my code unnecessarily. Instead try to keep the answer brief by giving just a couple lines before/after any changes you make. Multiple code blocks are ok.

```

貼上後，這個規則就會在所有的專案中生效；另外大家可以再加上一個 Rules，告訴 AI 你習慣使用的語言。

```
- Always respond in 繁體中文

```

點擊刷新後，就可以看到自己設定好的 Rules。

另外如果你想給目前的專案設定特定的 Rules，比如程式碼規則這類的，也可以點擊 Workspace 來設定喔。

### ➤ 跨模型支援

在模型這塊，Google 除了自家的 Gemini 外，還提供了 Claude 與 GPT 的模型讓大家使用。

這邊要提醒一下，每個模型使用是有額度限制的，超過後需要換一個模型使用。

### ➤ 對話模式

在對話上，有分成 Planning 跟 Fast 兩種。

如果你今天要處理的是複雜任務，比如專案初始的規劃，就建議選擇 Planning。

假使你只是想要簡單快速的得到解答，那選擇 Fast 更省時喔。

### ➤ 不要完全賦權給 AI Agent

- Terminal Command Auto Execution 不建議用 Turbo 模式（此模式僅建議在獨立容器內執行）
- 編輯 Deny List Terminal Commands 放黑名單指令

## 三、引導 AI 生成專案

可以在右側的 Agent 對話框中，貼上我們的需求：

```
# 角色
你是一位資深的 React 前端工程師與遊戲設計師。請根據需求描述協助我設計一個網頁的「闖關問答遊戲」

# UI/UX 需求
- 前端框架：React Vite
- 風格：Pixel Art 像素風，像是 2000 年代的街機，樸實但是有設計感。
- 關主圖片處理：使用 DiceBear API 預先載入 100 張不同素材
- 關卡呈現：每一關皆配有一個 Pixel 風格的「關主」圖片

# 功能需求

## 操作流程
- 首頁：使用者需輸入「ID」才能開始遊戲，此 ID 是為了記錄到 Google Sheets
- 題目來源：透過 Google Apps Script 從指定 Google Sheets 的「題目」工作表隨機撈取 N 題（不包含解答欄位）
- 成績計算：將作答結果傳送到 Google Apps Script 計算成績，並記錄到 Google Sheets

## Google Sheets 配置
- 「題目」工作表：題號、題目、A、B、C、D、解答
- 「回答」工作表：ID、闖關次數、總分、最高分、第一次通關分數（若同 ID 已通關過，後續分數不覆蓋，僅在同列增加闖關次數）、花了幾次通關、最近遊玩時間
- Google Apps Script：是直接從這份 Google Sheet 擴充功能配置的

# 環境變數設定 (.env)
請將以下參數設計為可透過環境變數配置：
- GOOGLE_APP_SCRIPT_URL：Google Apps Script 的後端連結
- PASS_THRESHOLD：通過門檻（需要答對幾題才算通過）
- QUESTION_COUNT：每次遊戲的題目數量

```

這邊說明一下我是如何發想專案的。

最近在幫學校老師與企業的人資上課時，他們提到自己時常有出考題給學生，以及對員工有培訓考核的需求。

**但市場上找不到可以免費亂數出題、自動批改、簡單操作的工具。**

了解需求後，我就想：

那不如我嘗試用 Vibe Coding 設計一個簡易的闖關遊戲？

於是今天的影片就出來了，我希望大家 Vibe Coding 時，盡可能從「實際」的需求出發；如果只是複製貼上別人的提示詞，你是學不到什麼東西的。

下面跟大家說明一下我的提示詞架構：

### ➤ 前端風格

首先，我做了角色設定，讓他扮演資深的 React 前端工程師與遊戲設計師。

另外如果你是第一次接觸 React，會需要先安裝 Node.js，詳細安裝與設定步驟可以參考上方的資訊卡，我有特別錄一集影片，分享安裝哪些工具可以讓 Vibe Coding 效率加倍。

好，除了有角色設定與框架選擇外，我還跟他說了我期待的設計風格是 Pixel 像素風。

並且關主的圖片要先下載好，每一關都要配置一個。

### ➤ 前後端邏輯設計

接下來，我則是非常明確的規範專案的「功能需求」，這塊就是講解前後端的運作邏輯。

我告訴他使用者需輸入「ID」才能開始遊戲，此 ID 是為了讓 Google Sheets 紀錄分數時知道對方是誰。

另外遊戲一開始會透過 Google Apps Script 從 Google Sheets 撈取「題目」，這邊有特別交代不要包含解答欄位。

之所以不直接從 Google Sheets 抓題目，是為了保護資料來源。

另外作答完畢後，會把答題結果傳送到 Google Apps Script 計算成績，並記錄到 Google Sheets。

### ➤ 資料庫設計

然後為了要讓 AI 明白資料如何讀取與寫入，所以我提供了非常明確的 Google Sheets 配置，減少模糊地帶。

不管題目還是回答的工作表，我都有明確列出欄位。

並且針對特殊欄位有額外的說明，像是「回答」工作表的第一次通關分數，我就備注：「若同 ID 已通關過，後續分數不覆蓋，僅在同列增加闖關次數」。

還有特別補充一點，Google Apps Script 是直接從這份 Google Sheet 擴充功能配置的，這是表達工具間的相依性。

### ➤ 環境變數設計

最後考量到專案完成後的「易用性」，所以我把「Google Apps Script 連結、通過門檻、題目數量」等設計為環境變數。

這樣日後要換考題、改變評核標準，就不用打開程式，在網頁動動手指就好了。

通常第一次的結果都不會滿意的

不滿意的地方，你可以直接跟 AI 討論（[可以參考影片](https://youtu.be/-FW5DgQEV0M)）

## 四、部署前注意事項

確認功能都符合需求後，接下來我們就要準備將專案部署到 GitHub Pages。

但部署前，我先提醒一下要注意的事項：

### ➤ 設計 .gitignore

在這裡放不要加入 Git 版控的檔案、資料夾，比如：

- 套件資料夾（node_modules）
- 程式自動生成資料夾（dist）
- log 檔
- 暫存檔（.tmp）
- 隱私檔（.nev）

### ➤ 設計 .env.example

雖然 .env 不可以上傳，但為了讓其他人知道可以如何使用這個專案，我會設計一個 .env.example 的範例檔：

```
請幫我建立一個 .env.example 的範例檔

```

### ➤ 設計 Github Actions

另外因為我們想要在 GitHub 部署，所以這邊我也請 AI 協助設計對應的工作流

```
請設計能自動部署到 Github Pages 的 Action，會使用到的環境變數請參考 .env.exmaple 檔
並將操作流程更新到 README.md 文件

```

## 五、用 GitHub Pages 免費發布網頁

先點擊左側的 Source Control，點擊 Initialize Repository 可以初始化專案。

接著點擊 Generate，AI 就會辨識我們更新的內容，來產生 commit 訊息。

確認文字描述符合自己的預期後，點擊 commit，就會在自己的電腦記錄這個變更。

但如果想更新到雲端，你需要點擊 Push 才會正式上傳喔！

如果專案尚未存在於 GitHub，這邊會有彈窗出現，讓你輸入專案名稱。

另位因為我們的專案預計要發布到 GitHub Pages，所以選擇 Public Repository，完成後，點擊 Open GitHub 去看看是否部署順利。

如果能看到專案的內容，就代表你成功上傳專案了！

完成這些操作後，我們回到 GitHub 點擊「Actions」的分頁，查看部署的狀況。

成功的話會有綠色勾勾，失敗則是紅色叉叉，會需要點進去釐清具體的錯誤原因。

一開始部署失敗很正常，你可以把錯誤訊息複製起來，直接貼到與 AI 的對話視窗中，讓他幫忙解決

如果一切順利，點擊進去就會看到這個專案的網址嚕

之後這個專案都是使用這個網址，把它點開來，順利的話，就可以看到成品嚕～

但這個時候還不要高興的太早，因為我們還沒有設定這個專案的環境變數

這邊我們再回到 GitHub 頁面，選擇 Settings 分頁，選擇左側的「Secrets and variables」。

選擇「Actions」，這時你會發現環境變數的設定有分成兩種

Secrets，是放比較隱私的變數，而 Variables 則是放較不敏感的環境變數。

至於怎麼放，我們可以回到專案，點開「.github」資料夾底下的 deploy.yml 檔案。

在 .env 的地方就有跟我們說明了哪些環境變數使用 secrets，哪些使用 variables，這邊使用「New repository secret」來設定。

完成後，要記得到 Action 的分頁，找最後一個 deploy 的結果，選擇 Re-run all jobs，這樣環境變數的變更才會生效喔！

這邊提醒一下，這隻影片是先幫大家初步掌握「全端專案的核心概念」

讓你了解前端怎麼顯示、後端如何處理資料，以及資料庫如何存取

目前的專案架構，並不適合處理邏輯複雜與高流量的使用場景

但在了解這些概念後，未來後端想換成 Node.js，資料庫換成 MySQL 會輕鬆許多

## 六、結語：培養真正的工程思維

最近我拍了許多 AI 主題的影片，但主要目的不是要介紹工具如何使用，而是透過自己的經驗，從「實戰」的角度去深入分享。

這幾年來，我在學校、企業講過許多 AI 輔助撰寫程式的課程，也就是現在大家所說的 Vibe Coding。

很多學員都在問我，使用哪個工具才是最好的，我跟他們說：「選擇你最順手的主流工具就好，儘管工具間有差異；但完成專案的關鍵在於人，而不是工具。」

以程式編輯器來說，從 VSCode、Cursor，到今天介紹的 Antigravity。

工具永遠在更新、競爭，但我開發十幾年後，體悟到很重要的一件事：

**「不管程式語言還是工具，他們的本質都是為了解決問題；**

**一直學新的程式語言、換新的工具，其實對個人的成長極其有限。」**

因為你的精力被分散了，對新手來說，還不如選擇一門程式語言，好好去深入研究。雖然程式間的語法不同，但底層邏輯都是差不多的。

我不會特別崇拜某個程式語言，也不會非某個工具不用，因為現實世界只有「當下最合適」的選擇。

技術會一直迭代更新，但程式的底蘊可以持續累積。

在文章的最後，請大家在 AI 時代一定要保有自己的判斷力與行動力；就算你覺得這篇文章的內容很棒，那終究是「我的使用情境」。

動手將知識轉化成自己實際能用到的，那才會變成你的。

如果未來你打算用 Vibe Coding 開發更完整的全端專案，可以先[看看這隻影片](https://www.youtube.com/watch?v=sLGT6fA9kIA)。

我有錄一隻影片分享 Vibe Coding 時，前端、後端、資料庫最常遇到的陷，他可以幫助你有系統地建立安全意識。

儘管 AI 工具會越來越強大，但他同時會給沒有相關知識背景的人帶來更大的幻覺。

過去 AI 寫錯程式會跑不動，你會知道他錯了。

**可是他現在有能力寫出「看起來能動」的工具，畫面正常、功能正常；但可能背後的邏輯有問題，或者你看到的只是假資料。**

如果沒有前後端概念，不了解資料庫如何運作，那你是沒有能力判斷 AI 給的資訊是否正確的。

記住！AI 是一個好幫手，但他並非萬能

如果這篇文章對你有幫助，可以順手按個讚，把文章分享給正在 Vibe Coding 的朋友們。

  [https://www.tenlong.com.tw/products/9786264142083](https://www.tenlong.com.tw/products/9786264142083)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202512/google-antigravity-full-stack/
