# 不用網路！在電腦安裝類 ChatGPT 的 AI，還可以上傳文件對話（GPT4All）

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202401/gpt4all-offline-ai/
- 發布：2024-01-11（更新：2024-10-24）
- 主題：AI 實戰
- 標籤：ChatGPT、GPT4All、AI、LLM、Local
- 摘要：使用 ChatGPT 等 AI 聊天機器人時，我們會被提醒不可上傳個人、公司的隱私資料。儘管 ChatGPT、Copilot 有推出企業方案，宣稱不會將客戶的資料拿去訓練，但很多公司對此還是持保留態度。因此在本地安裝 LLM 大語言模型的 GPT4All…

## 不用網路！在電腦安裝類 ChatGPT 的 AI，還指定對話的文件（GPT4All）

使用 ChatGPT 等 AI 聊天機器人時，我們會被提醒不可上傳個人、公司的隱私資料。

儘管 ChatGPT、Copilot 有推出企業方案，宣稱不會將客戶的資料拿去訓練，但很多公司對此還是持保留態度。

因此在本地安裝 LLM 大語言模型，即使沒有網路也能使用的 GPT4All 也許是個不錯的替代方案，他在 Windows、Mac、Ubuntu 都能輕鬆使用。

  在 ChatGPT 當機的時候就會覺得有他挺方便的

```
文章大綱

STEP 1：下載 GPT4All
STEP 2：安裝 GPT4All
STEP 3：安裝 LLM 大語言模型
STEP 4：開始使用 GPT4All
STEP 5：調整 GPT4All 設定
Bonus：指定對話的文件（LocalDocs）

```

## STEP 1：下載 GPT4All

GPT4All 的操作介面跟 ChatGPT 很像，你也可以將聊天記錄保存在自己的電腦，進入官網後挑選自己的作業系統下載就好：

延伸閱讀：[GPT4All](https://gpt4all.io/index.html)

往下滑一些會看到不同模組（Model）的效能測試數據。

  包含是非題、日常理解能力、推理能力、學術問題等測試項目

## STEP 2：安裝 GPT4All

在安裝上，只要一直按「Next」下一步就好。

同意使用者條款要打勾才能下一步

➤ **小提醒**：如果官網最新的版本有問題，可以去官方 Github 下載穩定版（筆者下載的是 2.5.4）

延伸閱讀：[Releases · nomic-ai/gpt4all](https://github.com/nomic-ai/gpt4all/releases)

## STEP 3：安裝 LLM 大語言模型

打開 GPT4All 時，他會先詢問你是否要分享數據來改善 GPT4All，**如不想就選擇 No（筆者建議）**。

然後就會出現下載 LLM 大語言模型的列表，上面會說明這個模型擅長的領域（聊天、寫程式）、由哪間公司訓練、是否能商用，並且提供檔案大小、RAM 需求、參數等資訊，通常檔案越大的效果越好。

  目前看下來，Model 所需要的 RAM 從 1GB~16GB 不等

列表中也有 ChatGPT-3.5、ChatGPT-4 可以選擇，不過需要使用 OpenAI API Key，如果使用這個選項就意味著需要連網與付費。

這邊我挑選 Orca 2（Full）這個由微軟訓練的模型來實驗。

➤ **小提醒**：如果你直接下載網路上模型，只要把檔案放到上圖 Download path 的目錄下就可以嚕。

## STEP 4：開始使用 GPT4All

下載完後，就可以在聊天界面中看到模型嚕～

這邊我就先用「請假信」初步測試給大家看看。

表現的並不是很好，我們再來測試一下「寫程式」的能力。

基礎程式的表現尚可，但…情境複雜一點的就不行了。

看起來寫得有模有樣，不過實際上是錯的，但這個題目 GPT-3.5 可以輕鬆答對。

## STEP 5：調整 GPT4All 設定

點擊上方的「齒輪」，可以進一步調整 GPT4All 的設定。

### Model（調整模型參數）

- **System Prompt**：這是對話開始前會執行的指令，你可以在這邊設定 AI 要扮演的角色（ex:翻譯員、工程師），這邊有很多優秀的 Prompt 可以參考: [https://github.com/f/awesome-chatgpt-prompts](https://github.com/f/awesome-chatgpt-prompts%E3%80%82)
- **Prompt Template**： 設計 AI 要如何回應你，如果你希望 AI 都以中文回覆，就可以加上「用繁體中文回應。」
- **Generation Setting**：如果沒有特別需求，筆者並不建議調整；另外 Max Length 儘管可以調整，但如果超過模型可以承受的長度，他是不會理你的。

下面我讓他扮演詩人（poet）的角色，並用「love」這個主題來作詩。

  大家可以看到就算指定繁體中文，很多時候還是會以簡體呈現。

### Application（基礎設定）

- **Theme**：調整主題為深色（Dark）、淺色（Light）。
- **Font Size**：字體大小。

## Bonus：指定對話的文件（LocalDocs）

如果想上傳自己的文件，就需要先去下載對應的 Model。

在 LLM 列表中選擇「SBert」這個模型來安裝。

點擊齒輪後，在 LocalDocs 分頁透過 Browse 選擇資料夾，點擊 Add 便可新增文件；他支援大多數的文件格式（doc/pdf/txt/html/xls/csv/json…）。

新增後的的路徑會出現在下方。

然後我們開一個新的對話來實驗看看，這邊要把對應的 Local Documents 勾選才會生效喔！

下面可以看到 AI 的回答會參考我給予的文件。

不過這部分測試下來，如果把它當成「關鍵字查詢」的工具還算堪用；若想做更進階的應用，像是整合、發想、摘要，AI 的表現都不太好。

## 總結

跟官方的 GPT-4 相比，他真的蠻弱（笨）的；這邊分享筆者在體驗後看到的優缺點：

- **優點**：可以不連網使用、隱私性較高、免費。
- **缺點**：回答速度較慢、無法回答太長的內容、回覆品質較差、理解問題能力較差。

如果公司禁止使用 ChatGPT、Bard 等需要連網的 AI 工具，或是你有許多隱私資訊需要處理；也許 GPT4All 會是一個可行的替代方案。大家可以多測試幾個 Model，比較一下各自的差異。

➤ **小提醒：**筆者並沒有仔細研究裡面的程式碼，若打算上傳機敏資料，請自行評估風險。

延伸閱讀：[讓我們一起掌握 AI 帶來的紅利](https://dean-lin.medium.com/list/ddcc9c53a4ac)

**想將 AI 導入自己的職場與生活嗎？歡迎[購買筆者出版的新書](https://www.books.com.tw/products/0011004216)**，幫助您**在 AI 的浪潮中站穩腳步，並在未來的職場競爭中佔據優勢！**

  [https://www.books.com.tw/products/0011004216](https://www.books.com.tw/products/0011004216)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202401/gpt4all-offline-ai/
