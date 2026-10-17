# 新手必看！ChatGPT 常見迷思總整理，你遇到了幾個？

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202303/chatgpt-common-myths/
- 發布：2023-03-21
- 主題：AI 實戰
- 標籤：ChatGPT、GPT-4、GPT-3、DALL-E 2、OpenAI
- 摘要：隨著 ChatGPT 火爆全球，使用人數以指數型成長，許多使用上的迷思也以「日經文」的方式呈現在網路上。 今天這篇文章會用實作的方式帶大家了解 ChatGPT 有哪些常見迷思，並用簡單的範例讓大家感受 GPT-3.5 與 GPT-4 的能力差異，希望對大家有所幫助。

隨著 ChatGPT 火爆全球，使用人數以指數型成長，許多使用上的迷思也以「日經文」的方式呈現在網路上。

今天這篇文章會用實作的方式帶大家了解 ChatGPT 有哪些常見迷思，以此減少踩坑的機會。

並用簡單的範例讓大家感受 GPT-3.5 與 GPT-4 的能力差異，希望對大家有所幫助。

> 大家會有這些迷思，其實就是希望 ChatGPT 在未來也能做到這些事情，筆者自己也期望在不久的將來，ChatGPT 能變得更加強大！

```
大綱

 ChatGPT 可以讀網址做摘要嗎？
 ChatGPT 可以產圖片嗎？
 為什麼 Line 上面可以輸入文字就產圖？
 微軟的 New Bing 有多強（連網的 ChatGPT）？
 GPT-4 可以使用了嗎？
 GPT-4 相較於 GPT-3.5 有什麼不同？
 結語：批判性思維非常重要

```

## ChatGPT 可以讀網址做摘要嗎？

「目前」免費版的 ChatGPT 是無法連網的，也就是說他沒有能力讀取網址後將裡面的內容做摘要。

「請你讀取這個網址：https://medium.com/dean-lin/靠-chatgpt-我一個月寫完一本書-全台第一本-chatgpt-應用解析專書-56334fbb6bc8，並做出內容摘要」

但如果你升級到 ChatGPT PLUS，透過 Plugins 就可以聯網嚕！下面用 WebPilot 來示範。

## ChatGPT 可以產圖片嗎？

「目前」免費版的 ChatGPT 是不具備產圖片功能的，但他可以做到「搜尋」圖片，範例如下。

「請使用 Unsplash API ([https://source.unsplash.com/960x640/?{工程師](https://source.unsplash.com/960x640/?%7B%E5%B7%A5%E7%A8%8B%E5%B8%AB)}) 來找圖片，使用 Markdown 語法且不要用程式碼區塊。」

因為是從網路上搜尋除來的，所以每次刷新網頁會看到不同的圖片。

> 上面的咒語（Prompt）是參考「Will 保哥的技術交流中心」簡化而來。

## 為什麼 Line 上面可以輸入文字就產圖？

現在有不少人開發 Line 機器人，讓我們透過 Line 就能夠與 ChatGPT 溝通。

在探討這個話題前，我們可以先了解一下 OpenAI、GPT、DALL.E、ChatGPT 之間的關係。

OpenAI 提供了許多 API 讓開發人員使用，其中有語言模型如「GPT-3.5、GPT-4」，以及產圖工具「DALL.E」。

ChatGPT 算是「GPT-3.5、GPT-4」的一個應用，主要功能就是文字的問答。

至於 Line 聊天室裡面能夠輸入文字就產圖，其實是「DALL.E」的應用，你可以直接在他的官網上試玩：[https://labs.openai.com/](https://labs.openai.com/)

「abstract pencil and watercolor art of a lonely robot holding a balloon」

> 一個拿著氣球的孤獨機器人，畫風為抽象的鉛筆水彩藝術

至於 Line 聊天室能夠產圖，其實就是應用他們的 API：[https://platform.openai.com/docs/guides/images/introduction](https://platform.openai.com/docs/guides/images/introduction)

現在大部分的聊天室都會要求你去 OpenAI 官網申請 API Key 才能使用他們的服務，筆者提醒如下：

- API Key 是隱私的，如果你透過 Line 訊息交給對方，那對方也能使用這個 API Key。
- API Key 是有期限的，為 3 個月。
- API Key 的使用是有次數限制的，他的計算方式為 Token。

> 當然筆者相信每位開發人員都是善良的人。

## 微軟的 New Bing 有多強（連網的 ChatGPT）？

現在只要前往這個網址：https://www.microsoft.com/zh-tw/bing ，就能**免費**體會 ChatGPT 連上網路後有多強大。

GPT 過去受限於訓練模型，以及資料截止於 2021 年 9 月，導致各種回答都受到限制。

而微軟的 New Bing 就直接解決了這個問題，讓 GPT 可以得到更廣泛的應用，下面就以筆者來舉例。

「請簡短介紹林鼎淵是誰」

  ChatGPT 的回答

> ChatGPT 很明顯就是在瞎掰…我也很希望自己有這麼傑出（然後我還活著啊）。

  New Bing 的回答

而 New Bing 給出的結果就是正確的了，雖然有些指令他並沒有完全吃進去（ex：我要求長度為 250 字，但回饋只有 200 字。）

> New Bing 雖然有連網，但這並不代表他給的資料全然正確，建議讀者要自己進一步求證。

## GPT-4 可以使用了嗎？

如果你有**付費訂閱 ChatGPT**，就能使用「GPT-4」。

不過他的回覆速度真的比 ChatGPT 3.5 （付費版）慢不少…

## GPT-4 相較於 GPT-3.5 有什麼不同？

如果你只是把它當成聊天機器人，那基本上不會感受到太大的不同；但如果把它應用在生產力相關的問題上（ex：產生文案、創意發想、撰寫程式），會發現他變得更好、更精準。

「我想請你擔任專業的文案人員，撰寫一份讓消費者買單的按摩椅文案，走感動人心的路線，限制字數 250 字內，分段呈現。」

> 平心而論，GPT-3.5 寫出的文案還需要不少潤飾，而 GPT-4 的文案幾乎可以直接使用了。

## 結語：批判性思維非常重要

水能載舟，亦能覆舟。

原本就已經資訊爆發的世代，隨著 AI 的發展，又再爆發了一次。

無論 AI 未來發展的多強大，我都希望讀者保持懷疑的態度，千萬不要完全相信他給的答案，一定要自己親自去實踐。

當然，別人給的資訊自己也要有判斷對錯的能力，就像我寫的這篇文章可能也有漏洞，歡迎大家批評指教，這樣我們才能進步成長，感謝大家的閱讀！

延伸閱讀：[讓我們一起掌握 AI 帶來的紅利](https://dean-lin.medium.com/list/ddcc9c53a4ac)

**想將 AI 導入自己的職場與生活嗎？歡迎[購買筆者出版的新書](https://www.books.com.tw/products/0011004216)**，幫助您**在 AI 的浪潮中站穩腳步，並在未來的職場競爭中佔據優勢！**

  [https://www.books.com.tw/products/0011004216](https://www.books.com.tw/products/0011004216)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202303/chatgpt-common-myths/
