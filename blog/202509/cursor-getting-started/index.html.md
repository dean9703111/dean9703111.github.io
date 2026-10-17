# Cursor 教學：入門到實戰，用 AI Agent 自動化你的工作流！

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202509/cursor-getting-started/
- 發布：2025-09-14
- 主題：AI 實戰
- 系列：Cursor 教學
- 標籤：Cursor、AI、AI Agent、實戰、入門
- 摘要：Cursor 不只是工程師專屬的程式編輯器（IDE），更是創作者的 AI 夥伴！斜槓講師、作家、影片創作者的我，就把它應用到課程規劃、文章撰寫、資料彙整、字幕生成、腳本設計、字卡規劃、音效建議…等多個領域。

Cursor 不只是工程師專屬的程式編輯器（IDE），更是創作者的 AI 夥伴！

斜槓講師、作家、影片創作者的我，就把它應用到課程規劃、文章撰寫、資料彙整、字幕生成、腳本設計、字卡規劃、音效建議…等多個領域。

這篇文章會從 Cursor 的基礎操作開始，帶大家學會範圍限制，與設定 Rules 提升答案品質。

最後透過實際案例，分享 AI Agent 可以給我們帶來的幫助。

建議搭配影片觀看實際的操作步驟。

[YouTube 影片：YouTube 影片](https://www.youtube.com/watch?v=kVniOF36GEk)

## 動機：我厭倦在 AI 工具間複製貼上

我的本職是工程師，一開始只把 Cursor 當做程式編輯器；而創作文章、資料彙整還是在 ChatGPT 或 NotebookLM 上操作。

**但時間一久，我覺得要根據需求使用不同工具很浪費時間。**

像是我在企業內訓備課的時候，有些主辦方會給我課程的參考資料，但這些資料的格式不一，可能是 Word/PDF/PPT。

而且有些資料還有重複的問題，儘管可以靠 NotebookLM 彙整資訊，但上傳、複製、貼上的動作不但無趣又很浪費時間，而且無法跨筆記本操作。

另外我開始拍 YouTube 後，也需要花很多時間處理「腳本設計、字卡規劃、音效建議、宣傳導流」這一系列的事情。

**錄幾隻影片後，我覺得自己拍片的熱情，快被這些瑣碎的事情給消耗殆盡。**

於是我開始思考：

有哪些任務可以交給 AI 處理？有沒有可能，把任務集中到一個工具搞定。

最後我把寫程式的思維，整合進 Cursor；**才發現它根本就是創作者的神器！**

## ① 下載 Cursor，掌握基礎操作

### 下載安裝

首先我們要進入[ Cursor 官網](https://cursor.com/cn/downloads)，點擊「下載」按鈕，便可獲得安裝檔。

這篇文章以 Mac 操作為主，但 Windows 與 Linux 系統也是可以操作的。

### 隱私設定

在安裝並註冊完 Cursor 後，我們先打開設定；確認目前是否已經選擇 Privacy Mode。

啟用後，通過 Cursor 伺服器的資料不會被用於模型訓練，提升我們使用上的資訊安全。

### 基礎操作

操作方面，你可以在右側的對話窗，透過對話向 AI 提出需求，提示詞的技巧與 ChatGPT 相同。

下面是範例 Prompt：

```
幫我設計一個 Cursor Vibe Coding 的教學大綱，並建立一個 Markdown 檔案，檔名需為好理解且具描述性的英文檔名。
大綱內容要有清楚的結構（如章節或步驟），用詞要容易理解，方便教學使用。
Cursor 是一款 IDE 工具，課程要注重如何善用 AI 取得更流暢的開發體驗。授課對象為沒寫過程式的學員。

```

他遵循的結構如下：

- **目標**：在目標方面，我跟他說要設計一個 Cursor Vibe Coding 的教學大綱
- **格式**：在輸出格式上，我要求他要用 Markdown 檔案呈現，並且使用英文檔名
- **注意**：並要求他注意設計大綱時結構要清楚，用詞要容易理解
- **脈絡**：並補充工具簡介與學員的背景

從下圖可以看到，只要提示詞足夠明確，就能在初始對話獲得品質較高的回覆。

同時也跟大家說明 Cursor 與 ChatGPT 最大不同的點，**就在於 ChatGPT 要來回複製貼上，而 Cursor 可以直接新增、修改檔案，讓你免於複製貼上的繁瑣步驟。**

### Clear all chat

但也提醒大家要**定期清空對話**，避免 AI 因為錯誤的上下文，導致回覆的品質下降。

### Cursor 價格說明

完成註冊後，一開始會有兩週的 Pro 版試用，但試用是有額度限制的；像我大概一天就把免費額度用完。

如果大家看完文章，並在實際操作中，覺得 Cursor 對你的工作、創作有幫助，可以考慮付費升級到 Pro 版。

**以我個人來說，Cursor 幫我節省下來的時間、精力，遠遠超過這個價值。**

## ② 了解不同模型的使用時機

現在是生成式 AI 大爆發的年代。

每隔幾天，就會有新聞報導，說某個模型又取得了更優秀的成績。

**但如果你使用的是 Cursor，那現在最夯的 ChatGPT、Gemini、Claude 的最新模型都能使用。**

### 模型選擇

下面的列表，就是Cursor 有提供的模型：

你會看到有些模型**有大腦的符號 **🧠，這代表他是「推理模型」，可以**處理更複雜的任務，但執行時間較長**。

而其他的則是「非推理模型」，面對**簡單問題可以更快的得到答案。**

你可以在這個頁面勾選想使用的模型，**建議勾選最新的模型，會得到更好的表現。**

不過在對話的時候，我建議選擇「Auto」就好，讓 AI 來幫你決定；不然每次對話都要選對應的模型也太累人。

## ③ 學會範圍限制，提升答案品質

儘管 Cursor 具備檢索「整個專案」的能力；但執行比較久，錯誤率也會隨之上升。

所以我會**建議限制參考資源的範圍，比如特定的資料夾、檔案；**這樣 Cursor 就能更快、更精準地完成需求。

使用上非常簡單，用拖拉的方式即可，可以一次選擇多個檔案、資料夾。

如果想要更精確的指定，你也可以選擇文件內特定範圍的文字（點擊 Add to Chat），請 AI 針對性的閱讀、修改。

## ④ 設定 Rules，讓 AI 更懂你

除了限制範圍可以提升 AI 回應的品質外，設定好 Rules 可以讓 AI 更懂你

Rules 就是告訴 AI，你的工作習慣、偏好和要求的設定

這邊分享一個 [Cursor 內部員工的 Rules 範例](https://x.com/kayladotdev/status/1853272891023872450)：

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

大家可以把它複製起來，然後貼到 Rules 設定中，這會在所有專案中生效！

另外大家可以再加上一個 Rules，告訴 AI 你習慣使用的語言

```
Always respond in 繁體中文

```

每個人都有不同的職業、偏好、需求，大家可以針對性地來設計專屬自己的 Rules，讓他更符合工作流。

## ⑤ 撰寫 Spec，讓 AI 知道怎麼做事

剛剛示範的 Rules，是跟著你的帳號走，在每個專案都會生效；但現實生活中的每個專案，往往都有不同目的。

像我就是一個斜槓的工程師，除了寫程式外，還要寫部落格、寫書、備課，甚至還有拍 YouTube。

**如果把每個身份都寫進 Rules，不但不好管理，甚至還會讓 AI 混亂，導致效能下降。**

所以這個時候，你應該要**針對專案的不同目標，來設計 Rules，不過因為規則複雜，所以我習慣把它稱為 Spec，也就是需求規格**。

讓 AI 知道在不同專案下，該如何完成任務。

### 建立給專案的 Rules

在剛剛建議 Rules 的地方往下滑一點，就可以建立針對 Project 的 Rules。

比如我們想請 AI 做的是把文章轉成腳本，名字我就打「article-to-script」。

接著你會看到這個檔案出現在「.cursor」的「rules」資料夾中，副檔名為 .mdc。

你可以自由設定這個規格要觸發的時機，像是永遠觸發、AI 自動判斷、針對特定檔案、或是人為呼叫都可以。

至於內容要怎麼寫，我們一樣可以請 AI 幫忙。

```
你是一位擅長打造 KOL 的專家團隊，我想請你請從業界實戰的角度，規劃出當用戶輸入「文章轉腳本」時， AI 要執行的步驟、輸出的限制、以及結果品質的檢查，完成後將結果更新到這個檔案中。

```

得到初步的版本後，你可以根據實際需求調整細節；比如我**在結尾加入這個腳本要儲存的位置，避免修改原本的檔案。**

```
## 檔案儲存位置
在同層資料下以 `{原檔名}_script.md` 命名來建立檔案

```

### 驗證 Rules 是否生效

這邊我拿自己過去寫的一篇文章，實際 Demo 給大家看；我先把這篇文章拖到右側 AI 對話窗，然後跟他說「文章轉腳本」。

接著稍等一段時間，一份腳本的草稿就生成了，這可以大幅增加創作者的效率！

## ⑥ 體驗 AI Agent 一鍵搞定的快感

### 讓 Project Rules 互相參考

在剛剛的範例中，我只有提供 AI 一個客製化規則。

**但其實你可以設計多個可以串聯的規則，這邊我也拿自己的案例來分享。**

我有為了製作 Youtube 專門開一個專案，並在下方設計完整的 Spec，涵蓋了了「文章轉腳本，腳本轉逐字稿，以及把腳本轉成短影音」的規則。

我除了可以透過這些關鍵字來執行外，也可以輸入「一次搞定」，這邊我就有告訴他完整的執行順序。

### 發揮 AI Agent 的潛力

在影片的開頭，我有說到 Cursor 可以幫我彙整 Word/PPT/PDF 等多種格式的文字與圖片。

不過更準確的說法，是 Cursor 辨識完文件的格式後，安裝了許多 Python 的套件，然後下指令提取出檔案的文字、圖片。

**而這一系列從「辨識文件、安裝套件、執行指令、彙整文件」的操作，都是 AI 幫我完成的，我只有要求他幫我彙整資料夾內的文件，整合資訊到新檔案而已。**

我有次也好奇 AI Agent 能做到什麼地步，於是把 MP4 影片檔丟給他，請他幫我生成出 SRT 字幕檔，結果沒想到**他安裝完辨識語音的 Whisper 套件後，真的把字幕檔生成出來了**。

看到結果的時候我超 Shock！

但也提醒一下大家，**儘管 AI Agent 可以幫我們做到很多事情，但開放權限給他時，請先搞清楚是否會造成危害。**

**像套件自動安裝、指令自動執行儘管很方便；但越強大的工具，出事時，問題往往也越大。**

AI Agent 有辦法將一個簡單的指令，解析成「多個執行步驟」後，完成「複雜、龐大」的任務（但結果未必正確）。

## 結語：一起嘗試 AI 工具的潛能吧！

希望這篇文章，能讓沒有開發經驗的朋友，也願意嘗試 Cursor 這款強大的 AI 工具。

在具備正確觀念，並熟悉設定後，相信 AI 可以幫我們搞定許多繁瑣的事務，讓我們專注在真正有價值的工作上！

如果你對 AI 工具、自我成長、職涯發展感興趣，[歡迎追蹤我](https://dean-lin.medium.com/subscribe)！我會我會持續分享新的觀點 💡

  [https://www.tenlong.com.tw/products/9786264142083](https://www.tenlong.com.tw/products/9786264142083)

延伸閱讀：[List: 讓我們一起掌握 AI 帶來的紅利 | Curated by 林鼎淵 | Medium](https://medium.com/@dean-lin/list/ai-ddcc9c53a4ac)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202509/cursor-getting-started/
