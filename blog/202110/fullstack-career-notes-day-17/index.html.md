# [面試][資料庫]設計資料庫時會考量哪些點？

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202110/fullstack-career-notes-day-17/
- 發布：2021-10-02
- 主題：職涯成長
- 系列：全端工程師生存筆記
- 標籤：iT 邦幫忙鐵人賽、面試、資料庫、MySQL Vs Postgresql、index設計
- 摘要：如果公司有專門的 DBA，後端工程師就可以把心力放在資料邏輯的處理上；但筆者詢問周圍的朋友，似乎有不少公司都是讓後端工程師兼任 DBA...

> 好的設計讓你上天堂，爛的設計讓你下地獄。

如果公司有專門的 DBA，後端工程師就可以把心力放在資料邏輯的處理上；但筆者詢問周圍的朋友，似乎有不少公司都是讓後端工程師兼任 DBA…

筆者在這塊也並非專家，主要是分享自己實作中會注意的基礎細節，避免犯下一些原則性錯誤。

## 大綱

1. **設計資料庫時會考量哪些點？**

2. 1.1 面試官為什麼會問？
3. 1.2 面試官想從答案確認什麼？
4. 1.3 筆者提供的簡答

5. **回答問題所需具備的知識**

6. 2.1 在 Table 建立有效的 Index
7. 2.2 資料庫設計與優化
8. 2.3 MySQL 與 PostgreSQL 的區別及應用場景

9. **衍伸問題**

10. 3.1 為什麼 Index 不能建立太多？
11. 3.2 寫 DB SQL 時會注意什麼？
12. 3.3 有用過 NoSQL 嗎？什麼應用情境下會使用呢？
13. 3.4 你會用哪些點來評估要使用關聯式資料庫 or 非關聯式資料庫？

## 1. 設計資料庫時會考量哪些點？

### 1.1 面試官為什麼會問？

- 因為履歷資訊中表明擅長多種資料庫（Database）

- MySQL、MSSQL、PostgreSQL

- 且履歷資訊中還表明有優化資料庫的經驗。

- **XXX 資料庫系統**
將 Visual Basic 的單機版程式改寫成網頁系統，整合過去冗贅資料表，優化使用者體驗。

### 1.2 面試官想從答案確認什麼？

- 選用資料庫前，會思考哪些問題
- 設計資料庫時會注意哪些細節
- 同樣都是關聯式資料庫，你知道他們間的差異嗎？
- 有用過哪些方式優化資料庫效能
- 你對 NoSQL 的認知有多少

### 1.3 筆者提供的簡答

在決定要使用什麼資料庫前，我會先分析專案的應用場景：

- 像是`商城`會有金流，所以資料穩定性與 Transaction 機制是很重要的。
- 如果要設計地圖平台，就會選擇對 GIS 支援度高的資料庫。

在設計資料庫時會畫 `ER Model` 來分析資料間的關聯與屬性；在優化方面，為了要提高搜尋效率會在 Table `建立 Index`，同時也會依據實際業務設計欄位適合的長度及 Type。

## 2. 回答問題所需具備的知識

### 2.1 在 Table 建立有效的 Index

我們知道在 Table 建立 Index 可以增加搜尋效率，但你知道自己建立的 Index 有沒有派上用場嗎？筆者將在這個小節透過實作，帶大家了解 `Coverage Index (覆蓋索引)`的意義。

- **SETP 1**：建立測試用的 **tmp_user** Table

- **id** 為 primary key
- **name** 為 index

```
create table tmp_user (
    id bigint(20) not null auto_increment ,
    name varchar(255) not null,
    password varchar(255) ,
    index index_name (name),
    primary key (id))
    engine=innodb
    default character set=utf8 collate=utf8_general_ci;

insert into tmp_user (name, password) values ("寶寶不說","123");

```

- **SETP 2**：下 SQL 指令了解是否使用 Index

> 觀察最後「Extra」欄位的資訊有否有 `Using index`

- **SQL A**：`explain select id from user_table where name= '寶寶不說';`

因為 `name` 有建立 index，所以在查詢到 `name` 後可以直接與 `id` 對應返回結果；因為 name 已經覆蓋了查詢資料的需求，所以稱為 「Coverage Index」。

- **SQL B**：`explain select password from user_table where name= '寶寶不說';`

透過 `name` 找到「寶寶不說」對應的 `id`，但接著要透過 id 這個 `primary key`，在 Table 重新輪詢才能取得這一列的完整資料。

- **SETP 3**：建立 Coverage Index 優化 SQL B

- **先將資料庫 drop 掉**：`drop table tmp_user;`

- **重新建立測試資料以及「Coverage Index」**
為了避免用 primary key 再次從 Table 查詢的耗能，我們可以`把 select 與 where 用到的欄位(name、password)做聯合索引`，這樣就能夠直接使用索引查詢，而不需用 primary key 重新比對，這就是「Coverage Index」的應用。

```
create table tmp_user (
  id bigint(20) not null auto_increment ,
  name varchar(255) not null,
  password varchar(255) ,
  primary key (id))
  engine=innodb
  default character set=utf8 collate=utf8_general_ci;

insert into tmp_user (name, password) values ("寶寶不說","123");

alter table tmp_user add index name_password(name,password);

```

- **再次執行 SQL B**：`explain select password from tmp_user where name = '寶寶不說';`
本次的查詢就可以在「Extra」欄位看到 `Using index` 嚕～

### 2.2 資料庫設計與優化

- **使用 ER Model 做規畫**
在需求規格出來後，我們可以先透過 ER Model 整理資料間的關聯性，然後以此為基礎設計資料庫；向新人說明 Table 間的關聯時，它也是一個非常棒的輔助工具。
- **設計合適的文字欄位**

- 如果不確定該欄位會填入的文字長度，會選擇 VARCHAR 這種依據文字長度來使用儲存空間的 Type。
- 以時常變更文字的欄位來說，會選擇 CHAR 而非 VARCHAR；因為 VARCHAR 的欄位在儲存時會需要額外的計算。
- 盡量`避免使用 TEXT/BLOB` 這類的 Type，它們在查詢時會消耗更多的效能。
- 根據實際業務需求設計欄位長度，

- **減少 SQL 寫入次數**
以 Insert 來做舉例，假設有 1000 筆資料要寫入 Table；一次插入 1000 筆的執行時間會遠遠小於分 1000 次插入的時間。
- **適當的增加冗餘欄位**
在設計資料庫時要盡量符合三大正規化；但 Table 間的關聯越複雜也會導致查詢效率的下降，因此有時會適當的增加冗餘欄位，用犧牲磁碟空間的方式換取更高的查詢效率。
- **定期審視資料庫的規劃**
如果沒有專門的 DBA，那資料庫通常會有以下問題需要修正：

- 許多欄位因為不確定實際業務需求，直接把欄位長度開到最大。
- 因為新需求不斷產生，在思慮不夠周密的狀況下容易新增許多無用欄位。
- 在系統版本的更迭中，沒有整理不符合實際業務需求的`檢視表（View）`。
- 後端存取 Table 的欄位改變，但 Table 的 Index 並沒有一起調整，導致許多 Index 並沒有發揮作用。

### 2.3 MySQL 與 PostgreSQL 的區別及應用場景

`MySQL` 是世界上最流行的資料庫之一，世界上有非常多的 Web Application 都採用它作為解決方案；但在被收購後分出了非常多的版本，有免費也有收費的；而 `PostgreSQL` 基於 BSD/MIT 的 linecense，為完全開源的資料庫。

- **PostgreSQL 的優勢**

- **不僅是關聯式資料庫，還支援 jsonb 的格式**
相比於 json，jsonb 以二進位格式儲存且可以使用索引（Index），你可以利用它整合關聯式資料與非關聯式資料。
- **有 PostGIS 讓他成為很棒的空間資料庫**
相比於 MySQL spatial extension，PostGIS 可以支持「二維、三維、曲線」的空間類型，並有豐富的空間操作函數。
- **可以輕鬆與外部資料庫關聯**
透過 FDW（Foreign Data Wrapper），你可以把外部資料庫（ex：MySQL、Oracle、CSV）當成自己資料庫中的 Table 查詢，讓 PostgreSQL 成為聯合資料庫。

- **MySQL 的優勢**

- **會 MySQL 的工程師更多**
在招募工程師時更容易，遇到問題時也有豐富的社群資源提供解答。
- **MySQL 使用 Thread 而 PostgreSQL 使用 Process**

- Process 比 Thread 要花更多時間建立連線。

- Process 因為缺失 Thread 實作的「共享」特性，使得每次連線建立時，都需要耗費較高的記憶體空間配置。

> PostgreSQL 可以搭配 Connection Pool 的外掛解決這個問題，但架構上就會變更複雜。

- **應用場景**

- **MySQL**
適合業務邏輯相對簡單、較少處理資料一致化問題的網際網路場景。
- **PostgreSQL**
專案資料寫入頻繁，且資料庫龐大而複雜，內容需要高度一致性，有 GIS 需求。

> 其實在 DB 的選擇上，除了考慮專案適用的場景外，還要考量到團隊內的工程師對哪個比較擅長；思考萬一發生意外時有誰可以救場，不然災難復原是一場惡夢。

## 3. **衍伸問題**

### 3.1 為什麼 Index 不能建立太多？

考點：了解 Index 對資料庫效能的影響

Index 的數量太多，會影嚮 DML（Insert、Update、Delete）的效能，因為 Table 資料更新時也要連帶更新 Index。

### 3.2 寫 DB SQL 時會注意什麼？

考點：確認求職者的 SQL 基本功

- 不要回傳大量資料，要有條件的回傳固定筆數（ex：limit）。
- Select 時盡量不要使用「*」，只抓取需要的欄位。
- Select 充分使用 `Index` 來做搜尋。
- 避免使用不兼容的型態做比較，像是 FLOAT 跟 INT、CHAR 跟 VARCHAR。

### 3.3 有用過 NoSQL 嗎？什麼應用情境下會使用呢？

考點：確認求職者對 NoSQL 的認知

> 即便你的履歷中沒有寫自己會 NoSQL，還是會遇到這類考題，所以建議至少有基礎的認知。

- **資料表欄位常常變化**
像是商城這種擁有龐大資料量的系統，在關連式資料庫中如果要 Update 欄位有很多顧慮；因為每個操作都需要重頭到尾輪詢一次 Table，非常消耗時間。
如果使用 `MongoDB` 這類的 NoSQL，就可以很靈活的對欄位進行調整。
- **需要高速的讀寫**
像是 `Redis` 這類記憶體資料庫，單一節點就能應付每秒 10 萬次的讀寫請求。
- **資料量龐大，有擴充需求**
NoSQL 具備水平擴充能力，只要增加新的伺服器節點就能擴充資料庫容量；且對於伺服器的性能要求較低，可使用`較便宜`的電腦進行擴充。

### 3.4 你會用哪些點來評估要使用關聯式資料庫 or 非關聯式資料庫？

考點：確認求職者是否有評估的能力與經驗

我會考慮的點有資料量多寡、併發量、實時性、資料一致性、未來擴充性、資料穩定性、維護成本。

以企業內部管理系統來說，因為資料量少、併發數少、未來擴充需求較低，所以會選擇關連式資料庫；如果是遊戲的排行榜頁面，因為資料量大、併發數高、且要求高的更新頻率，所以我會考慮使用記憶體資料庫（ex：Redis）。

感謝大家的閱讀，如果喜歡我的文章可以`訂閱`接收通知；如果有幫助到你，按`Like`可以讓我更有寫文的動力，我們明天見～

### 參考資源

1. [MySQL 資料庫面試題](https://www.gushiciku.cn/pl/poXE/zh-tw)
2. [你在 Table 建立的 Index 真的有效嗎？用範例帶你理解 Coverage Index 的意義(筆者部落格)](https://medium.com/dean-lin/f852af308c27)
3. [MySQL 與 PostgreSQL 相比哪個更好？](https://zi.media/@yidianzixun/post/FaABsS)

> 在許多人的幫助下，本系列文章已成功出版，除了添加新的篇章，更完善了每個案例的應對進退；如果對現在的職涯感到迷茫，也許這本書能帶給你不一樣的觀點～
> **天瓏書局：** [https://www.tenlong.com.tw/products/9786263334571](https://www.tenlong.com.tw/products/9786263334571)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202110/fullstack-career-notes-day-17/
