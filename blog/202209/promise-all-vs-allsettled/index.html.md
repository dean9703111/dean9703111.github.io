# JavaScript 如何讓多個 async/await 函式同時執行──  從實際案例了解 Promise.all 和 Promise.allSettled 的區別

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202209/promise-all-vs-allsettled/
- 發布：2022-09-19（更新：2023-05-05）
- 主題：技術教學
- 標籤：Await、Promise All、Parallel Processing、非同步函式、同步函式
- 摘要：撰寫前端時，一個頁面往往需要 call 多支 API 才能完成渲染。為了等資料回傳，我們通常都會搭配 await，但有時 API 之間並沒有要求執行的順序；在這樣的情境下，如果還是採取順序執行的方案，當需要 call 多支 API 時就會浪費使用者許多時間。

大家可以在瀏覽器按下「F12」，於 Console 的分頁貼上文中的程式碼來驗證喔！

撰寫前端時，一個頁面往往需要 call 多支 API 才能完成渲染。

為了等資料回傳，我們通常都會搭配 await，但有時 API 之間並沒有要求執行的順序（下一隻 API 並不需要使用上一隻 API 回傳的結果作為參數）。

在這樣的情境下，如果還是採取順序執行的方案，當需要 call 多支 API 時就會浪費使用者許多時間。下面筆者就先舉一個直觀的範例：

```
const waitFor = delay => new Promise(resolve => setTimeout(resolve, delay));

async function api1 () {
    await waitFor(500);
    return "A";
}
async function api2 () {
    await waitFor(500);
    return "B";
}
async function api3 () {
    await waitFor(500);
    return "C";
}

async function callMutiApi () {
    console.time();
    const a = await api1()
    const b = await api2()
    const c = await api3()
    console.log({ a, b, c });
    console.timeEnd();
}
callMutiApi();

```

原始碼：[await-basic.js](https://gist.github.com/dean9703111/96bab87fbec6f71ab71c28a334cd14cb)（GitHub Gist）

  在瀏覽器 Console 分頁的執行結果

在上圖中，我們可以看到總執行時長為 1512 ms，有許多時間被浪費在等待上一支 API 執行完畢。

## 採用 Promise.all 來解決問題

既然 API 之間沒有要求執行的順序，我們就可以將其改寫為 `Promise.all`，讓函式同時執行來提升效率：

```
const waitFor = delay => new Promise(resolve => setTimeout(resolve, delay));

async function api1 () {
    await waitFor(500);
    return "A";
}
async function api2 () {
    await waitFor(500);
    return "B";
}
async function api3 () {
    await waitFor(500);
    return "C";
}

async function callMutiApi () {
    console.time();
    const promises = [
        api1(),
        api2(),
        api3(),
    ];
    Promise.all(promises).then(values => {
        console.log(values);
        console.timeEnd();
    });
}
callMutiApi();

```

原始碼：[promise-all.js](https://gist.github.com/dean9703111/74b5067420a84b845ad7fec8c7f415b2)（GitHub Gist）

  在瀏覽器 Console 分頁的執行結果

經過改寫，我們可以從輸出的結果中看到，總執行時長從 `1512 ms` 下降到 `502 ms`，這對使用者來說是非常有感的提升。

## Promise.all 會碰到什麼問題？

儘管透過 `Promise.all` 已經提升了不少效率，但這個方案在其中一支 API 吐 error 時，情況就會變得很麻煩。

```
const waitFor = delay => new Promise(resolve => setTimeout(resolve, delay));

async function api1 () {
    await waitFor(500);
    return "A";
}
async function api2 () {
    await waitFor(500);
    return "B";
}
async function api3 () {
    await waitFor(500);
    throw "error"; // 丟出錯誤訊息
}

async function callMutiApi () {
    console.time();
    const promises = [
        api1(),
        api2(),
        api3(),
    ];
    Promise.all(promises)
        .then(values => console.log(values))
        .catch(err => console.log(err))
        .finally(() => console.timeEnd())
}
callMutiApi();

```

原始碼：[promise-all-err.js](https://gist.github.com/dean9703111/f486fa060c62e1f35136ec0f23e26900)（GitHub Gist）

如果執行上面的程式，你會發現透過 catch **我們成功抓到 “error” 的錯誤訊息，但其他執行成功的訊息全部都消失了（**`Promise.all`**的失敗優先原則）**。

有時這種「只有成功 or 失敗」的結果的確是我們要的，但在實務上，我們未必需要所有 API 都執行成功，面對錯誤我們可以設計另一套處理方案。

## 導入 Promise.allSettled 解決問題

為了取得所有回傳結果，我們可以改為使用 `Promise.allSettled`，在輸出的時候，它會多出一個 `status` 的參數，用來說明執行結果的成功 or 失敗。

```
const waitFor = delay => new Promise(resolve => setTimeout(resolve, delay));

async function api1 () {
    await waitFor(500);
    return "A";
}
async function api2 () {
    await waitFor(500);
    return "B";
}
async function api3 () {
    await waitFor(500);
    throw "error"; // 丟出錯誤訊息
}

async function callMutiApi () {
    console.time();
    const promises = [
        api1(),
        api2(),
        api3(),
    ];
    Promise.allSettled(promises).then(values => {
        console.log(values);
        console.timeEnd();
    });
}
callMutiApi();

```

原始碼：[promise-allSettled.js](https://gist.github.com/dean9703111/7d76243df020caf8f2b9e24649ed77da)（GitHub Gist）

  在瀏覽器 Console 分頁的執行結果

從上圖我們可以看到，無論執行的結果是成功或失敗，我們都能取得那些執行成功後的訊息。

**備註： **Node.js 從 v12.9.0 開始支援 Promise.allSettled 的語法，大多數的瀏覽器也在 2019 年後支援；針對少數不支援的環境，你可以透過 npm 套件自行擴充，如：[promise.allsettled](https://www.npmjs.com/package/promise.allsettled)

## 總結

- 如果 API 沒有要求執行的順序，但需要等每個 API 都執行完畢後才能進入下一步，那 Promise.all 同時執行的機制可以有效縮短執行時間。
- 為了解決 Promise.all 失敗後，成功訊息一併消失的問題，可以改用 Promise.allSettled 來取得所有回傳結果。

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202209/promise-all-vs-allsettled/
