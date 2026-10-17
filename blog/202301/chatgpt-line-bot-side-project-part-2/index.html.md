# 靠問 ChatGPT 寫程式，能完成 OpenAI & Linebot 的 Side Project 嗎？（下）

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202301/chatgpt-line-bot-side-project-part-2/
- 發布：2023-01-25（更新：2024-10-24）
- 主題：AI 實戰
- 系列：ChatGPT LINE Bot 實作
- 標籤：ChatGPT、OpenAI、OpenAI ChatGPT、LINE Bot、ngrok
- 摘要：在這篇文章中，我們要來設定 Line 的 webhook，並測試 ChatGPT 寫的程式是否可以正確執行；並告訴讀者遇到問題時，我們可以從哪些面向來思考、解決；最後於文末給想用 ChatGPT 寫程式、學程式的朋友一些建議。

在[上一篇文章](https://deanlin.net/blog/202301/chatgpt-line-bot-side-project-part-1/)中，我們好不容易請 ChatGPT 完成 OpenAI-Linebot 的專案，並順利啟動。

在這篇文章中，我們要來設定 Line 的 webhook，並測試 ChatGPT 寫的程式是否可以正確執行；並告訴讀者遇到問題時，我們可以從哪些面向來思考、解決；最後於文末給想用 ChatGPT 寫程式、學程式的朋友一些建議。

```
大綱

 在 local 建立一個可以讓 Line 訪問的 HTTP server（ngrok）
 設定 Line 的 webhook
 測試功能＆痛苦的 Debug 之路
 結論：給想用 ChatGPT 寫程式、學程式的朋友一些建議

```

## 在 local 建立一個可以讓 Line 訪問的 HTTP server（ngrok）

上一篇文章中，專案是在本地啟動的，因此他的網址是 [http://localhost:3000](http://localhost:3000) ，而 Line 的 webhook 是無法接受本地網址的。

為了方便 Debug，我這邊選擇在 local 建立一個可以讓 Line 訪問的 HTTP server，具體的方案我們來詢問一下 ChatGPT。

這邊我選用「ngrok」的方案，並在指定「作業系統」的狀態下，請 ChatGPT 告訴我接下來該怎麼做。

接下來我們就按造建議的步驟來實作看看。

**SETP 1**：先到 ngrok 的官網下載 ngrok 的 Mac 版本： [https://ngrok.com/download](https://ngrok.com/download) 。

提醒讀者，要先確認自己 mac 的處理器是 Intel 還是 Apple Silicon 喔！

**SETP 2**：解壓縮下載的檔案，並將 ngrok 放入您的 PATH 中 (例如: /usr/local/bin)

解壓縮檔案後，你會得到一個 ngrok 的值行檔，其實你就算放在 download 的資料夾也能執行，不過放到 bin 的資料夾下可以在任何位置執行。

下面說明一下 mac 進入 bin 資料夾的步驟：「到 mac 桌面」or「點擊 Finder」➝ 「前往」➝ 「前往資料夾」➝ 輸入「/usr/local/bin」➝ 把「ngrok 值行檔」丟進去。

**SETP 3**：打開終端機，輸入 `ngrok http 3000` (3000 是您的本地環境的 HTTP Server 的通訊埠)，啟動 ngrok。

**SETP 4**：ngrok 將會顯示一個可供外部訪問的 URL（上圖紅框處 `[https://xxxx.ngrok.io](https://xxxx.ngrok.io)`），您可以在 LINE 的 Developer Console 中設定 webhook 的 URL。

**小提醒**：Session 2 小時後會過期，到時再重新執行一次 `ngrok http 3000` 就好了，然後別忘記把新的 URL 設定到 Line 的 webhook。

## 設定 Line 的 webhook

因為設定步驟很簡單，我就不特別問 ChatGPT 了。

**SETP 1**：在 Messaging API 的 WebhookURL 貼上剛剛的的網址，並於後面加上 `/webhook`（那是我們程式設定的 API 路徑）。

**SETP 2**：按下「Verify」後得到 Success 就代表有成功聯通嚕！

**SETP 3**：在 Messaging API 取得 Linebot 的 LineID，並把他加入好友。

加入好友後，他會跟你打招呼。

但當你很興奮的傳訊息給他時，他的回覆語氣一點都不像是 ChatGPT。

雖然到這裡我們已經知道出事了，但還是先把下面的步驟做完吧。

**SETP 4**：在 Messaging API 關閉自動回覆（Auto-reply messages）與打招呼（Greeting messages）。

因為自動回覆（Auto-reply messages）會干擾 Debug 所以建議關閉，至於打招呼（Greeting messages）要不要關閉就看你自己。

## 測試功能＆痛苦的 Debug 之路

上面 Webhook URL 的 Verify 得到 200 只是證明這條路有打通，如果把畫面切到 ngrok，會發現許多 500 的錯誤訊息。

### ➤ 上一篇 ChatGPT 給的程式有哪些錯誤？

回到程式，你會發現有「TypeError: Cannot read properties of undefined (reading ‘events’)」的錯誤。

如果你把這個問題拿去詢問 ChatGPT，會收到一大堆鬼打牆的回覆。

這邊筆者就直接告訴大家為什麼會有各式各樣的錯誤：

- 「@line/bot-sdk」套件使用方式錯誤
- 「openai」套件使用方式錯誤

你沒看錯，要實作的兩個套件，ChatGPT 的使用方式都是有問題的。

### ➤ 讓 ChatGPT 擔任程式專家，改用英文詢問

為了避免冤枉好人，我這次改成用英文詢問，並讓 ChatGPT 擔任程式專家來撰寫，這是他給出的新回覆：

```
const line = require('@line/bot-sdk');
const express = require('express');
const openai = require('openai');

openai.apiKey = "YOUR_API_KEY";

const config = {
  channelAccessToken: 'YOUR_CHANNEL_ACCESS_TOKEN',
  channelSecret: 'YOUR_CHANNEL_SECRET'
};

const client = new line.Client(config);

const app = express();

app.post('/webhook', line.middleware(config), (req, res) => {
  Promise
    .all(req.body.events.map(handleEvent))
    .then((result) => res.json(result))
    .catch((err) => {
      console.error(err);
      res.status(500).end();
    });
});

function handleEvent (event) {
  if (event.type !== 'message' || event.message.type !== 'text') {
    return Promise.resolve(null);
  }

  return openai
    .engines
    .completions
    .create({
      prompt: event.message.text,
      engine: "text-davinci-002",
      maxTokens: 100
    })
    .then((completions) => {
      const message = completions.choices[0].text;
      return client.replyMessage(event.replyToken, {
        type: 'text',
        text: message
      });
    });
}

app.listen(3000);
console.log('Linebot is running on 3000 port');

```

果然專家就是比較厲害，錯誤訊息變得不一樣了！

但這個錯誤並沒有辦法靠詢問 ChatGPT 解決（他會再次鬼打牆）。

### ➤ 好吧…最後還是得靠自己處理

因為我看錯誤的源頭出在 openai 套件，所以我就去官網研究了一下：[https://www.npmjs.com/package/openai](https://www.npmjs.com/package/openai)

先給大家看一下官網給的範例程式：

```
const { Configuration, OpenAIApi } = require("openai");

const configuration = new Configuration({
  apiKey: process.env.OPENAI_API_KEY,
});
const openai = new OpenAIApi(configuration);

const completion = await openai.createCompletion({
  model: "text-davinci-002",
  prompt: "Hello world",
});
console.log(completion.data.choices[0].text);

```

**如果把他跟 ChatGPT 給的程式做比對，會發現 ChatGPT 的使用方式根本是錯的**，下面筆者就直接把修改好的程式放上來，調整過的部分有在旁邊用註解說明：

```
require('dotenv').config(); // 皆採用 .env 讀取
const line = require('@line/bot-sdk');
const express = require('express');
const { Configuration, OpenAIApi } = require("openai"); // 他套件使用方式錯誤，我直接改掉
const configuration = new Configuration({
  apiKey: process.env.OPENAI_API_KEY,
});
const openai = new OpenAIApi(configuration);

const config = {
  channelAccessToken: process.env.CHANNEL_ACCESS_TOKEN,
  channelSecret: process.env.CHANNEL_SECRET,
};

const client = new line.Client(config);

const app = express();

app.post('/webhook', line.middleware(config), (req, res) => {
  Promise
    .all(req.body.events.map(handleEvent))
    .then((result) => res.json(result))
    .catch((err) => {
      console.error(err);
      res.status(500).end();
    });
});

function handleEvent (event) {
  if (event.type !== 'message' || event.message.type !== 'text') {
    return Promise.resolve(null);
  }

  return openai.createCompletion({ // 方法用錯修正
    prompt: event.message.text,
    model: "text-davinci-003", // 使用 model 而非 engine
    max_tokens: 1000 // 應用 max_tokens 而非 maxTokens
  }).then((completions) => {
    // 少了 data 這層結構，並要加上 trim() 去除空白
    const message = completions.data.choices[0].text.trim();
    return client.replyMessage(event.replyToken, {
      type: 'text',
      text: message
    });
  });
}

app.listen(3000);
console.log('Linebot is running on 3000 port');

```

最後再給大家看看順利溝通時的畫面：

看到成功溝通的畫面時，筆者真的感動到快哭了，這是個能與你溝通的 AI 機器人喔！

不過操作下來我覺得回答穩定度沒有比 ChatGPT 高，有時會出現一些奇怪的程式碼，讀者有興趣可以再做優化，本篇文章僅為參考範例。

## 結論：給想用 ChatGPT 寫程式、學程式的朋友一些建議

可能是我運氣比較好，剛好測試到 ChatGPT 的許多 Bug，下面筆者彙整了一些遇到的「坑」給讀者參考：

- **ChatGPT 沒有最新資料**：筆者在使用 ChatGPT 時，他的資料是在 2021 年截止的，也就是說他不認識新推出的套件，當套件有更新時他也無法第一時間知道。
- **有些問題 ChatGPT 真的回答不出來**：如果是程式邏輯問題，ChatGPT 有機會給出解答，但如果是套件相關問題，可能真的給不出解答；你一直問他也只是鬼打牆。
- **似是而非的答案最可怕**：ChatGPT 在上一篇（中文）跟這一篇（英文）給的答案，如果只看「邏輯」，那是正確的；但實際跑起來卻有一堆問題，不去看官方網站查證真的無法解決。

會有上述問題，我覺得是因為 ChatGPT 還在學習、以及資料不夠即時；相信在未來推出新版時，能給出更正確的解答。

至於如何透過 ChatGPT 學程式，我的建議如下：

- 要學習的**程式語言、工具要有一定的年紀**。
- 使用套件遇到錯誤時，**先去官網看說明文件**。
- 如果完全沒基礎，**強烈建議先從書籍、課程開始學習**。

目前筆者把 ChatGPT 的定位放在「輔助」，適合給你方向、關鍵字、優化的建議；像是這次使用下來，雖然程式有些許錯誤，但是在「步驟、關鍵字、指令」方面的建議都是很不錯的。

未來筆者會出一系列使用 ChatGPT、openAI 的文章，如果你對這個議題感興趣，可以持續關注我的文章！

延伸閱讀：[讓我們一起掌握 AI 帶來的紅利](https://dean-lin.medium.com/list/ddcc9c53a4ac)

**想將 AI 導入自己的職場與生活嗎？歡迎[購買筆者出版的新書](https://www.books.com.tw/products/0011004216)**，幫助您**在 AI 的浪潮中站穩腳步，並在未來的職場競爭中佔據優勢！**

  [https://www.books.com.tw/products/0011004216](https://www.books.com.tw/products/0011004216)

**參考資料**：

- 用英文溝通 ChatGPT 得到的回覆：[Github](https://github.com/dean9703111/linebot-openai-chatgpt/blob/master/bug2-index.js)
- 筆者自己優化得到的解答：[Github](https://github.com/dean9703111/linebot-openai-chatgpt/blob/master/index.js)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202301/chatgpt-line-bot-side-project-part-2/
