# 【AgentGPT】網頁版的 Auto-GPT，讓 AI 成為我們的生產力工具

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202304/agentgpt-guide/
- 發布：2023-04-18
- 主題：AI 實戰
- 標籤：AgentGPT、Auto-GPT、ChatGPT、OpenAI、AI
- 摘要：Auto-GPT 的安裝＆使用相對繁瑣，可能會導致許多想用工具的人卡在環境的設定上。 所以筆者再介紹另一款無需安裝，可直接使用的工具「AgentGPT」；他的操作超級簡單，只要設定好目標（Goal），一鍵發送即可！

在[上一篇文章](https://deanlin.net/blog/202304/auto-gpt-guide/)筆者分享了 Auto-GPT 的安裝＆使用，不過因為操作相對繁瑣，可能會導致許多想使用工具的人卡在環境的設定上。

所以這邊再介紹另一款無需安裝，可直接使用的工具**「AgentGPT」**；他的操作超級簡單，只要設定好目標（Goal），一鍵發送即可！

```
大綱

 如何免費使用 AgentGPT
 加入 OpenAI API key 讓 AgentGPT 給出更完整的答案
 儲存 AgentGPT 的紀錄
 自己設定 Auto-GPT 有什麼好處？

```

## 如何免費使用 AgentGPT

網站：[https://agentgpt.reworkd.ai/](https://agentgpt.reworkd.ai/)

跟 Auto-GPT 不同，你只要輸入「Name、Goal」就可以開始執行了。

儘管無須登入就可以直接執行，但執行到第 4 次就會停止。

## 加入 OpenAI API key 讓 AgentGPT 給出更完整的答案

如果想產出完整的內容，需要在左右下「Settings」設定自己的「OpenAI API key」。

登入 OpenAI 後，進入此頁面：[https://platform.openai.com/account/api-keys](https://platform.openai.com/account/api-keys)

按下「Create new secret key」即可取得（筆者撰寫文章時，試用版有 18 美金的限額，3 個月後失效；如果你有綁信用卡，請記得設定上限額度，不然可能會哭，具體操作請參考[上一篇文章](https://deanlin.net/blog/202304/auto-gpt-guide/)）。

取得 Key 後，在 Settings 的 Key 欄位貼上即可，另外可以設定幾個參數，第一個是 Model。

特別提醒一下，如果你的 API Key 沒有 GPT-4 的權限，就別選擇了，不然會出現如下錯誤：

點開 Advanced Settings 後，可以設定 Temp、Loop 兩個參數：

- **Temp**：回答的方式，數值越大越充滿創意，數值越低則更專注穩定，可設定在 0~1 之間。
- **Loop：** 執行次數，可設定在 1~100 之間。

如果覺得答案滿意了，就點擊「Stop agent」，否則他會執行到 Loop 結束。

下面是 AgentGPT 的成果，有興趣的朋友可以參考一下：

Autogpt is an advanced AI model that has been widely used in various industries to increase productivity and efficiency. In this article, we will explore some successful implementations of Autogpt in real-world scenarios.

1. Writing and Content Creation
Autogpt has been extensively used in writing and content creation. With its powerful language processing capabilities, Autogpt can generate high-quality content in different formats such as articles, emails, and social media posts. Companies have been using Autogpt to automate content creation, which has significantly reduced the time and resources required to produce content.

2. Customer Support
Autogpt has also been used in customer support to automate responses to frequently asked questions. This has helped companies to improve their response time and provide quick solutions to customers’ queries. Autogpt can analyze customer queries and generate appropriate responses in a matter of seconds.

3. Data Analysis
Autogpt has been used extensively in data analysis to generate insights and predictions. With its ability to process large amounts of data, Autogpt can analyze complex datasets and provide actionable insights. Businesses have been relying on Autogpt to analyze customer behavior, market trends, and sales data.

4. Translation Services
Autogpt has been used in translation services to automate the translation of text from one language to another. This has helped businesses to communicate with their customers and partners from different parts of the world. Autogpt can translate large volumes of text accurately and quickly.

In conclusion, Autogpt has been successfully implemented in various industries, including writing and content creation, customer support, data analysis, and translation services. Its ability to process large amounts of data, analyze complex datasets, and generate high-quality content has made it a valuable tool for businesses looking to increase productivity and efficiency.

> **小提醒**： AgentGPT 的答案未必正確，因為他的本質還是使用 OpenAI 的 GPT-3.5、GPT-4，這兩個模型會犯的錯誤，AgentGPT 還是會犯。

## 儲存 AgentGPT 的紀錄

在右上方有「Image、Copy、Save」等 3 個選項：

- **Image**：將執行過程儲存成 png 檔，會非常的長，不建議使用。
- **Copy**：會得到複製的文字，如果想自己做一些後製處理，這個最為合適。
- **Save**：會得到 PDF 檔，適合當成紀錄。

## 自己設定 Auto-GPT 有什麼好處？

我想應該會有人覺得 AgentGPT 這麼好用，那就用它就好了啊？設定 Auto-GPT 這麼麻煩！

其實從使用體驗來說，AgentGPT 在操作上真的好很多，但如果你想要細部調整一些「.env」參數（ex：max_tokens 是決定回答長度的參數），Auto-GPT 更加自由。

不過如果僅是基礎操作，網頁版的 AgentGPT 就已經符合大多數人的需求了。

延伸閱讀：[AI 實戰](https://deanlin.net/categories/ai/)

**想將 AI 導入自己的職場與生活嗎？歡迎[購買筆者出版的新書](https://www.books.com.tw/products/0011004216)**，幫助您**在 AI 的浪潮中站穩腳步，並在未來的職場競爭中佔據優勢！**

  [https://www.books.com.tw/products/0011004216](https://www.books.com.tw/products/0011004216)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202304/agentgpt-guide/
