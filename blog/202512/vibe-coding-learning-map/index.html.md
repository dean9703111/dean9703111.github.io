# 【Vibe Coding 學習地圖】用實戰帶你搞懂 Gemini、Google AI Studio、Antigravity 的使用情境

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202512/vibe-coding-learning-map/
- 發布：2025-12-19
- 主題：AI 實戰
- 系列：Google Antigravity
- 標籤：Vibe Coding、Gemini、Google AI Studio、Antigravity、Google Antigravity
- 摘要：掌握 AI 工具應用場景，Vibe Coding 不迷路

如果你剛開始接觸 Vibe Coding，可能會有一種感覺：

❝工具很多、教學很多，但不知道該怎麼開始，才不會走冤枉路。❞

因此這篇文章，我會用「實戰 Demo」的方式，帶你有系統的建立使用觀念。

讓你知道不同階段，該選擇什麼樣的 AI 工具比較合適。

我們會從入門的 Gemini，到進階的 Google AI Studio，再到偏專業應用的 Antigravity。

實際示範它們各自的特點。

> 完整的 Demo，可以觀看 YT 喔 👉
> 對 AI 應用感興趣的朋友，歡迎按下訂閱按鈕，未來我計畫分享一系列 AI 工具的應用教學。

[YouTube 影片：【Vibe Coding 學習地圖】用實戰帶你搞懂 Gemini、Google AI Studio、Antigravity 的使用情境](https://www.youtube.com/watch?v=2hbYCe_E5aU)

## 一、Gemini：Vibe Coding 起手式

現在應該很多人使用 [Gemini](https://gemini.google.com/app) 處理自己的日常工作事宜。

不管是文書應用、圖片生成，還是簡報草稿，他都能給我們驚艷的表現。

同時，他也是最適合新手入門 Vibe Coding 的工具。

因為他只要打開瀏覽器就能操作，用簡單的對話就能生成漂亮的網頁，完成後還可以把成果分享給朋友。

下面我直接示範給大家看：

### 優化提示詞

像我本身是一個作家，如果想製作一個宣傳自己書籍的 Landing Page（著陸頁）。

就可以請 AI 來協助設計，但第一步，我會先提出自己的基礎需求，請他優化成更完善的提示詞：

```
我想要提供[書籍簡介]讓 AI 生成網頁

下面的 Prompt 如何優化才能得到更好的結果？

```
根據下方內容，判斷合適的風格後，設計出吸引人的互動網頁
```

```

- **需求**：一開始我先說明自己的需求是提供書籍簡介，讓 AI 生成網頁
- **目標**：然後請 AI 優化我提供的初始提示詞
- **初始 Prompt**：最後提供初步的 Prompt 請 AI 延伸

相信有些人會覺得要學那麼多提示詞架構好累，到底 AI 是在幫我還是在搞我？

下次你有這樣的想法時，不妨學我先描述自己腦中的想法，然後請 AI 協助優化成更加完善的提示詞。

像這邊 AI 就幫我們撰寫了一個更完善的提示詞。

把它複製下來後，我建議大家開啟一個新的對話貼上去，這麼做是為了避免上下文的干擾。

### 用 Canvas 生成網頁

點開工具，選擇 Canvas，這樣才能生成網頁。

另外最下面要放上給 AI 參考的書籍資訊，這邊我就使用今年剛出版的[工程師下班有約](https://www.tenlong.com.tw/products/9786264142083)當作範例。

把書籍大綱貼上後，就可以點擊送出，稍等一段時間，網頁就生成完畢了！

> 但這邊要提醒一下大家，有結果不代表內容正確。

### 優化網頁細節

比如你點擊購買書籍，會發現這個按鈕根本是假的，按下去毫無回應，另外網站沒有作者頭像，也沒有書籍封面。

如果要增加這些資訊，你可以直接跟他說：

```
根據下面需求調整
1. 書籍的購買連結為: https://www.tenlong.com.tw/products/9786264142083
2. 作者頭像連結：https://duk.tw/DVw5tb.jpg
3. 書籍封面連結：https://duk.tw/0poc5o.jpg

```

特別提醒一下，在 Gemini 想加入圖片，需要把圖片上傳到圖床網站上，然後貼上圖片網址才會有效。

**你直接複製圖片貼上是沒有用的喔！**

調整完畢後，就可以看到作者頭像跟書籍封面了，購買連結也可以順利跳轉到我們指定的網頁了。

### 新增 Gemini 功能

這邊再介紹一個 Gemini 有趣的功能

如果你點擊右下角「新增 Gemini 功能」，AI 會根據網站主題來自動增加對應的功能

AI 新增了 Dean AI 職涯導航的功能：

### 分享網頁

確認網頁都調整的差不多後，你可以點擊分享向大家展示成果

但是上下面都有 Gemini 的浮水印標誌，我覺得有點礙眼

### 用 Google Site 建立網站

這邊要跟大家分享一個移除標誌的小技巧，那就是使用 [Google Sites ](https://sites.google.com/)來免費建立網站。

記得把剛剛建立的頁面拉到最上方，然後對原本的首頁點擊右鍵，選擇刪除，就可以發布網頁了！

用 Google Site 發布網頁後，原本在最上方跟最下方的 Gemini 標籤消失了。

但如果你去使用 AI 功能，就會發現無法順利使用，這是因為假如你要使用 AI 功能，會需要搭配 Gemini 的 API Key。

### API Key 警告

把視角轉換到 Gemini，在程式碼的地方搜尋 apikey，如果沒有填寫的話，你是無法使用 Gemini AI 功能的。

至於這個 Key 如何取得，我會在下一節 Google AI Studio 的介紹中說明；但並不建議大家使用，因為免費的 API Key 額度很少。

如果想要正式使用，勢必要綁定帳單，但假使額度沒有設計好，你的信用卡可能會被刷爆。

另外使用 Gemini 建立的網頁是純前端網頁，如果你真的把 API Key 貼上去，別人按下 F12，搜尋 apikey 就能取得你的設定，然後刷爆你的卡。

> 希望大家不要跟自己的錢包過不去！

### 純 HTML 網站可以直接複製

這邊再給大家建立一個基礎觀念，Gemini 建立的 App 基本上都是純前端網頁（可搭配 Firebase 存取資料庫資源）。

所有的業務邏輯都是公開的，也就是說如果有人想要抄你的網站，只要檢查網頁元素，把 HTML 的部分複製起來，就可以整包帶走。

儘管 Gemini 可以用簡單的指令建立網站，**但透過他建立的應用，別人可以毫無難度的直接抄襲。**

但我依然建議大家先使用 Gemini 作為 Vibe Coding 入門的工具，因為成就感是一切的起點。

## 二、Google AI Studio：掌握進階技巧

用 Gemini Vibe Coding 會得到一個 HTML 檔案，結構相對簡單，而接下來分享的 Google AI Studio 則是以「前端專案」的角度呈現。

### 生成第一個 AI App

這邊我們先選擇 Vibe code GenAI Apps 來建立第一個 AI App。

在官網中，你可以看到他有許多可以選擇的 AI 應用，圖片生成、語音辨識、影片生成等應有盡有，這邊我先以最近爆火的 Nano banana 為範例，帶大家完成一個有 AI 功能的網頁。

這邊我跟他說：

```
使用者只要上傳照片，就可以轉換成自己想要的風格
介面先列出 5 個常見風格，也允許使用者自己輸入

```

接著點擊「Build」後就可以開始看 Gemini 表演了，稍等一段時間後，這個 AI APP 就完成惹。

這邊我先上傳自己的照片，選擇一個風格後，點擊開始轉換就能得到結果嚕～

### 分享 App

在確認結果符合需求後，我們就可以分享成果。

把 Publish your app 打開，這樣別人才能看到；而 Default to fullscreen 打開，才能預設顯示全螢幕，否則會是跟 Google AI Studio 對話的視窗。

用這種方式分享 App，一樣是要登入 Google 帳號後才可以使用。

但用這種方式分享，App 的上下都有預設的文字，我個人很不喜歡。

另外點擊右上角的 Leave fullscreen，你專案的程式碼都是直接公開給對方看的。

甚至可以根據這個 App 為基礎去做二次創作，所以大家分享前要先注意到這些細節。

### Deploy app 注意事項

另外我並不建議大家使用 Deploy app on Google Cloud 這個功能。

因為他要綁定付費的專案才能使用，當別人使用這個 App 時，帳單是算在你頭上的。

**除非你很清楚自己在做什麼，否則我不建議透過這個方式分享 App。**

### API Key

Google AI Studio 首頁，左下角有一個 Get API Key

點進去後，點擊右上角的「Create API Key」就可以取得自己 API Key 了。

但使用上，會分成免費版與付費版兩種：

- 以免費版來說，你只能使用一些比較舊款的模型，像最新推出的 gemini-3-pro 這類模型都是不可用的。
- 但如果你的專案有綁定信用卡，各種最新的模型都可以使用，但使用前你要搞清楚自己在做什麼，因為用多少算多少！

### 建立前端專案

其實 Google AI Studio 除了能建立有 AI 功能的 App 外，也可以建立一般的前端專案，這邊我就以「抽籤、分組」的 App 為例。

並在建立完成後，分享如何將成果保存到 GitHub 做版本控管，並透過 Antigravity 補齊專案細節，在 GitHub Pages 上免費部署。

剛好最近接近尾牙時間，許多公司都會有抽獎的需求，這時不妨自己 Vibe Coding 一個！

我的提示詞是這樣設計的：

```
我是一個 HR，想製作一個網站
可以上傳 CSV 或貼上姓名作為名單來源
有兩個功能，一個是獎品抽籤，會有動畫隨機抽取，這邊可以設定要重複抽取，或是不重複
另一個是自動分組，可以設定要幾人分組，分組後的結果會視覺化顯示

```

等 AI 完成後，我們來驗證一下功能是否符合需求，先貼上姓名，然後去抽籤，接著分組：

### 優化專案

儘管功能都符合需求，但操作過程中發現幾個可以改進的地方：

- 首先是名單，站在想快速理解與測試功能的角度上，如果有模擬名單的功能，那會更實用。
- 另外匯入名單時，目前沒有考慮到重複名單的問題，這塊也可以請 AI 在預覽名單處標記出來，並提供一個按鈕讓使用者一次性移除重複的姓名。
- 最後則是自動分組的地方，希望可以下載成 CSV 紀錄，畢竟這只是一個前端網頁，下次進來後紀錄就不在了。

```
我希望增加以下功能
1. 名單這塊，我覺得可以有模擬名單的選項，讓大家可以理解如何使用
2. 匯入 CSV 或貼上名單時，若發現有重複的學生姓名，則在預覽名單處標記出來，並提供一個按鈕讓使用者一次性移除重複的姓名
3. 自動分組後，希望可以下載成 CSV 紀錄

```

等 AI 調整完畢後，記得測試一下他增加的功能是否符合需求！

### 保存到 GitHub

在測試確認功能都符合需求後，我們可以將它保存到 [GitHub](https://github.com/) 上面。

如果你是第一次接觸 GitHub，可以先到他們的官方網站，用 Gmail 就可以註冊了，他可以幫你的專案做版本控制。

這邊我們點擊「Save to GitHub」

首先你要輸入 App 的 name，建議使用英文，這邊我輸入 hr-helper。

至於 description 則可以中文白話描述，我就寫「上傳名單後可以抽籤、分組」。

因為後續我們想要用 GitHub Pages 發布，所以這邊選擇 Public。

然後點擊「Create Git repo」就建立這個儲存庫嚕！

接著 AI 會根據專案生成這次更新的 commit 訊息，你也可以自行編輯成自己看得懂的版本。

沒問題後點擊「Stage and commit all changes」就會上傳資料。

這邊我們把視角切換到 GitHub，看一下自己帳號的 GitHub Repository，就會發現剛剛我們透過 Google AI Studio 建立的 App 就出現了！

### Gemini 與 Google AI Studio 差異

大家還記得一開始分享 Gemini 生成網頁時，是只有一個 html 檔案的嗎？

而現在我們使用 Google AI Studio 製作出來的網站，就不是一個單純的 HTML 檔案了。

他是用 React 這個框架設計的前端專案，你會發現他的結構複雜很多。

而且這些檔案是要經過編譯後才能使用的，所以無法透過先前 Google Site 的方式來分享網頁。

所以接下來，我要跟大家分享，如何使用 Google 推出的另一款產品 Antigravity 的補充專案細節，並部署到線上環境。

## 三、Antigravity：認識專業工具

[Antigravity](https://antigravity.google/) 是一款程式碼編輯器。

不管你電腦的作業系統是 Windows、Mac、Linux，都可以安裝體驗，目前 Antigravity 只要登入 Google 帳號就可以免費使用。

但如果付費升級到 Pro，你的使用體驗會好很多。

### 設定 Rules，讓 AI 更懂你

建議大家可以選擇「Customizations」來設定一下自己的使用偏好。

在 Rules 這個地方，我選擇 Global，這邊我複製貼上一個 Cursor 內部員工的 Rules 範例，其實提示詞都是通用的：

[https://x.com/kayladotdev/status/1853272891023872450](https://x.com/kayladotdev/status/1853272891023872450)

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

貼上後，這個規則就會在所有的專案中生效。

另外如果你想給目前的專案設定特定的 Rules，比如程式碼規則這類的，也可以點擊 Workspace 來設定喔。

而點擊上方的齒輪符號，你可以點擊「Open Antigravity User Settings」

在 Agent 的分頁中，如果把 Secure Mode 打開，那 AI 執行的過程會安全許多；但缺點就是大部分的任務執行都會詢問使用者。

### 下載 GitHub 專案

了解基礎設定後，接下來我們要透過 Clone Repository 來下載剛剛 Google AI Studio 完成的專案。

下載完成後，左側是專案資料夾區塊，裡面會有各種程式與設定檔；而右側則是 AI Agent 工作區，你可以直接輸入自己的需求。

在模型方面，Google 除了自家的 Gemini 外，還提供了 Claude 與 GPT 的模型讓大家使用。

在對話上，有分成 Planning 跟 Fast 兩種；如果要處理的是複雜任務，就建議選擇 Planning，而簡單的問答用 Fast 就足夠了。

### 完善專案

因為我們要請他補齊專案缺少的資訊，所以我這邊選擇的是 Planning。

然後輸入下面的 Prompt:

```
請擔任資深的 React 工程師，幫專案完成以下任務：
1. 設定 package.json，並安裝對應套件，完成後啟動確定是否可以運行
2. 設計 GitHub Action，讓我可以直接部署上線
3. 設計 .gitignore，避免上傳不必要的資料夾、暫存檔、隱私檔
4. 將相關操作記錄到 README.md

```

如果執行過程遇到不確定的指令，請務必確認

```
請問執行這個指令有危害嗎？
[貼上指令]

```

可以請 AI 執行專案，確認功能符合需求。

確認功能都符合需求後，就可以將變更部署到線上環境。

但在檔案上傳前，我建議大家先到 .gitignore 的這個檔案，確認一下設計是否完善。

### 部署專案

點擊左側的 Source Control，點擊 Generate，AI 就會辨識我們更新的內容，來產生 commit 訊息。

確認文字描述符合自己的預期後，點擊 commit，就會在自己的電腦記錄這個變更；但如果想更新到雲端，你需要點擊 Sync Changes才會正式上傳喔！

完成這些操作後，我們先來 README.md 看看專案的 GitHub Action 如何設計。

我們把視角切換到 GitHub 網頁，前往 Settings 的頁面，選擇左側的 Pages，然後將部署的來源選擇為「GitHub Actions」

完成這些操作後，我們回到 GitHub 點擊「Actions」的分頁，查看部署的狀況。

成功的話會有綠色勾勾，失敗則是紅色叉叉，會需要點進去釐清具體的錯誤原因。

這邊我們看到部署失敗，所以點進去把錯誤訊息複製起來，直接貼到與 AI 的對話視窗中，讓他幫忙解決

```
我遇到如下錯誤，請告訴我如何解決
[貼上錯誤訊息]

```

這邊可以看到錯誤原因是因為剛剛 GitHub Action 還沒開啟，所以我們點擊 Re-run all jobs 讓這個 Job 重新執行看看。

OK，這次執行得很順利，我們點擊一下網址，畫面有成功出來～

以上就是把 Google AI Studio 的 App 透過 Antigravity 放到程式編輯器開發與線上部署的流程。

當然 Antigravity 的功能不只這些，我過去有拍一隻影片專門介紹，如果你做更近一步的功能設計，可以參考下面影片：

[YouTube 影片：【Google Antigravity 教學】把想法化為現實！用 Vibe Coding 完成輕量級全端專案](https://www.youtube.com/watch?v=-FW5DgQEV0M)

## 結語：心態比聰明更重要

到企業做 Vibe Coding 內訓時，我會先問大家一個問題：「你們覺得，Vibe Coding 最需要的是什麼？」

有人說需要有程式的背景，也有人說，與 AI 對話的經驗很重要。

我覺得這些答案都沒錯，但根據我的教學經驗，能把 Vibe Coding 學好，並且實際應用在日常工作中的人，**通常都是過去被某個痛點困擾很久的朋友**。

也正因為這樣，**當 AI 給錯答案、改壞功能，甚至整個流程卡住時；他們不會第一時間放棄，而是願意多試一步、多問一句。**

我想提醒大家一件事：

> Vibe Coding 並不是「一次就成功」的工具

就像今天這麼簡單的前端網頁，我們都能看到 AI 犯錯了；但遇到錯誤不要害怕，因為只有直面錯誤，才有機會一步一步把問題解開。

另外隨著你越來越深入使用 Vibe Coding，面對更複雜的邏輯與資料結構時，你可能會發現：

> 為了完成 A 功能，AI 可能把原本正常的 B 功能一起改壞

但這些問題單純靠「更明確的指令、給予輸入輸出範例、限制操作範圍」等技巧未必能解決。

所以在接下來的 Vibe Coding 系列影片中，我會分享如何用 AI 撰寫「測試程式」。

當專案有測試案例後，即使規模變大、功能增加，也能讓系統維持在相對穩定的狀態！

> Vibe Coding 最大的優勢；就是你可以在短時間快速迭代，驗證想法可行性

如果這篇文章對你有幫助，可以順手按個讚，把文章分享給正在 Vibe Coding 的朋友們。

  [https://www.tenlong.com.tw/products/9786264142083](https://www.tenlong.com.tw/products/9786264142083)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202512/vibe-coding-learning-map/
