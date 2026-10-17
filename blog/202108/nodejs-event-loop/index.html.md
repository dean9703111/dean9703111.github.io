# Node.js 的 Event Loop (事件輪詢)到底在做什麼？

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202108/nodejs-event-loop/
- 發布：2021-08-10（更新：2023-05-05）
- 主題：技術教學
- 標籤：Node.js、Event Loop、Node.js Event Loop、事件輪詢
- 摘要：因為網路上的教學文我大部分都有看沒有懂，只好自己實作一遍來了解他的運作機制；希望我這篇文章足夠口語化，讓讀者能輕鬆理解，如果有解釋錯誤或是不夠清晰的部分再煩請告知，謝謝。

因為網路上的教學文我大部分都有看沒有懂，只好自己實作一遍來了解他的運作機制；希望我這篇文章足夠口語化，讓讀者能輕鬆理解，如果有解釋錯誤或是不夠清晰的部分再煩請告知，謝謝。

```
大綱
一、請用一段話簡述 Node.js 的 Event Loop
二、Node.js 在 Event Loop 邏輯下的執行順序
三、Event Loop 執行的邏輯
  ➤ 了解 MacroTask 的 6 個階段
  ➤ NextTick Queue 優先層級最高
  ➤ MicroTask Queue 優先層級次高
四、解析一開始 Node.js 範例程式執行順序
  ➤ 順序執行，先把同步函式解決
  ➤ 進入 Event Loop

```

## 一、請用一段話簡述 Node.js 的 Event Loop

「Node.js 之所以高效，是因為採取單執行緒與 Event Loop 的概念；將所有需要等待結果、請求外部資源的函式，全部丟到 Event Loop 中等待；而 Event Loop 的邏輯是 Node.js 底層用 C 語言撰寫的 libuv 庫來運行的。」

有興趣的朋友可以來 GitHub 上看 libuv 的程式：

- 前往 libuv 的 [GitHub](https://github.com/libuv/libuv)
- 看運作的核心程式碼「[core.c](https://github.com/libuv/libuv/blob/v1.x/src/unix/core.c)」
- 搜尋「uv_run」，直接看最原本的邏輯

## 二、Node.js 在 Event Loop 邏輯下的執行順序

大家可以先用大腦模擬看看下面程式的執行順序：

```
console.log('start');

process.nextTick(function () {
    console.log('nextTick1');
});

setTimeout(function () {
    console.log('setTimeout');
}, 0);

new Promise(function (resolve, reject) {
    console.log('promise');
    resolve('resolve');
}).then(function (result) {
    console.log(result);
});

(async function () {
    console.log('async');
})();

setImmediate(function () {
    console.log('setImmediate');
});

process.nextTick(function () {
    console.log('nextTick2');
});

console.log('end');

```

原始碼：[event_loop.js](https://gist.github.com/dean9703111/a8e641d53e5e81d7831b596a2e47af72)（GitHub Gist）

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

## 三、Event Loop 執行的邏輯

  Node.js 的 Event Loop

### ➤ 了解 MacroTask 的 6 個階段

**Timer**：等計時器（setTimeout、setInterval）的時間一到，會把他們的 callback 放在這裡等待執行。

**Pending callbacks**：作業系統層級使用（TCP errors、sockets 連線被拒絕）。

**Idle, Prepare**：內部使用。

**Polling**：

- 如果 Queue 不為空，依次取出 callback 函數執行，直到 Queue 為空或是抵達系統最大限制。
- 如果 Queue 為空但有設置「setImmediate」就進入 check 階段。
- 如果 Queue 為空但沒有設置「setImmediate」就會在 Polling 階段等待直到 Queue 有東西或是 Timer 時間抵達。

**Check**：處理 setImmediate 的 callback。

**Close callbacks**：處理關閉連線、檔案的 callback。

Macrotask Queue 在 Evevnt Loop 優先層級較低，接下來介紹優先層級更高的兩個 Queue。

### ➤ NextTick Queue 優先層級最高

「process.nextTick()」的 callback 可以隨意插隊；只要這個 Queue 有東西就會優先執行。

### ➤ MicroTask Queue 優先層級次高

「Promise 」的狀態從 pending 轉變為 resolve 或 reject 時，此時執行的 callback 會進入這個 Queue。

注意：Promise 在創建時是同步的，不會進入 Event Loop。

## 四、解析一開始 Node.js 範例程式執行順序

### ➤ 順序執行，先把同步函式解決

- 因為 Promise 在創建時是同步的，所以會先執行
- 在 function 前面加上 async 宣告為異步函式，執行時會被轉換為 Promise

```
console.log('start');

new Promise(function (resolve, reject) {
    console.log('promise');
    resolve('resolve');
}).then(function (result) {
    console.log(result);
});

(async function () {
    console.log('async');
})();

console.log('end');

```

原始碼：[sync_function.js](https://gist.github.com/dean9703111/44406b76d559e2183b9514e290da2f08)（GitHub Gist）

```
start
promise
async
end

```

### ➤ 進入 Event Loop

- 如果執行過程中遇到 NextTick Queue 就優先執行

```
process.nextTick(function () {
    console.log('nextTick1');
});

process.nextTick(function () {
    console.log('nextTick2');
});

```

原始碼：[nextTick_queue.js](https://gist.github.com/dean9703111/5eaffaea4c9d482ffb36a111fbf1fd2f)（GitHub Gist）

```
nextTick1
nextTick2

```

2. 在處理完最高優先層級後，如果執行過程中遇到 MicroTask Queue 就執行

```
new Promise(function (resolve, reject) {
    console.log('promise');
    resolve('resolve');
}).then(function (result) {
    console.log(result);
});

```

原始碼：[MicroTask_queue.js](https://gist.github.com/dean9703111/d5a22e3600e7d0334cb213051bc8ca40)（GitHub Gist）

```
resolve

```

3. 在前兩個高優先層級的 Queue 清空後，返回 Event Loop 繼續執行，setTimeout 設定的時間已到先執行。

```
setTimeout(function () {
    console.log('setTimeout');
}, 0);

```

原始碼：[setTimeout_function.js](https://gist.github.com/dean9703111/cdcdddce3fd03f545e6f159956cf9c7f)（GitHub Gist）

```
setTimeout

```

4. 接著再在「Check」 階段執行 setImmediate

```
setImmediate(function () {
    console.log('setImmediate');
});

```

原始碼：[setImmediate_function.js](https://gist.github.com/dean9703111/5cba974b22f28334f0b21e268081e3dc)（GitHub Gist）

```
setImmediate

```

以上步驟執行完成後就是範例程式的解答啦～希望對大家理解 Event Loop 有幫助。

**參考資源：**

- [完整圖解Node.js的Event Loop(事件迴圈)](https://notes.andywu.tw/2020/%E5%AE%8C%E6%95%B4%E5%9C%96%E8%A7%A3node-js%E7%9A%84event-loop%E4%BA%8B%E4%BB%B6%E8%BF%B4%E5%9C%88/)
- [15 尚硅谷 前端面试题 nodejs事件轮询机制](https://www.youtube.com/watch?v=yHnlOAKYxQU&ab_channel=%E6%9D%8E%E7%9A%93%E8%AF%9A)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202108/nodejs-event-loop/
