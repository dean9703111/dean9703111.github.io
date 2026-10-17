# 6 種不可不知的「console.log」進階應用

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202209/advanced-console-log/
- 發布：2022-09-09（更新：2023-05-05）
- 主題：技術教學
- 標籤：Console Log、Console、Console Time、Front End Development、JavaScript Development
- 摘要：相信 console.log 是 Frontend 開發人員的好朋友，我們用它印出各種輔助資訊來解決問題。這邊文章會跟你分享 6 種 console.log 的進階應用，並說明各自的使用情境，讓你在開發路上更加順利。

相信 console.log 是 Frontend 開發人員的好朋友，我們用它印出各種輔助資訊來解決問題。

這邊文章會跟你分享 6 種 console.log 的進階應用，並說明各自的使用情境，讓你在開發的路上更加順利。

大家可以在瀏覽器按下「F12」，於 Console 的分頁貼上文中的程式碼來驗證喔！

```
大鋼
ㄧ、在 console.log 加入 {}，讓資訊顯示的更友善
二、原來 console.log 竟然可以使用 CSS Style !
三、console.log 寫太多找不到重點？你該試試看 console.warn()！
四、錯誤訊息請用 console.error()，不要用 console.log() 來整自己
五、用 console.time() 追蹤程式執行時間，找出效能瓶頸
六、用 console.table() 將資訊轉成表格來閱讀

```

## ㄧ、在 console.log 加入 {}，讓資訊顯示的更友善

如果想要將變數的內容印出來，我們通常會這麼做：

```
const name = "Dean";
const book = "給全端工程師的職涯生存筆記";

console.log(name, book);

```

原始碼：[console-basic.js](https://gist.github.com/dean9703111/e917d4286b1ccfb52cb27b14a997050f)（GitHub Gist）

但這種作法在變數很多的時候，你就會搞不清楚各自對應的關係了，比如：

```
const name = "Dean";
const book = "給全端工程師的職涯生存筆記";
const skill = "Full-Stack";
const type = "Career";

console.log(name, book, skill, type);

```

原始碼：[console-complex.js](https://gist.github.com/dean9703111/e29960572a643720eb49ec7e04d5f706)（GitHub Gist）

有沒有覺得上面的資訊閱讀起來很不友善？其實我們只要在 console.log 中，**把這些變數用 {} 包起來**，它就會以物件的類型來顯示嚕！

```
const name = "Dean";
const book = "給全端工程師的職涯生存筆記";
const skill = "Full-Stack";
const type = "Career";

console.log({ name, book, skill, type });

```

原始碼：[console-object.js](https://gist.github.com/dean9703111/365d5c816d7aed5bd299e83a9b33378b)（GitHub Gist）

## 二、原來 console.log 竟然可以使用 CSS Style !

如果你在 Facebook 的頁面按下「F12」，於 Console 的分頁中應該會看到如下警告：

當時我就很好奇這種特殊的 Style 是怎麼呈現的，後來發現只要在前面加上「%c」就可以了：

```
const text = "大寶寶";
const style = "color: blue;font-size:40px";

console.log(`使用 CSS Style %c${text}`, style);

```

原始碼：[console-css.js](https://gist.github.com/dean9703111/f38fbea3fcb0e1847b6d96f82f15a172)（GitHub Gist）

## 三、console.log 寫太多找不到重點？你該試試看 console.warn()！

遇到 Bug 時，我們常常需要沿著線索下 console.log 找問題，但如果 console.log 寫太多，我們會無法找到哪個才是我們需要的資訊，下面就舉一個極端點的例子：

```
for (let i = 0; i < 30; i++) {
  if (i === 13) {
    console.log(`i ---: ${i}`);
  } else {
    console.log(`i -+-: ${i}`);
  }
}

```

原始碼：[console-no-warn.js](https://gist.github.com/dean9703111/0cfe7cea92a72819ca67e15950cbd0db)（GitHub Gist）

我相信大多數人第一眼是不會發現「i === 13」時印出的東西不同，但如果透過 console.warn() 來輔助就會一目瞭然：

```
for (let i = 0; i < 30; i++) {
  if (i === 13) {
    console.warn(`i ---: ${i}`);
  } else {
    console.log(`i -+-: ${i}`);
  }
}

```

原始碼：[console-warn.js](https://gist.github.com/dean9703111/fb17de3c628059d2897af68f5c86998d)（GitHub Gist）

## 四、錯誤訊息請用 console.error()，不要用 console.log() 來整自己

筆者發現有些工程師在 try…catch 的撰寫上，是用 console.log() 來印錯誤訊息的 😅

這會導致錯誤訊息被其他 console.log() 的訊息淹沒，運氣不好就是系統出包後才發現，因此建議乖乖使用 console.error() 來印錯誤訊息，這樣自己比較方便 Debug。

```
try {
  // 直接拋錯誤訊息讓 catch 來接
  throw "Something went wrong.";
} catch (err) {
  console.error(err);
}

```

原始碼：[console-error.js](https://gist.github.com/dean9703111/6571d256c3d3c2a402961407e40e8b17)（GitHub Gist）

## 五、用 console.time() 追蹤程式執行時間，找出效能瓶頸

有時我們發現某些頁面跑的特別慢，為了確定問題到底出哪裡，「console.time() & console.timeEnd()」就是一個很好的方案：

```
console.time();

setTimeout(function () {
  console.timeEnd();
}, 2000);

```

原始碼：[console-time.js](https://gist.github.com/dean9703111/fa9d05d945efc7b1eb4fbba650523d03)（GitHub Gist）

但一個功能裡面可能用到多個 api，當你想釐清到底是哪個 api 效率有問題時，就可以給 console.time() 加上標記(flag)。

```
console.time("api-1");

setTimeout(function () {
  console.timeEnd("api-1");
}, 2000);

console.time("api-2");

setTimeout(function () {
  console.timeEnd("api-2");
}, 1000);

```

原始碼：[console-time-multi.js](https://gist.github.com/dean9703111/07a46eb153dd89f5e02ee5a6f9f2071f)（GitHub Gist）

## 六、用 console.table() 將資訊轉成表格來閱讀

Backend 很常回傳 Table 格式的資訊，如果我們使用 console.log() 直接將結果印出來，在需要比對資料的時候會不好閱讀：

```
const users = [
  {
    name: "Dean",
    email: "dean_lin@test.com",
    department: "Cloud",
    job_title: "Manager",
    age: 33,
    hire_date: "2016-06-06",
  },
  {
    name: "Ray",
    email: "ray_chen@test.com",
    department: "Cloud",
    job_title: "Engineer",
    age: 31,
    hire_date: "2019-06-06",
  },
  {
    name: "Davy",
    email: "davy_chang@test.com",
    department: "Cloud",
    job_title: "Designer",
    age: 35,
    hire_date: "2017-06-06",
  },
];

console.table(users);

```

原始碼：[console-array.js](https://gist.github.com/dean9703111/786093942e0147d8fbb09d13a79b9a98)（GitHub Gist）

在上圖中，我們可以看到如果想要知道個別 user 的詳細資訊，需要自己手動展開 object，不過如果我們將最後的 `console.log(users);` 改寫成 `console.table(users);`，資料就會像下圖一樣變成表格很好閱讀嚕！

希望這篇文章對 Frontend 開發人員有所幫助，透過 console.log 的靈活使用，資料就會更清晰、準確地呈現！

參考資源：[It’s 2022, Please Don’t Just Use “console.log” Anymore](https://javascript.plainenglish.io/its-2022-please-don-t-just-use-console-log-anymore-217638337c7d)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202209/advanced-console-log/
