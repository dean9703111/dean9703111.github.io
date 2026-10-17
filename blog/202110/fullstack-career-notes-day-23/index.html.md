# [面試][白板題]設計一個簡易的抽獎程式

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202110/fullstack-career-notes-day-23/
- 發布：2021-10-08
- 主題：職涯成長
- 系列：全端工程師生存筆記
- 標籤：iT 邦幫忙鐵人賽、面試、白板題、抽獎程式、完善程式
- 摘要：相比於系統設計，白板題往往需要寫出能夠運行的程式，或者提供面試官認為可行性高的演算法；大部分的白板題難度會落在 LeeCode Easy 的等級，有程式基礎的人基本上都能夠給出答案，但除了給出答案外，程式的正確率與執行效率也會列入評分。

> 白板題跟系統設計問題的相同點，就是重視釐清問題與`溝通`。

相比於系統設計，白板題往往需要寫出能夠運行的程式，或者提供面試官認為可行性高的演算法；大部分的白板題難度會落在 LeeCode Easy 的等級，有程式基礎的人基本上都能夠給出答案，但除了給出答案外，程式的正確率與執行效率也會列入評分。

本篇文章筆主要是分享答題思路以及完善程式，演算法並非筆者所擅長；如果想提升演算法的能力，除了買一本書好好閱讀外，同時也要去 LeetCode 刷題累積實戰經驗。

> 有些公司是考 LeeCode Medium/Hard 等級的題目，除非天賦異稟；否則這個難度需要花大量時間（可能半年以上）練習，才能在現場穩定發揮。

## 大綱

1. **初探白板題**

2. 1.1 白板題的類型
3. 1.2 過程有時比解答更重要

4. **設計一個簡易的抽獎程式**

5. 2.1 問題敘述
6. 2.2 面試官為什麼會問？
7. 2.3 面試官想從答案確認什麼？
8. 2.4 筆者提供的簡答

9. **衍伸問題**

10. 3.1 如果使用者輸入的機率有小數點呢？
11. 3.2 你有考慮到使用者可能輸入不合理的機率嗎？
12. 3.3 你會用什麼方式來驗證程式的正確性？
13. 3.4 留給讀者優化與思考的問題

## 1. 初探白板題

### 1.1 白板題的類型

白板題大概分成以下幾種形式：

- **在白板回答**
通常用白板回答的只要寫出演算法跟架構就好。

> 相較於其他形式，更注重與面試官的「溝通」與「確認問題」。

- **紙上作業**
用來觀察求職者在沒有 IDE 輔助下，是否具備寫程式的能力。

> 現在的 IDE 實在太方便了，函式會自動補全、錯誤會直接 Highlight，很多工程師脫離 IDE 後真的是武功全廢。

- **上機考**
通常會給你一個時間限制，要求你在時間內寫出一個可以 Run 的 MVP；大多數的面試官會看著你寫 Code 的過程。

> 有些人會因為不適應有人盯著寫程式，導致現場發揮失常。

### 1.2 解題過程比解答更重要

也許你已經非常熟悉 LeeCode 的解題模式；但白板題跟 LeeCode 不同的地方在於通常沒有測資、題目相對模糊。

如果你接到題目就直接解題，可能會忽略一些隱性條件；在了解題目時如果發現模糊地帶，請與面試官釐清問題，有時自己想像出來的條件反而會增加題目的複雜度。

通常在你給出第一版的解答後，面試官會以你的解答為基礎循序漸進問一些延伸性的問題，這部分除了考核程式能力外，也是判斷求職者在開發過程中能否與團隊成員溝通；畢竟隨著專案規模的擴增，單打獨鬥的情境已不多見。

如果遇上你無法解決的白板題，請以積極的態度與面試官溝通你想到的解題方式和卡住的點；相比於求職者的程式能力，有些面試官更加看重求職者的人格特質；有些人也許程式能力不足，但能理解面試官的問題，並在討論過程中作出改善；若能在解題過程中被面試官視為有潛力的人才，也是有機會獲得 Offer 的。

> **備註 1**：解題過程中並不是所有面試官都是以「討論」的形式與你溝通，有些是以「質問」的態度面對求職者；遇到用「質問」態度的面試官，你也要思考自己是否能夠與這樣的同事一起共事。

> **備註 2**：白板題需要有良好的程式基礎與抗壓性，才可能在面試現場表現優良。

## 2. 設計一個簡易的抽獎程式

為了方便讀者閱讀與驗證，本篇文章以`上機考`的形式向大家分享。

### 2.1 問題敘述

請你設計一個抽獎系統，獎品有「一獎、二獎、三獎」，其餘都是感謝獎。

一獎、二獎、三獎的中獎機率可以自由輸入，若沒輸入則默認一獎（1%）、二獎（2%）、三獎（3%）。

**時間限制**：5 分鐘

### 2.2 面試官為什麼會問？

這個題目雖然簡單，卻有很多延伸的議題可以討論；是一個瞭解求職者`基本功`的經典題目。

### 2.3 面試官想從答案確認什麼？

- 你的程式是否保留擴充空間
- 你會用什麼演算法解決這個問題
- 是否有驗證使用者輸入的參數
- 如何驗證你的程式是符合需求的

### 2.4 筆者提供的簡答

在面對白板題時，筆者是先求有再求好；如果一昧追求高效但自己不熟悉的演算法，有時會有意想不到的錯誤。

筆者的邏輯是先把籤放入籤筒，再透過亂數抽獎印出結果，以符合題目的基礎要求為目標：

- 使用者可以透過調整參數變更中獎機率。
- 有`默認`的中獎機率：一獎（1%）、二獎（2%）、三獎（3%）。
- 用 `Math.random()` 函式模擬亂數抽獎。
- 會印出中獎結果。

```
// 寫一個函式，有默認的抽獎機率：一獎(1%)、二獎(2%)、三獎(3%)
function lottery(first_prize = 1, second_prize = 2, third_prize = 3) {
  let lickbox = [];
  // 放入籤筒
  for (var i = 0; i < 100; i++) {
    if (first_prize !== 0) {
      lickbox.push("一獎");
      first_prize--;
    } else if (second_prize !== 0) {
      lickbox.push("二獎");
      second_prize--;
    } else if (third_prize !== 0) {
      lickbox.push("三獎");
      third_prize--;
    } else {
      lickbox.push("感謝獎");
    }
  }
  // 抽獎
  console.log(lickbox[Math.floor(Math.random() * 99)]);
}
lottery();

```

上面的程式先不論演算法優劣，還有許多細節是沒有被考慮到的；在完成 MVP 後有些面試官會先問你：「之前有寫過類似的程式嗎？」如果沒有相關經驗就老實說沒有吧；接下來面試官會以這個程式為基礎，跟你討論哪裡還可以做改善。

## 3. 衍伸問題

### 3.1 如果使用者輸入的機率有小數點呢？

考點：求職者對浮點數的處理

很顯然，如果使用者輸入的機率包含小數點，現在的函式無法應付，下面是筆者的處理方式：

- 取得輸入得獎機率中最長的小數點有幾位（N）。
- 將所有得獎機率乘以 10 的 N 次方，這樣機率就沒有小數點了。
- 將 10 的 N 次方再乘以 100，以此為籤筒的總數量。

```
function lottery(first_prize = 1, second_prize = 2, third_prize = 3) {
  // 取得最長小數點
  let tmp = [first_prize, second_prize, third_prize];
  let longest = 0;
  tmp.forEach((prize) => {
    let after_point = prize.toString().split(".")[1];
    if (after_point) {
      if (after_point.length > longest) {
        longest = after_point.length;
      }
    }
  });

  // 讓機率變整數
  let multiple = Math.pow(10, longest);
  // 沒有用 Math.floor 會出現 js 小數點結尾的 bug，ex:11100.000000000002
  first_prize = Math.floor(first_prize * multiple);
  second_prize = Math.floor(second_prize * multiple);
  third_prize = Math.floor(third_prize * multiple);
  // 籤筒的總數量
  let max = multiple * 100;

  let lickbox = [];
  // 放入籤筒
  for (var i = 0; i < max; i++) {
    if (first_prize !== 0) {
      lickbox.push("一獎");
      first_prize--;
    } else if (second_prize !== 0) {
      lickbox.push("二獎");
      second_prize--;
    } else if (third_prize !== 0) {
      lickbox.push("三獎");
      third_prize--;
    } else {
      lickbox.push("感謝獎");
    }
  }
  // 抽獎
  console.log(lickbox[Math.floor(Math.random() * (max - 1))]);
}
lottery(1.11, 2.34, 3.567);

```

### 3.2 你有考慮到使用者可能輸入不合理的機率嗎？

考點：能舉例會發生錯誤的情境，並設計程式來驗證傳入的參數

如果沒有對輸入參數做驗證，程式的穩定性真的是極低，下面列舉幾個要做的基礎驗證：

- 中獎機率不可能超過 100%，如果一獎、二獎、三獎加起來超過 100 是不合理的。
- 中獎機率要是正數，輸入負的機率也是不合理。
- 如果參數輸入文字或是符號就一定是錯的。

這邊筆者透過撰寫一個函式（validate_input）來驗證輸入的參數，如果不符規則就會跳出錯誤訊息。

```
function validate_input(first_prize, second_prize, third_prize) {
  // 確認是否為正數
  var reg = /^(?=.+)(?:[1-9]\d*|0)?(?:\.\d+)?$/;
  if (!(reg.test(first_prize) && reg.test(second_prize) && reg.test(third_prize))) {
    console.log("請確認輸入參數皆為正數！");
    return false;
  }

  // 確認沒爆表
  if (first_prize + second_prize + third_prize > 100) {
    console.log("中獎機率超過 100 %，爆表了！");
    return false;
  }
  return true;
}
const argsArray = [
  [100, 2.34, 3.567], //中獎率不可能超過100
  [-1, 2.34, 3.567], //機率不可為負
  [1, "錯誤", 3.567], //含有文字
  [1, 2.34, 3.567],
];
argsArray.forEach((args) => {
  console.log("驗證: " + args);
  validate_input.apply(this, args);
});

```

### 3.3 你會用什麼方式來驗證程式的正確性？

考點：是否有自動化的測試方案來驗證程式

程式的正確性並不是用嘴說的，面試官想了解你會用什麼方法驗證程式的正確性。

筆者這邊會寫一個測試函式來蒐集抽獎結果，並對原本程式做如下調整：

- 籤筒會反覆使用，所以把它獨立成函式（initLickbox）。
- lottery 函式改為純粹抽籤用。
- 建立測試函式（qa），可調整測試次數，在執行時會印出實際機率與期望機率的比對。

這裡附上調整過的完整程式：

```
function initLickbox(first_prize = 1, second_prize = 2, third_prize = 3) {
  // 先驗證輸入參數
  if (!validate_input(first_prize, second_prize, third_prize)) {
    return;
  }
  // 取得最長小數點
  let tmp = [first_prize, second_prize, third_prize];
  let longest = 0;
  tmp.forEach((prize) => {
    let after_point = prize.toString().split(".")[1];
    if (after_point) {
      if (after_point.length > longest) {
        longest = after_point.length;
      }
    }
  });

  // 讓機率變整數
  let multiple = Math.pow(10, longest);
  // 沒有用 Math.floor 會出現 js 小數點結尾的 bug，ex:11100.000000000002
  first_prize = Math.floor(first_prize * multiple);
  second_prize = Math.floor(second_prize * multiple);
  third_prize = Math.floor(third_prize * multiple);
  // 籤筒的總數量
  let max = multiple * 100;

  let lickbox = [];
  // 放入籤筒
  for (var i = 0; i < max; i++) {
    if (first_prize !== 0) {
      lickbox.push("一獎");
      first_prize--;
    } else if (second_prize !== 0) {
      lickbox.push("二獎");
      second_prize--;
    } else if (third_prize !== 0) {
      lickbox.push("三獎");
      third_prize--;
    } else {
      lickbox.push("感謝獎");
    }
  }
  return lickbox;
}

function lottery(lickbox = []) {
  let max = lickbox.length;
  // 抽獎
  let result = lickbox[Math.floor(Math.random() * (max - 1))];
  return result;
}
function validate_input(first_prize, second_prize, third_prize) {
  // 確認是否為正數
  var reg = /^(?=.+)(?:[1-9]\d*|0)?(?:\.\d+)?$/;
  if (!(reg.test(first_prize) && reg.test(second_prize) && reg.test(third_prize))) {
    console.log("請確認輸入參數皆為正數！");
    return false;
  }

  // 確認沒爆表
  if (first_prize + second_prize + third_prize > 100) {
    console.log("中獎機率超過 100 %，爆表了！");
    return false;
  }
  return true;
}
function qa(test_times = 10000) {
  // 設定中獎機率
  let first_prize = 1.5,
    second_prize = 2.33,
    third_prize = 3.98;
  let thanks_prize = 100 - first_prize - second_prize - third_prize;
  let lickbox = initLickbox(first_prize, second_prize, third_prize);
  let first = 0,
    second = 0,
    third = 0,
    thanks = 0;
  for (var i = 0; i < test_times; i++) {
    let result = lottery(lickbox);
    if (result == "一獎") {
      first++;
    } else if (result == "二獎") {
      second++;
    } else if (result == "三獎") {
      third++;
    } else {
      thanks++;
    }
  }
  console.log("一獎:" + ((first / test_times) * 100).toFixed(2) + "% (" + first_prize + "%)");
  console.log("二獎:" + ((second / test_times) * 100).toFixed(2) + "% (" + second_prize + "%)");
  console.log("三獎:" + ((third / test_times) * 100).toFixed(2) + "% (" + third_prize + "%)");
  console.log("感謝獎:" + ((thanks / test_times) * 100).toFixed(2) + "% (" + thanks_prize + "%)");
}
qa();

// console.log(lottery(initLickbox()))

```

圖片為程式測試的結果：

### 3.4 留給讀者優化與思考的問題

上面只是筆者在面對白板題時的簡單發想，這邊還有一些問題留給讀者思考：

- 需不需要限制浮點數在小數點後的位數？
- 筆者的解決方案超級土法煉鋼，有沒有更好的演算法可以優化？
- 如果改成抽籤不放回的機制，程式要如何調整？
- 假使有四獎、五獎、六獎…程式架構要如何優化？

即使是抽獎系統這種看似簡單的題目，如果深入探討還是有許多的細節需要完善。

筆者上面所提出的衍伸問題，其實是求職者一開始要跟面試官釐清的細節，如果能夠在開始作業前就定義好需求，在開發的過程中就能避免不合適的設計。

不過有些面試官會傾向等求職者完成 MVP 後再做細節討論，筆者就曾遇過面試官出完白板題後就離開 15 分鐘讓求職者作答；所以實際狀況還是需要讀者在現場隨機應變。

感謝大家的閱讀，如果喜歡我的文章可以`訂閱`接收通知；如果有幫助到你，按`Like`可以讓我更有寫文的動力，我們明天見～

> 在許多人的幫助下，本系列文章已成功出版，除了添加新的篇章，更完善了每個案例的應對進退；如果對現在的職涯感到迷茫，也許這本書能帶給你不一樣的觀點～
> **天瓏書局：** [https://www.tenlong.com.tw/products/9786263334571](https://www.tenlong.com.tw/products/9786263334571)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202110/fullstack-career-notes-day-23/
