# 【Auto-GPT】會自主完成任務的 AI！安整的安裝＆使用教學

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202304/auto-gpt-guide/
- 發布：2023-04-16
- 主題：AI 實戰
- 標籤：Auto-GPT、OpenAI、ChatGPT、GPT-4
- 摘要：ChatGPT需要我們不停的輸入指令，引導AI的回答方向才能得到期待的結果；而Auto-GPT之所以爆紅，就是因為他能夠「自我反思」，只要給他任務，他就會不停地自問自答，不需要人為插手。就讓筆者透過這篇文章帶大家了解如何安裝Auto-GPT，以及如何使用它來幫我們達成工作吧！

ChatGPT 需要我們不停的輸入指令，引導 AI 的回答方向才能得到期待的結果；而 Auto-GPT 之所以爆紅，就是因為他能夠「自我反思」，只要給他任務，他就會不停地自問自答，不需要人為插手。

聽起來是不是棒呆了？就讓筆者透過這篇文章帶大家了解如何安裝 Auto-GPT，以及如何使用它來幫我們達成工作吧！

```
大綱

手把手帶你安裝 Auto-GPT
取得 OpenAI 的 API Key，並將它將放入 Auto-GPT 環境檔
讓 Auto-GPT 跑起來！基礎操作說明
使用 Auto-GPT 的注意事項

```

## 手把手帶你安裝 Auto-GPT

**SETP 1**：安裝 Python 3.8 以上的版本：[https://www.python.org/downloads/](https://www.python.org/downloads/)

> 如果有安裝 Homebrew 也可輸入指令：`brew install python`

**SETP 2**：安裝 Git：[https://git-scm.com/downloads](https://git-scm.com/downloads)

**SETP 3**：打開終端機（Terminal），透過 Git 指令下載 Auto-GPT：`git clone [https://github.com/Torantulino/Auto-GPT.git](https://github.com/Torantulino/Auto-GPT.git)`

**SETP 4**：輸入 `ls` 確認下載的位置，然後透過 `cd Auto-GPT` 進入目錄。

**SETP 5**：輸入 `pip3 install -r requirements.txt` 安裝必要套件（會跑一段時間）。

如果使用 pip3 指令出現錯誤，可以先輸入 `python3` 確認自己 Python 的環境是否安裝成功。

> **小提醒**：如果是 Windows 環境，輸入 pip3、python3 沒反應，可以改成輸入 pip、python 來替代喔！

## 取得 OpenAI 的 API Key，並將它將放入 Auto-GPT 環境檔

**SETP 1**：登入 OpenAI 後，進入此頁面：[https://platform.openai.com/account/api-keys](https://platform.openai.com/account/api-keys)

**SETP 2**：按下「Create new secret key」即可取得（筆者撰寫文章時，試用版有 18 美金的限額，3 個月後失效；如果你有綁信用卡，請記得設定上限額度，不然可能會哭。）。

**SETP 3**：回到終端機，輸入`mv .env.template .env`，將環境變數範例檔重新命名。

**SETP 4**：輸入 `vi .env` 打開檔案，然後找到「OPENAI_API_KEY」這個關鍵字，將 your-openai-api-key 替換成自己取得的 API Key。

如果不熟悉 command line 操作，也可以進入 Auto-GPT 夾內，直接變更 .env 檔；此檔案為隱藏檔， Mac 系統請輸入「Shift + Command + .」顯示，Windows 系統建議參考[官方說明](https://support.microsoft.com/zh-tw/windows/%E5%9C%A8-windows-%E4%B8%AD%E6%AA%A2%E8%A6%96%E9%9A%B1%E8%97%8F%E7%9A%84%E6%AA%94%E6%A1%88%E5%92%8C%E8%B3%87%E6%96%99%E5%A4%BE-97fbc472-c603-9d90-91d0-1166d1d9f4b5#WindowsVersion=Windows_11)。

看到「.env」後，按右鍵選擇「打開檔案的應用程式」，以文字編輯器的方式開啟進行編輯（若無此選項請從最下方的「其他」選取）。

## 讓 Auto-GPT 跑起來！基礎操作說明

在終端機輸入指令： `python3 -m autogpt`

> **小提醒**：如果關掉終端機，之後想要執行這個指令，請先 `cd Auto-GPT` 再執行，不然會出現找不到 module 的錯誤訊息。

如果一切平安順利，就會看到下圖的畫面嚕！

操作 Auto-GPT 很簡單，主要分成三個部分：

- **AI Name**：AI 機器人名稱。
- **AI Role**：AI 扮演的角色。
- **AI Goals**：AI 要達成的目標，目前最多設定 5 個（如果不想設定這麼多，按下 Enter 就會直接執行）。

下面是我的設定範例：

- **AI Name**：Smart Writer
- **AI Role**：可以滿足我所有需求的 AI 作家
- **AI Goals**：幫我寫一篇介紹 autogpt 的文章

接下來就可以看 Auto-GPT 表演，你可以看到上面是使用 chrome 瀏覽器，他是會搜尋關鍵字、瀏覽網站的喔（目前官方默認使用 Google 搜尋）。

他在 Thinking 一段時間後，會列出：

- 想法（THOUGHTS）
- 推測（REASONING）
- 方案（PLAN）
- 意見（CRITICISM）

並讓你確認是否執行，筆者這邊先按下「y」。

上面就是 Auto-GPT 執行後的結果，如果你不想要他執行的每一步都經過你的審核，輸入「y -N」即可，像是 `y-5` 就是連續執行 5 次的概念。

> 相信透過以上實際演練，讀者能理解 Auto-GPT 的運作邏輯，基本上就是不斷地「自我反思」，也許我們一步到不了目標，但多優化幾次就能抵達。

下面是跑了 5 次後，Auto-GPT 給出的結果：

他還是可以持續優化，不過我們先看看它說出了什麼吧（這邊我用 ChatGPT 把它翻譯成中文，並排版）。

Autogpt是一個多功能的工具，可廣泛應用於各個行業。其先進的算法和靈活性使其成為需要快速高效地創建高品質內容的企業和個人的理想工具。以下是autogpt的一些主要特點及其在不同行業的應用：

1. 電子商務：Autogpt可用於生成產品描述、評論和其他電子商務網站的內容。這可以幫助企業提高搜索引擎排名，吸引更多客戶。
2. 出版：Autogpt可用於生成文章、博客文章和其他出版公司的內容。這可以幫助出版商用更少的時間生產更多內容，降低成本。
3. 市場營銷：Autogpt可用於創建吸引人的市場營銷活動內容。這可以幫助企業建立品牌知名度和提高銷售。 教育：Autogpt可用於生成教育內容，如教科書、學習指南和在線課程。這可以幫助教育工作者創建與學生相關且有吸引力的高品質內容。
4. 醫療保健：Autogpt可用於生成醫療內容，如患者教育資料和研究論文。這可以幫助醫療專業人士向患者和其他利益相關者傳達複雜的醫療信息。
5. 法律：Autogpt可用於生成法律內容，如合同、摘要和法律意見。這可以幫助律師和其他法律專業人士更高效地生產高品質內容。
6. 新聞：Autogpt可用於生成新聞文章和其他媒體公司的內容。這可以幫助記者用更少的時間產生更多內容，跟上快節奏的新聞週期。

總之，autogpt是一個可廣泛應用於各個行業的強大工具。其先進的算法和靈活性使其成為需要快速高效地創建高品質內容的企業和個人的理想工具。無論您身處電子商務、出版、營銷、教育、醫療保健、法律還是新聞行業，您都應該考慮使用autogpt這一工具。憑藉其先進的算法和靈活性，無論您的需求是什麼，autogpt都能夠快速有效地幫助您創建高品質的內容，為您的業務和事業帶來更多的機會和競爭優勢。

## 使用 Auto-GPT 的注意事項

- **請小心使用不限執行次數的指令**：輸入 `python3 -m autogpt –continuous`，就會讓 Auto-GPT 持續執行；使用這個模式的時候，Auto-GPT 也會給你提出警示，畢竟每次執行都是要消耗 Token。

- **OpenAI 並不是完全免費**：我們使用 Auto-GPT 執行任務時，會不斷地消耗 Token，一但試用的額度滿了，就需要付費。
- **請設定 OpenAI 的付費額度**：如果你免費額度用完後，就需要透過付費繼續使用，但建議設定合理的上限額度，否則信用卡可能被刷爆：[https://platform.openai.com/account/billing/limits](https://platform.openai.com/account/billing/limits)

- **Auto-GPT 的答案未必正確**：因為 Auto-GPT 的本質還是使用 OpenAI 的 GPT-3.5、GPT-4，這兩個模型會犯的錯誤，Auto-GPT 還是會犯。
- **沒有 GPT-4 也能使用嗎**：如果你提供的 API Key 沒有 GPT-4，會自動默認 GPT-3.5。
- **執行的紀錄在哪裡呢**：在「Auto-GPT/outputs/logs/」底下。

不過 ChatGPT 與 Auto-GPT 的使用情境還是有所差異的，如果你今天想學習一個技能、幫助發想，ChatGPT 更好用；但假使你的目標很明確，你也很清楚事情執行的步驟，那 Auto-GPT 更加適合。

以上是 Auto-GPT 的安裝與基礎應用示範，希望對大家有所幫助～

> 如果覺得環境設定起來太複雜，可以參考筆者的這篇文章：[**【AgentGPT】網頁版的 Auto-GPT，懶人的福音**](https://deanlin.net/blog/202304/agentgpt-guide/)

延伸閱讀：[讓我們一起掌握 AI 帶來的紅利](https://dean-lin.medium.com/list/ddcc9c53a4ac)

**想將 AI 導入自己的職場與生活嗎？歡迎[購買筆者出版的新書](https://www.books.com.tw/products/0011004216)**，幫助您**在 AI 的浪潮中站穩腳步，並在未來的職場競爭中佔據優勢！**

  [https://www.books.com.tw/products/0011004216](https://www.books.com.tw/products/0011004216)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202304/auto-gpt-guide/
