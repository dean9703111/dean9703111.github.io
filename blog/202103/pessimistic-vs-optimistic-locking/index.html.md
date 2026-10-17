# 理解資料庫『悲觀鎖』和『樂觀鎖』的觀念

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202103/pessimistic-vs-optimistic-locking/
- 發布：2021-03-12（更新：2023-05-05）
- 主題：技術教學
- 標籤：悲觀鎖 Vs 樂觀鎖、資料庫、樂觀鎖、悲觀鎖、範例
- 摘要：沒有經過實際操作的理論都只是空談，這篇文章我會附上自己的MySQL 語法 方便大家在自己的環境中驗證這個資料庫的觀念，讓日後處理併發問題時有更完整的思路。

沒有經過實際操作的理論都只是空談，這篇文章我會附上自己的MySQL 語法方便大家在自己的環境中驗證這個資料庫的觀念，讓日後處理併發問題時有更完整的思路。

```
一、為什麼要了解他們 🔐？
二、從字面上了解悲觀鎖、樂觀鎖
三、在 MySQL DB 實作悲觀鎖
四、在 MySQL DB 實作樂觀鎖
五、悲觀鎖、樂觀鎖優缺點

```

## 一、為什麼要了解他們 🔐？

網路商城開特賣會的時候常常會推出一件商品只限 10 人搶購，在搶購的過程除了會產生高併發的問題外，同時也需要考慮到『商品會不會超賣』，在現實生活中聽起來很像不可思議，但在網路的世界裡如果沒做好資料庫的防護真的可能發生明明只限 10 人搶購卻有 30 個人搶到商品的超賣事件。

### ❓ 今天想要解決的問題：

假設 A 跟 B 都想要買 iphone 手機，但商城的 iphone 庫存只剩下一隻，在不加鎖的狀況下同時下單會導致庫存變成負號；實作上要如何解決這個問題呢？

## 二、從字面上了解悲觀鎖、樂觀鎖

### 😭 悲觀鎖 —Pessimistic Lock

就跟字面上的意思一樣，非常的悲觀；他認為 table 裡面的 data 非常的不安全，無時無刻都在變動，當一個SQL command(可以理解為搶購的用戶)獲得悲觀鎖後，其他的 SQL command 無法對這個 data 進行修改，直到悲觀鎖被釋放後才能執行。

資料表中用 syncronized 實現的鎖均為悲觀鎖(ex：行鎖，表鎖，讀鎖，寫鎖)

### 😃 樂觀鎖 — Optimistic Lock

相比悲觀鎖，他認為 table 裡面的 data 變動頻率不會太頻繁，因此他會允許多個 SQL command 來操作 table；但樂觀並不代表不負責，通常會在 table 中增加一個 version 的欄位來做更新的確認。因此當 SQL command 想要變更欄位 data 時會先把之前取出 version 跟 table 現在的 version 做對比，如果相同就代表這段期間沒人修改可以執行；如果不同就會禁止這次的操作。

## 三、在 MySQL DB 實作悲觀鎖

### A. 建立模擬資料：

先用 SQL command 建立一個簡單的 table 並插入 data

```
CREATE TABLE `your_DB`.`items` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(45) NOT NULL,
  `num` INT NOT NULL,
  PRIMARY KEY (`id`));
INSERT INTO `your_DB`.`items` (`name`,`num`)
VALUES ("iphone",1);

```

原始碼：[create_mock_item_table.sql](https://gist.github.com/dean9703111/334f6b3cd81452fc56491e7360fd9265)（GitHub Gist）

  直接進去 MySQL command line 模式貼上去就好嚕

  用 **SELECT * FROM `your_DB`.`items`;** 確認一下資料是否建立成功

### B. 用悲觀鎖解決 iphone 購買問題

**STEP 1**：開一個視窗模擬 A 的行為，我們先在 A 購買前先將 `id=1` 的 data 加上悲觀鎖，此時這行 data 只有 A 可以操作

  Transaction 的開始要用 **begin;**，不然 MySQL 會自動提交

  接著用 **select num from `your_DB`.`items` where id = 1 for update; **將 id = 1 的 data 加上悲觀鎖

**STEP 2**：此時開另一個視窗模擬 B 的行為，一樣將 `id=1` 的 data 加上悲觀鎖

  你會發現會進入等待的模式，要等 A 執行 commit 完成交易或是指令逾時才能繼續

**STEP 3**：回到 A 的視窗，接著我們讓 A 執行購買的動作將 data 的 num 減一，然後檢視修改後的 data 是否符合預期，最後下 commit 結束這次交易

  購買： **update `your_DB`.`items` set num = num -1 where id = 1;** ，檢視修改：**select num from `your_DB`.`items` where id = 1; **，結束交易：**commit;**

**STEP 4**：當 A 完成下單後回到 B 的視窗，你會發現因為 A 釋放了悲觀鎖，所以 B 結束等待並且獲得鎖，但 data 的 num 已經變成了 0 只能放棄購買

  7.89 秒是等待獲得鎖的時間

## 四、在 MySQL DB 實作樂觀鎖

### A. 建立模擬資料：

先用 SQL command 建立一個簡單的 table 並插入 data，比起悲觀鎖的 table，我們多了一個 version 的欄位

```
CREATE TABLE `your_DB`.`happy_items` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(45) NOT NULL,
  `num` INT NOT NULL,
  `version` INT NOT NULL,
  PRIMARY KEY (`id`));
INSERT INTO `your_DB`.`happy_items` (`name`,`num`,`version`)
VALUES ("iphone",1,0);

```

原始碼：[create_mock_happy_item_table.sql](https://gist.github.com/dean9703111/4a8469117f075b3db7194106417c6765)（GitHub Gist）

  直接進去 MySQL command line 模式貼上去就好嚕

  用 **SELECT * FROM `your_DB`.`happy_items`; **確認一下資料是否建立成功

### B. 用樂觀鎖解決 iphone 購買問題

**思路**：A 跟 B 在下單前會先將 `id=1` 的 data 找出來，然後 A 先買，此時會以 table 的 `id=1 和 version=0` 作為條件來更新 data，更新後 num 減一、version 加一變成 `id=1、num=0、version=1`；此時 B 購買時會因為用 `id=1 和 version=0`找不到 data 而無法更新。

**STEP 1**：A 及 B 執行查詢的 SQL command 會取得相同的 iphone 庫存資訊

  用 **SELECT num,version FROM `your_DB`.`happy_items` where id = 1;** 找出 iphone 庫存資訊

**STEP 2**：A 先購買了 iphone 來更新 data，然後檢視修改後的 data 是否符合預期

  購買：**update `your_DB`.`happy_items` set num = num -1 , version = version + 1 where id = 1 and version = 0; **，檢視修改：**SELECT num,version FROM `your_DB`.`happy_items` where id = 1;**

**STEP 3**：此時 B 想要購買時就會無法更新 data，執行購買的 SQL command後你會發現沒有 Row 被更新

  購買：**update `your_DB`.`happy_items` set num = num -1 , version = version + 1 where id = 1 and version = 0;**

## 五、悲觀鎖、樂觀鎖優缺點

### 😭 悲觀鎖 — Pessimistic Lock

**優點**：使用資料庫 Transaction 的機制來強迫執行的順序

**缺點**：一但加入 Transaction 的機制會導致其他的 SQL command 針對這個 data 除了查詢功能外全部卡死，如果這段 Transaction 執行時間較長會給使用者不良的體驗並造成系統吞吐量下降

### 😃 樂觀鎖 — Optimistic Lock

**優點**：因為沒有在資料庫加鎖，所以 SQL command 都會購對 data 進行操作，只有在更新 data 時才會做驗證；這樣就避免悲觀鎖導致的吞吐量下降的問題

**缺點**：因為樂觀鎖是我們人為實現的，所以如果換一個業務場景可能會不適用，甚至可能因為其他的 SQL command 導致錯誤。

參考資源：[關於悲觀鎖和樂觀鎖的個人圖解](https://kknews.cc/zh-tw/news/z9aqnva.html)、[商品秒殺問題](https://www.jianshu.com/p/aaa12e90e9b9)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202103/pessimistic-vs-optimistic-locking/
