# [前端＆後端]工程師常犯的錯誤 — 下班有約系列文

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202202/common-frontend-backend-mistakes/
- 發布：2022-02-23（更新：2025-06-15）
- 主題：技術教學
- 系列：工程師下班有約
- 標籤：Frontend Development、Backend Development、Issues、Error Handling、下班有約
- 摘要：筆者跟擔任 Tech Lead 的朋友聊天時，發現在幫新人 Code Review、Code Refactoring、Bug Fix 時遇到了許多類似的問題，針對這個部分我安排了 3 篇文章做講解，今天這篇文章我挑選了幾個工程師常犯的錯誤來分享。

筆者跟擔任 Tech Lead 的朋友聊天時，發現在幫新人 Code Review、Code Refactoring、Bug Fixing 時遇到了許多類似的問題，針對這個部分我安排了 3 篇文章做講解，今天這篇文章我挑選了幾個工程師常犯的錯誤來分享。

```
大綱
➤ 保留太多無用的 console.log
➤ 沒有註明輸出的 console.log 要做什麼事
➤ 忽視 Console 吐出的警告、錯誤
➤ 不同函式共用「命名相同的全域變數」
➤ 大量存取不必要的 API
➤ 將很多用不到程式碼註解而非刪除
➤ 沒寫註解，或是寫沒有意義的註解
➤ Copy & Paste 後，Function、Variable 命名沒有改
➤ 沒有寫 try catch
➤ 在他人協助下完成功能後，沒有真正了解原理
➤ 沒有做 Refactoring
➤ 詢問別人前沒有自己嘗試
➤ 詢問的問題不夠精確
➤ 管理者權限與使用者權限混在一起

```

## ➤ 保留太多無用的 console.log

為了追蹤資料流向，我們會用 console.log 來確認程式是否依照我們的想法在執行；但有不少人只會一直增加新的 console.log，而不把已經不需要的 console.log 給移除。

## ➤ 沒有註明輸出的 console.log 要做什麼事

筆者曾在按下 F12 開啟開發人員工具幫人 debug 時，看到 Console 這個分頁下印出一堆 API 回傳的 response data，但命名完全一樣都叫「result」，這樣我完全無法知道這些 result 是由哪個 API 回傳的ＱＱ

不然就是很多意義不明只有自己才知道是什麼意思的 console.log（這類型的往往一個月後連本人都不知道在做什麼…）

## ➤ 忽視 Console 吐出的警告、錯誤

有些網頁看似運行的很正常，但在按下 F12 開啟開發人員工具後，你會看到一排讓人血壓上升的紅色 Error Message，如果沒有及時處理遲早有一天會業力引爆的。

## ➤ 不同函式共用「命名相同的全域變數」

像是 Vue 框架下用來儲存變數資訊的 「Vuex」，裡面 State 的資訊是可以在多個頁面共同使用，下面舉例幾個不夠謹慎會造成的問題：

- 如果在 loading 資料前沒有清空先前的 state 資訊，會導致進入頁面時會閃現上一次進入的資料。
- 如果把第幾頁(page)、分頁筆數(per-page)這些參數在多個頁面共用，那你就會看到有些頁面明明是第一次進入，卻跳轉到其他頁數。

## ➤ 大量存取不必要的 API

這裡舉出兩個常見的例子：

- 登入後就「一次呼叫所有的 API」，但許多 API 回傳的資料可能使用者根本還用不到。

這邊提醒一下，適當的「預載」可以增加使用者體驗，但如果在系統首頁就全部載入，除了會消耗使用者的資源外，一旦大量使用者同時上線，對伺服器端會造成很大的壓力。

- 有些搜尋功能會隨著使用者填入的文字去呼叫 API 來即時顯示搜尋結果或是建議關鍵字（ex：Google），但如果沒有把呼叫 API 的時間間隔設定好（debounceTime），使用者又不停更改搜尋列文字，那就會呼叫非常多不必要的 API。

## ➤ 將用不到程式碼註解而非刪除

在開發階段有時會想到很多的解決方案，因為不確定哪個效能較高，所以就把每個演算法實現出來。

但有些人在決定使用某個解決方案後，依舊沒把其他的方案刪除，這樣的做法容易造成未來維護的困難度。

## ➤ 沒寫註解，或是寫沒有意義的註解

雖然有神人說過：最好的註解就是程式碼本身

但我想：「**絕大多數的人都難以到達這個境界，所以要乖乖地寫註解ＸＤ」**

```
let mode = 0;

// do somethings ...

if (mode === 0) {
  // excute logic 1
} else if (mode === 1) {
  // excute logic 2
} else if (mode === 2) {
  // excute logic 3
}

```

原始碼：[bad_remark_example.js](https://gist.github.com/dean9703111/463d3dc91bc4e9789a29bcc4323560d9)（GitHub Gist）

像是上面這段 Code，我們可以知道 mode 這個變數應該是應用在某個狀態的判斷上；但具體在哪個地方使用、總共有哪些狀態、各自代表什麼意義，在沒有註解的狀態下只能一行一行的閱讀，沒辦法第一時間理解程式。

### 👍 註解的好處

- 交代前因後果：為什麼這樣寫?遇到了什麼困難?解決了什麼問題?
- 作為文件說明：有些技術是需要被學習後才能夠看得懂的(ex:引入的模組、套件)，你的註解能讓他快速理解這些工具怎麼使用

### 🙅 請不要有註解如下

- 毫無意義的牢騷(ex:我好崩潰啊，這段程式我寫了一個禮拜…)
- 把註解當成版本控制(ex:2020/08/23 完成此函式)
- 說明這段程式是誰寫的(簽名檔)
- 把舊的程式碼註解起來怕以後會用到(但通常再也不會用到，而且這個問題用版控就能解決)

## ➤ Copy & Paste 後，Function、Variable 命名沒有改

如果寫專案時發現之前做過類似的功能，我們會複製過來再做微調，以此加快開發的速度。

但**請修改 Function、Variable 命名！**這不會花你太多時間，但在未來當你需要維護專案時，你會感謝現在的自己。

筆者之前曾在幫忙 Debug 時，於Ａ系統裡面看到一堆Ｂ系統的命名，當下還以為自己開錯專案了…

## ➤ 沒有寫 try catch

程式在缺乏 try-catch 的機制下是非常脆弱的，只要發生例外事件就容易崩潰。

如果你直接假定 API 只會回傳正確的資訊，而沒有撰寫 try-catch 去捕捉可能的 400、500 系列錯誤，就很可能導致系統許多行為錯誤如：

- 跳轉到無權限的頁面
- 程式進入無限迴圈
- 瘋狂 Call 某隻 API 直到變成 429 錯誤（Too Many Requests）

## ➤ 在他人協助下完成功能後，沒有真正了解原理

我相信大部分的人都是樂於幫助別人解決問題的，但同時也相信，沒有人希望再次被「相同的問題」打擾。

我這邊強烈建議請他人協助問題前，先 commit 一版，這樣無論對方成功還是失敗，你都有一個記錄在。

如果對方成功解決這個問題，一定找時間回到當時 commit 的那版，用自己的邏輯把功能完成；**請記住，別人完成的任務不會自動成為你的知識。**

## ➤ 沒有做 Refactoring

演算法相關的 Refactor 有一定的難度，但有一些很基礎的 Refactor 我認為是有必要的，像是變數命名這一塊。

我們先來看一個不好的範例：

```
let result = responseData;
if (result[result.length - 1]) {
  if ("check_in" in result[result.length - 1] && result[result.length - 1].check_in !== null) {
    state.check_in = result[0].check_in.toString();
  }
}
if (result[result.length - 1]) {
  if ("check_out" in result[result.length - 1] && result[result.length - 1].check_out !== null) {
    state.check_out = result[result.length - 1].check_out.toString();
  }
}

```

原始碼：[need_refactor.js](https://gist.github.com/dean9703111/3c6c9d59d698b8738771eee07056b8fd)（GitHub Gist）

上面這個是要取得回傳的「最後一筆」資料做邏輯處理，但是全部都使用「**result[result.length -1]**」來表示，相信大家都會看得有點眼花。

下面我們做個簡單的 Refactor，看起來就會舒服很多：

```
let result = [];
let last_record = result[result.length - 1];
if (last_record) {
  if ("check_in" in last_record && last_record.check_in !== null) {
    state.check_in = last_record.check_in.toString();
  }
}
if (last_record) {
  if ("check_out" in last_record && last_record.check_out !== null) {
    state.check_out = last_record.check_out.toString();
  }
}

```

原始碼：[after_refactor.js](https://gist.github.com/dean9703111/f9afe9487cbf14ddec3b92a61335f2f5)（GitHub Gist）

## ➤ 詢問別人前沒有自己嘗試

有些人在遇到問題時沒有先從網路找資料，而是遇到問題先問同事、前輩。

儘管這樣可以最快速得到解答，但卻少了很多自我思考的部分，長期來講對工程師職涯是非常不健康的。

## ➤ 詢問的問題不夠精確

有時會收到主旨模糊的問題，像是：「這個搜尋功能常常會 fail，可以幫我看看嗎？」

如果可以，收到問題的人會希望問題敘述微調一下：「在人資系統中，我如果在職員列表頁面的搜尋條件中加入部門篩選，會發現搜尋出的結果不合預期，有時會找出其他部門的人員，可以幫我看一下是不是資料庫語法錯誤呢？」

**結構為**：哪個系統->哪個頁面->你調整了哪些參數->錯誤結果為何->初步判定錯誤來源

## ➤ 管理者權限與使用者權限混在一起

很多系統都會分成前台（一般使用者）以及後台（管理者），有時後端為了增加程式共用率，而讓他們使用同一隻 API 存取資料，如果權限有設計好那沒問題，但如果沒設計好可能就出大包了。

以人資系統來說，假設有一隻 API 是用來存取個人資訊的，API 格式為：「users/{user}」

假使把這隻 API 共用，一般使用者透過修改中間的「user id」就有可能看到其他人的隱私資訊，這在資訊安全上是非常危險的。

因此建議將一般使用者與管理者的 API 切割清楚，避免讓使用者看到權限外的資料。

筆者曾經犯過文章中許多錯誤，沒有人一開始就是強者，但至少要走在變強的道路上。

**如果你對目前的職涯感到迷茫，想從不同視角檢視自己的選擇，相信《[工程師下班有約](https://www.tenlong.com.tw/products/9786264142083)》這本書能為你帶來靈感與啟發：**

  [https://www.tenlong.com.tw/products/9786264142083](https://www.tenlong.com.tw/products/9786264142083)

```
下班有約系列文
 [前端]新手工程師容易卡住的問題
 [前端＆後端]工程師常犯的錯誤(本篇)
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
引用時請附上原文網址：https://deanlin.net/blog/202202/common-frontend-backend-mistakes/
