# [面試][後端]請簡述 Node.js 的 Event Loop

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202109/fullstack-career-notes-day-14/
- 發布：2021-09-29
- 主題：職涯成長
- 系列：全端工程師生存筆記
- 標籤：iT 邦幫忙鐵人賽、後端面試、Node.js、Event Loop、JavaScript
- 摘要：這是一個在了解後，無論面試還是工作，CP 值都很高的題目，建議讀者要真的融會貫通。

> 熟悉的起手式：「我方便問你一個 Node.js 核心的問題嗎？」

這是一個在了解後，無論面試還是工作，CP 值都很高的題目，建議讀者要真的融會貫通。

## 大綱

1. **請簡述 Node.js 的 Event Loop**

2. 1.1 面試官為什麼會問？
3. 1.2 面試官想從答案確認什麼？
4. 1.3 筆者提供的簡答

5. **回答問題所需具備的知識**

6. 2.1 Event Loop 執行的邏輯
7. 2.2 用 Node.js 範例程式確認自己是否理解 Event Loop
8. 2.3 解析 Node.js 範例程式執行順序

9. **衍伸問題**

10. 3.1 可以簡述 Node.js 跟 JavaScript 的差異嗎？
11. 3.2 readFile 和 createReadStream 函式的差異？
12. 3.3 REPL 是什麼？

## 1. 請簡述 Node.js 的 Event Loop

### 1.1 面試官為什麼會問？

只要你的`履歷上有 Node.js`，10 間公司至少有 3 間會問這個常見面試題；而且問題的起手式出奇的一致：「我方便問你一個 Node.js 核心的問題嗎？」

### 1.2 面試官想從答案確認什麼？

- 你了解 Event Loop 對 Node.js 的重要性
- 你能大概說明 Event Loop 每個階段的任務
- 如果出白板題要你解釋程式執行的順序，你可以正確回答

### 1.3 筆者提供的簡答

`Node.js 之所以高效，是因為採取單執行緒與 Event Loop 的概念`；將所有需要等待結果、請求外部資源的函式，全部丟到 Event Loop 中等待；而 Event Loop 的邏輯是 Node.js 底層用 C 語言撰寫的 libuv 庫來運行的。

## 2. 回答問題所需具備的知識

### 2.1 Event Loop 執行的邏輯

- **了解 MacroTask 的 6 個階段**

- **Timer**
等計時器（setTimeout、setInterval）的時間一到，會把他們的 callback 放在這裡等待執行。
- **Pending callbacks**
作業系統層級使用（ex：TCP errors、sockets 連線被拒絕）。
- **Idle, Prepare**
內部使用。
- **Polling：**

- 如果 Queue 不為空，依次取出 callback 函數執行，直到 Queue 為空或是抵達系統最大限制。
- 如果 Queue 為空但有設置「setImmediate」就進入 Check 階段。
- 如果 Queue 為空但沒有設置「setImmediate」就會在 Polling 階段等待，直到 Queue 有東西或是 Timer 時間抵達。

- **Check**
處理 setImmediate 的 callback。
- **Close callbacks**
處理關閉連線、檔案的 callback。

Macrotask Queue 在 Evevnt Loop 優先層級較低，接下來介紹優先層級更高的兩個 Queue。

- **NextTick Queue 優先層級最高**
「process.nextTick()」的 callback 可以隨意插隊；只要這個 Queue 有東西就會優先執行。

- **MicroTask Queue 優先層級次高**
「Promise」的狀態從 pending 轉變為 resolve 或 reject 時，執行的 callback 會進入這個 Queue。

> **注意**：Promise 在創建時是同步的，不會進入 Event Loop。

### 2.2 用 Node.js 範例程式確認自己是否理解 Event Loop

大家可以先用紙筆模擬看看下面程式的執行順序：

```
console.log("start");

process.nextTick(function () {
  console.log("nextTick1");
});

setTimeout(function () {
  console.log("setTimeout");
}, 0);

new Promise(function (resolve, reject) {
  console.log("promise");
  resolve("resolve");
}).then(function (result) {
  console.log(result);
});

(async function () {
  console.log("async");
})();

setImmediate(function () {
  console.log("setImmediate");
});

process.nextTick(function () {
  console.log("nextTick2");
});

console.log("end");

```

如果答案跟下面結果一樣，那代表你很了解 Node.js 的 Event Loop 邏輯：

```
start
promise
async
end
nextTick1
nextTick2
resolve
setTimeout
setImmediate

```

### 2.3 解析 Node.js 範例程式執行順序

- **SETP 1：** 順序執行，先把同步函式解決。

1.
因為 `Promise` 在創建時是同步的，所以會先執行。

2.
在 function 前面加上 async 宣告為異步函式，執行時會被轉換為 Promise。

```
console.log("start");

new Promise(function (resolve, reject) {
  console.log("promise");
  resolve("resolve");
}).then(function (result) {
  console.log(result);
});

(async function () {
  console.log("async");
})();

console.log("end");

```

- **STEP 2：** 進入 Event Loop。

1.
如果執行過程中遇到 `NextTick Queue` 就優先執行。

```
process.nextTick(function () {
  console.log("nextTick1");
});

process.nextTick(function () {
  console.log("nextTick2");
});

```

2.
在處理完最高優先層級後，如果執行過程中遇到 `MicroTask Queue` 就執行。

```
new Promise(function (resolve, reject) {
  console.log("promise");
  resolve("resolve");
}).then(function (result) {
  console.log(result);
});

```

3.
在前兩個高優先層級的 Queue 清空後，返回 Event Loop 繼續執行，`setTimeout` 設定的時間已到先執行。

```
setTimeout(function () {
  console.log("setTimeout");
}, 0);

```

4.
接著再在「Check」 階段執行 `setImmediate`

```
setImmediate(function () {
  console.log("setImmediate");
});

```

以上步驟執行完成後就是範例程式的解答啦～希望對大家理解 Event Loop 有幫助。

## 3. 衍伸問題

### 3.1 可以簡述 Node.js 跟 JavaScript 的差異嗎？

考點：確認求職者是否理解自己天天使用的工具

JavaScript 是程式語言，可以在合適的瀏覽器中運行；而 Node.js 是一個能執行JavaScript 的環境，它以 Chrome V8 引擎為核心，再加上 C/C++套件，讓 Sever 端也可以執行 JavaScript。

### 3.2 readFile 和 createReadStream 函式的差異？

考點：判斷求職者過去專案中使否使用過這類函式，並理解差異

`readFile` 函式會將讀取到的完整內容存在記憶體後傳給使用者，而 `createReadStream` 函式則是逐塊讀取檔案，不是全部存在記憶體中；因此要讀取較大的檔案時，建議使用 createReadStream ，利用其逐塊讀取的特性能減少使用者等待時間。

### 3.3 REPL 是什麼？

考點：對專有名詞是否熟悉，能否解釋

REPL（Read Eval Print Loop）是一個用來執行程式語言的虛擬環境。

就像是我們在瀏覽器的 Console 可以執行 JavaScript 程式碼；在安裝完 Node.js 後，新增一個 JavaScript 檔，在終端機輸入`node xxx.js`便可以執行它。

感謝大家的閱讀，如果喜歡我的文章可以`訂閱`接收通知；如果有幫助到你，按`Like`可以讓我更有寫文的動力，我們明天見～

### 參考資源：

1. [Node.js 的 Event Loop (事件輪詢)到底在做什麼？(筆者部落格)](https://medium.com/dean-lin/c7129063d0f4)
2. [15 個常見的 Node.js 面試問題及答案](https://www.gushiciku.cn/pl/gOTo/zh-tw)
3. [你有沒有想過，到底 Server 是如何「同時處理多個 requests」的？ - Node.js 篇](https://ithelp.ithome.com.tw/articles/10230126)

> 在許多人的幫助下，本系列文章已成功出版，除了添加新的篇章，更完善了每個案例的應對進退；如果對現在的職涯感到迷茫，也許這本書能帶給你不一樣的觀點～
> **天瓏書局：** [https://www.tenlong.com.tw/products/9786263334571](https://www.tenlong.com.tw/products/9786263334571)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202109/fullstack-career-notes-day-14/
