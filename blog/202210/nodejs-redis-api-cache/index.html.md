# 在 Node.js 使用 Redis 來 Cache API 的回傳資料，以減少查詢資料的回傳時間

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202210/nodejs-redis-api-cache/
- 發布：2022-10-19（更新：2023-05-05）
- 主題：技術教學
- 標籤：Redis、Node.js、Performance、Cache API、Koa.js
- 摘要：現在使用者對網站的要求越來越高，即使你的內容做得很棒，但如果頁面載入時間超過 3 秒，使用者可能在被你的內容吸引前就離開了網站。如果你做的是電子商務系統，這個「痛點」會更為明顯，一但讓客戶等太久，公司就會流失大筆訂單…

現在使用者對網站的要求越來越高，即使你的內容做得很棒，但如果頁面載入時間超過 3 秒，使用者可能在被你的內容吸引前就離開了網站。

如果你做的是電子商務系統，這個「痛點」會更為明顯，一但讓客戶等太久，公司就會流失大筆訂單。

為了解決這個問題，我們可以將資料設計成「快取」，這樣不但能大幅減少查詢資料的回傳時間（毫秒級別），還能有效降低後端資料庫的存取壓力。

本篇文章將以：「Node.js X Koa X Redis」作為範例解說；如果你完全沒接觸過 Redis，可以先參考[這篇文章](https://deanlin.net/blog/202108/install-redis-on-macos/)安裝環境。

## 應用場景說明

如果今天要設計一款「等公車」的服務，那你勢必要串接「第三方 API」來獲取即時訊息。

但有時第三方 API 的回傳時間並不理想，甚至有存取次數的限制；如果我們想降低第三方 API 存取次數，並減少使用者等待的回傳時間，我們可以針對資料做「快取（Cache）」設計。

等公車這個服務有幾個特點：

- 「公車到站時間」對所有使用者來說都是一樣的。
- 公車即將抵達的時間以「分鐘」來顯示，所以我們存取的第三方 API 的頻率不用太高。
- 公車的線路非常多，但不會每條線路都有剛好使用者在查。

我們可以根據特點來設計資料更新的方案：

- 我們並不需要輪詢所有的路線，因為很多資料我們就算取得，也沒有使用者會需要。
- 因為即將抵達的時間以「分鐘」顯示，所以我們只要把更新的頻率設計在 1 分鐘以下即可（ex：5 秒鐘）。
- 更新的策略可以採取「若 Redis 有資料就直接採用，若無則呼叫第三方 API 獲取資料」。

筆者之前有分享過用 Redis 解決「商品秒殺」的應用，有興趣的讀者可以參考[這篇文章](https://deanlin.net/blog/202108/nodejs-redis-flash-sale-concurrency/)。

## 專案環境設置

這個小專案我們會使用到如下套件：

- koa：可以理解為 Express.js 的精簡版後端框架。
- @koa/router：路由器設定用的，看起來會比較直觀。
- ioredis：與 Redis 連線、溝通的套件。
- axios：HTTP 請求工具，我們透過它來 Call API。

使用如下指令便可「建立專案資料夾＆初始化專案＆安裝套件」：

```
mkdir redis-cache-example
cd redis-cache-example
npm init -y
npm install --save koa @koa/router ioredis axios

```

## 找個方便使用的第三方 API

本專案的主要目的，是讓大家理解如何將 API 回傳的資料快取。

所以這邊筆者選擇[JSONPlaceholder](https://jsonplaceholder.typicode.com/)的服務，他有提供虛擬的 RESTful API（重點是不用申請 Key 方便很多）。

  [https://jsonplaceholder.typicode.com/todos](https://jsonplaceholder.typicode.com/todos)

上圖是 API 的回傳格式，大家可以看到，每個物件都有「id」的欄位，我們的程式會使用到這個參數來做 Demo（最多只有到 200，超過會回傳錯誤訊息）。

## 專案程式

本篇文章的目的是讓讀者理解 Redis 對效能有多大的提升，因此程式架構超級簡單，大概分成三個部分：

- 將相關套件引入（koa、@koa/router、ioredis、axios）
- Redis 資料庫連線
- Get Method 的 RESTful API

而在 Get Method 存取 Redis 資料庫的邏輯為：

- 先去 Redis 確認是否有快取，如果有就直接回傳。
- 如果 Redis 沒資料，就要 Call API，並將第一次獲得的資料儲存到 Redis 裡面（資料過期時間設定為 5 秒鐘）。

為了方便讀者實作，筆者下面直接提供完整程式，你只要在專案資料夾下新增 `index.js`，然後複製貼上程式即可：

```
const Koa = require('koa');
const Router = require('@koa/router');
const Redis = require('ioredis');
const axios = require('axios')
const app = new Koa();
const router = new Router();

const redis = new Redis({
  host: '127.0.0.1',
  port: 6379,
  password: ''
});
redis.on("error", function (error) {
  console.error(error);
});

router.get('/todos/:id', async ctx => {
  try {
    const cacheTodos = await redis.get(`todos:${ctx.params.id}`);
    if (cacheTodos) {
      console.log("從 Redis 獲得資料");
      ctx.body = JSON.parse(cacheTodos);
      return;
    }

    // 呼叫 API
    const response = await axios.get(`https://jsonplaceholder.typicode.com/todos/${ctx.params.id}`);
    console.log("將第一次獲得的資料儲存到 Redis");
    // 把 API 回傳結果放入 Redis，並設定 5 秒的過期時間（Expire Time）
    await redis.set(`todos:${ctx.params.id}`, JSON.stringify(response.data), "EX", 5);

    ctx.body = response.data;
  } catch (err) {
    ctx.body = err;
  }
});

app.use(router.routes());
app.listen(3000);

```

原始碼：[redis-cache.js](https://gist.github.com/dean9703111/c3cc905908e578703631d482641106cc)（GitHub Gist）

## 測試程式，確認結果是否符合預期

STEP 1：先在終端機輸入 `redis-server`，啟動 Redis Server。

STEP 2：輸入 `node index.js` 運行專案。

STEP 3：打開 POSTMAN，實際體驗有了 Redis Cache API 的資料後，減少了多少回傳時間。

  第一次存取 API，需要等待「626ms」才會回傳。

  第二次因為資料已經儲存到 Redis，我們等待的時間縮減為「4ms」，是毫秒級的回傳！

如果想感受差異，可以看看下面筆者錄製的 Gif，裡面也有呈現過 5 秒就要重新抓取資料的邏輯。

## 結語

本篇文章的重點在於「Cache 的思路」，Redis 只是協助我們達成目標的工具，即使改用 Memcached 也能達到相同目的。

希望這個簡單的範例讓大家對 Redis 的應用有更近一步的認識，也歡迎大家留言分享自己的應用喔！

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202210/nodejs-redis-api-cache/
