# [前端]新手工程師容易卡住的問題 — 下班有約系列文

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202202/frontend-junior-pitfalls/
- 發布：2022-02-23
- 主題：技術教學
- 系列：工程師下班有約
- 標籤：Code Review、Code Refactoring、Bug Fixes、Frontend、Junior
- 摘要：筆者跟擔任 Tech Lead 的朋友聊天時，發現我們在幫新人 Code Review、Code Refactoring、Bug Fixing時遇到了許多類似的問題，針對這個部分我安排了 3 篇文章做講解，而今天這篇文章會針對新手工程師容易卡住的問題來做分享。

筆者跟擔任 Tech Lead 的朋友聊天時，發現我們在幫新人 Code Review、Code Refactoring、Bug Fixing 時遇到了許多類似的問題，針對這個部分我安排了 3 篇文章做講解，而今天這篇文章會針對新手工程師容易卡住的問題來做分享。

```
大綱
➤ 對網頁的元件(tag)、屬性(attr)不熟悉
➤ 對 Object、Array 以及 null、undefine、"" 的特性並不熟
➤ 不了解「==」跟「===」的差異
➤ API 傳參數時遇到各種遇到困難
➤ 非同步(Asynchronous)/同步(Synchronous)概念混亂
➤ 不了解程式的運作流程
➤ 演算法能力需要加強
➤ 看不懂 Console 吐出的錯誤

```

## 對網頁的元件(tag)、屬性(attr)不熟悉

通常我們對「按鈕」的認知是「button」這個元件，相關的「click」事件也是由它來觸發；但我曾經遇過下面這個案例：

```
<v-btn color="grey" class="ml-4 mt-7">
  <h6 @click="search()">搜尋</h6>
</v-btn>

```

原始碼：[err_button.html](https://gist.github.com/dean9703111/4a40a295f6d3211094992235066a364b)（GitHub Gist）

他將「click」的事件包到「h6」這個元件中，這造成有時候**點擊按鈕是沒有反應的**，因為 click 事件觸發的有效範圍只在「h6」裡面，而**不是整個 button 的範圍**。

後來發現這類因為對網頁元件不熟悉而導致的 Bug 還蠻常見的，如果 Debug 遇到類似問題時，可以先到網路上查詢大家通常會用這個元件來做什麼事，可以減少烏龍事件的發生。

> 筆者曾看過用「table」來做切版的，在電腦螢幕 Demo 時看起來一切正常，但切換到手機的瞬間就崩潰了…

## 對 Object、Array 以及 null、undefine、”” 的特性並不熟

前端串接 API 時，通常都會接收到以 json 格式回傳的物件，但假設我們要取得的是 Array 裡面的資料，如果沒有經過驗證就直接使用很容易會產生 Bug。

像下面這個範例，如果回傳的是空陣列，如果沒有先驗證「陣列長度」就直接取用就會噴錯誤。

```
let response = [];
console.log(response[0].status);

```

原始碼：[array_err.js](https://gist.github.com/dean9703111/caae7f1fadb077386c91a1df48f0e190)（GitHub Gist）

有時我們會用物件參數的值來判斷要執行哪個程式，但如果用 undefined、空字串、null 來比對，就可能造成程式沒有依照我們想的邏輯去執行。

```
let user = {
  id: 1,
  name: "baobao",
  email: "baobao@gmail.com",
  status: "",
};
if (status === null) {
  console.log("執行A");
} else {
  console.log("執行B");
}

```

原始碼：[null_logic.js](https://gist.github.com/dean9703111/c6ae5bef92b0fd1974736afc837ef9d2)（GitHub Gist）

下面再舉例一個極端物件，大家可以不要急著往下滑看答案，先思考看看印出來會是什麼結果？

```
let object = {
  a: "",
  b: undefined,
  c: null,
  d: [],
  e: {},
};
if (object.a) console.log(object.a);
if (object.b) console.log(object.b);
if (object.c) console.log(object.c);
if (object.d) console.log(object.d);
if (object.e) console.log(object.e);

```

原始碼：[object_example.js](https://gist.github.com/dean9703111/a8b7daca5e33156180627955c87ba3c7)（GitHub Gist）

從下圖你可以看到除了「陣列、物件」外，其他都不會通過喔～

又或者有時我們在處理字串時會用「include」這個函式來比對，但如果你丟進去的參數是「null、undefine」這類的也會出事喔！

```
let object = {
  string: null,
};
object.string.include("something..");

```

原始碼：[include_err.js](https://gist.github.com/dean9703111/b4bf6ebb83058e45981b7d47aef86372)（GitHub Gist）

## 不了解「==」跟「===」的差異

不知道大家有沒有想過上面提到的「undefined、null、空字串」如果用「==」跟「===」來比較會有怎麼樣的結果。

```
console.log("undefined==null", undefined == null);
console.log("undefined===null", undefined === null);
console.log("null==''", null == "");
console.log("null===''", null === "");
console.log("''==undefined", "" == undefined);
console.log("''===undefined", "" === undefined);

```

原始碼：[null_undefined_empty.js](https://gist.github.com/dean9703111/1cd4b3426fda2774476d96f59ca98e76)（GitHub Gist）

或者像是數字、字串、布林值這類的比較，有時這類型的的錯誤也常常讓人 Bug 找半天。

```
console.log("'true'==true", "true" == true);
console.log("'true'===true", "true" === true);
console.log("1=='1'", 1 == "1");
console.log("1==='1'", 1 === "1");

```

原始碼：[number_boolean.js](https://gist.github.com/dean9703111/d1325479adeb360fedf3415766d1d18a)（GitHub Gist）

## API 傳參數時遇到各種困難

許多新手前端儘管可以完成切版，但是在串接 API 時卻遇到非常多的困難，就算看著 API 文件，在操作 GET、POST、PUT 傳遞參數時有很高的機率傳錯格式，像是：

- POST 的參數用 Params 傳，直接變成明碼。
- 不知道 Body 的 form-data 怎麼組成、raw 如何組成 json 格式。
- 常常有缺少或是格式錯誤參數。

這部分我覺得如果後端工程師願意將 Postman 的 Collection 匯出，並在 Description 寫好邏輯直接讓前端工程師來確認，就會稍微減少以上的問題。

## 非同步(Asynchronous)/同步(Synchronous)概念混亂

以非同步(Asynchronous)來說，最常見的錯誤就是前端在呼叫 API 時，並沒有等待資料回傳就直接使用它，導致畫面渲染的時候出現各種 Bug。

以同步(Synchronous)來說，有時會設計了太多不必要的等待機制，導致在某個環節卡死無法進行下一步。

## 不了解程式的運作流程

新進工程師通常都要維護 Legacy code，但通常他們並不具備掌握程式運作流程的能力。

這塊除了註解以及技術文件可以加快新手的熟練度外，資深工程師願意一起 Pair Programing 也是一個讓新進工程師能力大增的方式。

> 我在實戰經驗中發現[手把手教學（pair programing）](https://deanlin.net/blog/202111/pair-programming/)的效率比給 hint 的效率高很多，如果只給新手 hint，除發他天賦異稟，不然有大機率是有聽沒有懂。

在幫忙 Debug 時，筆者也發現很多新手工程師並不瞭解自己的程式到底會怎麼跑，這個時候我們可以透過下中斷點（Breakpoint），讓程式執行的每一步慢慢地在我們眼前呈現。

## 演算法能力需要加強

很多時候一個結果是需要經過多次運算才會得到，其中可能含有複雜的 and(&&)、or(ll)、not(!)、大於(>)、小於(<)..邏輯。

假設你今天要設計一個打卡系統，如果要判斷一個職員今日出勤是否有異常，在程式設計上就需要考慮到幾個基礎邏輯：

1. 公司是否有彈性上、下班時間
2. 該名職員上、下班打卡時間是否在彈性上、下班的區間中
3. 該名職員上班總時數是否符合公司規定
4. 在判斷上下班打卡異常時，是否有考慮到假勤

如果上面的邏輯複雜度已經超過你的腦容量，我強列建議回歸紙筆。

複雜的邏輯用紙筆寫下來會比較容易理解，這是因為大腦在面對電腦時，邏輯容易憑空想像；而用紙筆寫的時候則可以將邏輯具象化，讓大腦放慢一點腳步反而能找到解法。

> 想提升演算法能力，推薦挑戰 LeeCode easy 等級的題目。

## 看不懂 Console 吐出的錯誤

遇到問題卡關時，我們往往會先上網找答案，不管正不正確，先貼回來看看能不能動。

有時候可能真的找到正確解答了，但因為兩者程式執行的環境不同而有報錯，此時很多新人會因為看不懂這個錯誤，轉而找其他的解答複製貼上。

其實如果你將上面的錯誤訊息貼到 Google，就有很大的機率可以得到解答；一直尋找只靠一步就能完成的解答，有時反而是在走遠路。

> [StackOverflow ](https://stackoverflow.com/)上面聚集各種大神，只要善用關鍵字，基本上你遇到的問題以前已經有數不清的人踩過坑了。

## 只使用自己會的技能

面對新的問題時，我們會習慣從大腦搜尋已經純熟技能，但有些問題是現有技能無法處理的，或者要花大量時間才能解決。

以前端的 UI Component 舉例，有些人花大量的時間去創造，但往往最後產出的結果遠遠不如現成的框架、套件，甚至還會有各式各樣的 Bug。

下次遇到現有技能很難解決的問題時，不妨先搜尋網路，看看是否有前輩已經提供很好的解決方案；學習新技能所花的時間，有時會比硬要用過去技能解決更省時喔～

> 這篇文章只是舉出最常見的幾個問題，筆者相信還有很多點沒有提到，如果讀者有想到的話歡迎補充～

**如果你對目前的職涯感到迷茫，想從不同視角檢視自己的選擇，相信《[工程師下班有約](https://www.tenlong.com.tw/products/9786264142083)》這本書能為你帶來靈感與啟發：**

  [https://www.tenlong.com.tw/products/9786264142083](https://www.tenlong.com.tw/products/9786264142083)

```
下班有約系列文
 [前端]新手工程師容易卡住的問題(本篇)
 [前端＆後端]工程師常犯的錯誤
 讓開發團隊更好協作的方式
 談談 Pair Programing
 談談工程師的話語權
 出社會後，新鮮人要及早知道的 5 件事
 面試的性向測驗竟然準到讓人心裡發寒？
 反其道而行的任務分配
 現在的 Daily Scrum 真的適合團隊嗎？
 用低成本也能請到好員工？資深 HR 不想告訴你的秘密！

```

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202202/frontend-junior-pitfalls/
