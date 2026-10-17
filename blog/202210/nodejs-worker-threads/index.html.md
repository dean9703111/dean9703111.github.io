# Node.js + worker_threads 多執行緒實戰範例

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202210/nodejs-worker-threads/
- 發布：2022-10-20
- 主題：技術教學
- 標籤：Node.js、Multithreading、多執行緒、Worker Threads、吞吐量
- 摘要：一般狀況下 Node.js 為單執行緒，如果同步執行時遇到「阻塞」的程式，後續要執行的程式就會全部被卡住；為了解決這個問題， Node.js 在 v12 推出了 worker_threads，這個 Module 讓 Node.js 可以做多執行緒的平行處理！

一般狀況下 Node.js 為單執行緒，如果同步執行時遇到「阻塞」的程式，後續要執行的程式就會全部被卡住；為了解決這個問題， Node.js 在 v12 推出了 `worker_threads`，這個 Module 讓 Node.js 可以做多執行緒的平行處理！

> 今天這篇文章會採用 Demo 的形式，讓大家理解程式運作的順序，而「阻塞」的程式則是選用 `斐波那契數列（fibonacci sequence）` 來模擬。

## 單執行緒會遇到什麼問題

單執行緒如果遇上需要高 CPU 運算的程式，那整體效能就會大幅下降；下面我們用一個簡單的程式來做說明：

```
function getFibonacciNumber (num) {
  if (num === 0) {
    return 0;
  } else if (num === 1) {
    return 1;
  } else {
    return getFibonacciNumber(num - 1) + getFibonacciNumber(num - 2);
  }
}
console.log(`42th Fibonacci Result:` + getFibonacciNumber(42));
console.log(`15th Fibonacci Result:` + getFibonacciNumber(15));
console.log("被卡住的輕量級任務");

```

原始碼：[single-thread.js](https://gist.github.com/dean9703111/ef1bc467983a03bb631a90dcdd5a8d54)（GitHub Gist）

上述程式的執行結果為：

```
42th Fibonacci Result:267914296
15th Fibonacci Result:610
被卡住的輕量級任務

```

你可以將程式中的 3 個 console.log 理解為：

1. **42th Fibonacci**：會讓程式阻塞的重量級運算。
2. **15th Fibonacci**：運算需要的時間不長，但被上面的程式卡住了。
3. **被卡住的輕量級任務**：只是簡單的輸出，卻被上面的程式卡的動彈不得。

## 多執行緒如何解決

既然知道 `斐波那契數列（fibonacci sequence）` 是阻塞的元兇，那我們可以把這個運算抽離出來，給新的執行緒來計算。

在實作方面，先新增一個 `worker.js` 的檔案，讓他處理由「主執行緒」指派的「新執行緒」；這裡的執行緒會透過 `parentPort.postMessage` 函數與「主執行緒」傳遞訊息。

```
const { parentPort, workerData } = require("worker_threads");

parentPort.postMessage(getFibonacciNumber(workerData.num))

function getFibonacciNumber (num) {
  if (num === 0) {
    return 0;
  } else if (num === 1) {
    return 1;
  } else {
    return getFibonacciNumber(num - 1) + getFibonacciNumber(num - 2);
  }
}

```

原始碼：[worker.js](https://gist.github.com/dean9703111/15577f1cfec9a82f3c25344f7acaa3df)（GitHub Gist）

接著我們新增一個 `index.js` 的檔案，來設計主執行緒的程式，筆者在這新增一個「runWorker」的函式，將會造成「阻塞」的程式交給它來執行。

```
const { Worker } = require("worker_threads");

const runWorker = (number) => {
  const worker = new Worker("./worker.js", { workerData: { num: number } });
  worker.on("message", result => {
    console.log(`${number}th Fibonacci Result: ${result}`);
  });

  worker.on("error", error => {
    console.log(error);
  });

  worker.on("exit", exitCode => {
    console.log(`It exited with code ${exitCode}`);
  })
}

runWorker(42)
runWorker(15)
console.log("主執行緒的任務");

```

原始碼：[worker-threads.js](https://gist.github.com/dean9703111/00e22524c68601f108dbe1630f7b1534)（GitHub Gist）

經過這樣的調整後：

1. 繁重的運算由「新執行緒」處理，且不會互相阻塞；解決了 Fibonacci 在「15th」的運算明明很快，但於單執行緒卻被「42th」拖到後面才執行的問題。
2. 從下圖的運算結果，你可以觀察到主執行緒不會被阻塞。

## 結語

過去 Node.js 常常被說不適合執行高 CPU 運算的任務（因為會阻塞）。

但在導入 `worker_threads` 使其可以運用多執行緒後，我們只要將「高 CPU 運算」的任務交給其他執行緒處理，主執行緒就能繼續處理其他任務不被阻塞，以此提高 Server 的吞吐量。

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202210/nodejs-worker-threads/
