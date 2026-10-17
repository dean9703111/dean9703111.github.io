# [面試][資料庫]關聯式資料庫要如何設計避免超賣？

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202110/fullstack-career-notes-day-19/
- 發布：2021-10-04
- 主題：職涯成長
- 系列：全端工程師生存筆記
- 標籤：iT 邦幫忙鐵人賽、面試、資料庫、悲觀鎖、樂觀鎖
- 摘要：網路商城特賣會常常會推出特定商品限量 1 組的搶購活動，在搶購過程除了會產生高併發的問題外，同時也需要考慮到「商品會不會超賣」，在現實生活中聽起來很不可思議；但在網路的世界裡，如果SQL邏輯沒有設計好，真的可能發生明明只限 1 組卻賣了 10 組商品的超賣事件。

> 庫存只剩 1 件，但卻有 10 個人買到？

網路商城特賣會常常會推出特定商品限量 1 組的搶購活動，在搶購過程除了會產生高併發的問題外，同時也需要考慮到「商品會不會超賣」，在現實生活中聽起來很不可思議；但在網路的世界裡，如果SQL邏輯沒有設計好，真的可能發生明明只限 1 組卻賣了 10 組商品的超賣事件。

## 大綱

1. **關聯式資料庫要如何設計避免超賣？**

2. 1.1 面試官為什麼會問？
3. 1.2 面試官想從答案確認什麼？
4. 1.3 筆者提供的簡答

5. **回答問題所需具備的知識**

6. 2.1 什麼是悲觀鎖（Pessimistic Lock）？
7. 2.2 什麼是樂觀鎖（Optimistic Lock）？
8. 2.3 在 MySQL DB 實作悲觀鎖
9. 2.4 在 MySQL DB 實作樂觀鎖

10. **衍伸問題**

11. 3.1 悲觀鎖、樂觀鎖優缺點

## 1. 關聯式資料庫要如何設計避免超賣？

### 1.1 面試官為什麼會問？

這算是資料庫的常見面試題，對金融、電商相關產業來說都快要變成必考題了。

通常問完這題後會緊接著詢問：「如果是用 noSQL 有什麼處理方案？」這部分會在[明天的文章](https://deanlin.net/blog/202110/fullstack-career-notes-day-20/)跟大家講解。

### 1.2 面試官想從答案確認什麼？

- 是否有資料庫 Transaction 的概念
- 了解悲觀鎖（Pessimistic Lock）、樂觀鎖（Optimistic Lock）的概念
- 知道悲觀鎖（Pessimistic Lock）、樂觀鎖（Optimistic Lock）優缺點、使用情境

### 1.3 筆者提供的簡答

我會依照不同的情境來做設計，如果寫入需求較高，我會選擇使用`悲觀鎖（Pessimistic Lock）`；因為它是直接使用資料庫 `Transaction` 的機制，保證了資料的穩定性。

如果讀取需求較高，我會選擇使用`樂觀鎖（Optimistic Lock）`；因為它可以承受較高的吞吐量，不過它鎖的特性會導致購買順序未必是先到先得，只是保證不會超賣。

## 2. 回答問題所需具備的知識

### 2.1 什麼是悲觀鎖（Pessimistic Lock）？

就跟字面上的意思一樣，非常的悲觀；它認為 Table 裡面的 data 非常不安全，無時無刻都在變動，當一個 SQL command（可以理解為搶購的用戶）獲得悲觀鎖後，其他的 SQL command 無法對這個 data 進行修改，直到悲觀鎖被釋放後才能執行。

> 資料表中用 Synchronized 實現的鎖均為悲觀鎖（ex：行鎖，表鎖，讀鎖，寫鎖）。

### 2.2 什麼是樂觀鎖（Optimistic Lock）？

相比悲觀鎖，它認為 Table 裡面的 data 變動頻率不會太頻繁，因此他會允許多個 SQL command 來操作 Table；但樂觀並不代表不負責，通常會在 Table 中增加一個 version 的欄位來做更新的確認。因此當 SQL command 想要變更欄位 data 時，會把之前取出的 version 跟現在的 version 做對比，如果相同就代表這段期間沒人修改可以執行；如果不同這次的操作就不會生效。

### 2.3 在 MySQL DB 實作悲觀鎖

**情境描述：**

假設 A 跟 B 都想要買 iphone 手機，但商城的 iphone 庫存只剩下一隻，在不加鎖的狀況下同時下單會導致庫存變成負號；請問如何用`悲觀鎖`解決這個問題呢？

> 只要安裝好 MySQL，跟著步驟操作即可模擬 A、B 客戶搶購。

- **STEP 1**：建立模擬資料

- 先用 SQL command 建立一個簡單的 Table 並新增 data。

```
CREATE TABLE `your_DB`.`items` (
`id` INT NOT NULL AUTO_INCREMENT,
`name` VARCHAR(45) NOT NULL,
`num` INT NOT NULL,
PRIMARY KEY (`id`));
INSERT INTO `your_DB`.`items` (`name`,`num`)
VALUES ("iphone",1);

```

- 確認資料是否建立成功。

```
SELECT * FROM `your_DB`.`items`;

```

- **STEP 2**：開一個視窗模擬 A 客戶搶購行為

- Transaction 的開始要用 `begin;`，不然 MySQL 會自動提交。

- 在 A 購買前先將 `id=1` 的 data 加上悲觀鎖，此時這行 data 只有 A 可以操作。

```
select num from `your_DB`.`items` where id = 1 for update;

```

- **STEP 3**：開另一個視窗模擬 B 客戶搶購行為

- 一樣將 `id=1` 的 data 加上悲觀鎖，下完指令後便會進入等待的模式，要等 A 執行 commit 完成交易或是指令逾時才能繼續。

- **STEP 4**：回到 A 客戶的視窗完成交易

- 讓 A 執行購買的動作，將 data 的 num 減一。

```
update `your_DB`.`items` set num = num -1 where id = 1;

```

- 然後檢視修改後的 data 是否符合預期。

```
select num from `your_DB`.`items` where id = 1;

```

- 最後下 `commit;` 結束這次交易。

- **STEP 5**：當 A 完成交易後回到 B 的視窗
你會發現因為 A 釋放了悲觀鎖，所以 B 結束等待並且獲得鎖；但 data 的 num 已經變成 0 了，只能放棄購買。

> 7.89 秒是等待獲得鎖的時間。

### 2.4 在 MySQL DB 實作樂觀鎖

**情境描述：**

假設 A 跟 B 都想要買 iphone 手機，但商城的 iphone 庫存只剩下一隻，在不加鎖的狀況下同時下單會導致庫存變成負號；請問如何用`樂觀鎖`解決這個問題呢？

> 觀察兩者實作上的邏輯差異。

- **STEP 1**：建立模擬資料

- 先用 SQL command 建立一個簡單的 Table 並插入 data，比起悲觀鎖的 Table，我們多了一個 version 的欄位。

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

- 確認資料是否建立成功。

```
SELECT * FROM `your_DB`.`happy_items`;

```

- **STEP 2**：規劃實作邏輯思路

- A 跟 B 客戶在購買前會先將 `id=1` 的 data 找出來。
- A 先購買，此時會以 `id=1 和 version=0` 作為 where 條件來更新 data，更新後 num -1、version +1 變成 `id=1、num=0、version=1`。
- B 購買時會因為同樣是用 `id=1 和 version=0` 為 where 條件，所以會因為找不到 data 而無法更新。

- **STEP 3**：模擬搶購情境，A 跟 B 客戶購買前先查詢庫存

- A 及 B 執行查詢的 SQL command 會取得相同的 iphone 庫存資訊。

```
SELECT num,version FROM `your_DB`.`happy_items` where id = 1;

```

- **STEP 4**：A 先執行購買的 SQL command

- A 先購買 iphone 並更新 data。

```
update `your_DB`.`happy_items` set num = num -1 , version = version + 1 where id = 1 and version = 0;

```

- 檢視修改後的 data 是否符合預期。

```
SELECT num,version FROM `your_DB`.`happy_items` where id = 1;

```

- **STEP 5**：B 再執行購買的 SQL command

- 此時 B 的購買就會失敗，在執行購買的 SQL command 後你會發現沒有 Row 被更新，因為庫存資訊已更新，但 SQL command 還是舊的。

## 3. 衍伸問題

### 3.1 悲觀鎖、樂觀鎖優缺點

考點：對於關聯式資料庫「鎖」的認知程度

- **悲觀鎖（Pessimistic Lock）**

- **優點**
使用資料庫 `Transaction` 的機制來強迫執行的順序。
- **缺點**
一但加入 Transaction 的機制，會導致其他的 SQL command 針對這個 data 除了查詢功能外全部卡死，如果這段 Transaction 執行時間較長，會給使用者不良的體驗並造成系統吞吐量下降。

- **樂觀鎖（Optimistic Lock）**

- **優點**
因為沒有在資料庫加鎖，所以 SQL command 都會對 data 進行操作，只有在更新 data 時才會做驗證；這樣做就能避免因使用悲觀鎖而導致吞吐量下降的問題。
- **缺點**
因為樂觀鎖是我們人為實現的，所以如果換一個業務場景可能會不適用，甚至可能因為其他的 SQL command 導致錯誤。

感謝大家的閱讀，如果喜歡我的文章可以`訂閱`接收通知；如果有幫助到你，按`Like`可以讓我更有寫文的動力，我們明天見～

### 參考資源

1. [理解資料庫『悲觀鎖』和『樂觀鎖』的觀念(筆者部落格)](https://medium.com/dean-lin/2cabb858726d#55e3)

> 在許多人的幫助下，本系列文章已成功出版，除了添加新的篇章，更完善了每個案例的應對進退；如果對現在的職涯感到迷茫，也許這本書能帶給你不一樣的觀點～
> **天瓏書局：** [https://www.tenlong.com.tw/products/9786263334571](https://www.tenlong.com.tw/products/9786263334571)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202110/fullstack-career-notes-day-19/
