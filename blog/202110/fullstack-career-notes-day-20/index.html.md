# [面試][資料庫]如何解決高併發情境的商品秒殺問題

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202110/fullstack-career-notes-day-20/
- 發布：2021-10-05
- 主題：職涯成長
- 系列：全端工程師生存筆記
- 標籤：iT 邦幫忙鐵人賽、面試、Nosql、Redis、秒殺
- 摘要：如果今天有上萬人在同一時間搶限量商品，昨天分享的方案基本撐不住。

如果今天有`上萬人在同一時間搶限量商品`，昨天分享的方案基本撐不住。

不過面對這個情境，Redis 表示終於輪到我了！今天這篇文章會以 `Node.js + Redis` 為範例，帶讀者一起解決這個問題。

## 大綱

- **如何解決高併發情境的商品秒殺問題**

- 1.1 面試官為什麼會問？
- 1.2 面試官想從答案確認什麼？
- 1.3 筆者提供的簡答

- **回答問題所需具備的知識**

- 2.1 先了解 Redis 的基礎知識
- 2.2 使用 Node.js + Redis 解決高併發秒殺問題
- 2.3 模擬秒殺情境確認商品沒有超賣

- **衍伸問題**

- 3.1 如果有數萬用戶同時查看 TOP100 的點數排行榜，資料庫如何設計？
- 3.2 Redis 跟 Memcached 的差異

## 1. 如何解決高併發商品秒殺問題

### 1.1 面試官為什麼會問？

這算是資料庫的`常見面試題`，除了`金融、電商`喜歡考這題外；`社交平台、新創公司、遊戲產業`也會換個形式考類似的題目。

因為這算是`職缺所需具備的技術`，如果求職者沒有相關經驗可能就會止步於這個關卡。

如果有去以上產業的打算，最好有一定的 NoSQL（ex：Redis、MongoDB） 基礎再去面試；因為在面試官的眼中`有基礎跟完全不會的差距很大`，求職者可以沒有實務經驗，但明知產業會碰到這些技術還不去學習就是態度問題了。

### 1.2 面試官想從答案確認什麼？

- 能提出高併發情境商品秒殺的解決方案
- 如果解決方案為 NoSQL，對它的認知有多少
- 是否接觸過同類型的 NoSQL

### 1.3 筆者提供的簡答

通常這類型的秒殺活動都有固定檔期，我會先將活動用的`商品及庫存數量同步到 Redis 資料庫`；為了避免超賣，在 Client 端下單時會`執行 Lua 腳本`，當秒殺到指定數量或是時間結束後就不再接受請求，活動結束後`將 Redis 的資料同步到關聯式資料庫`。

## 2. 回答問題所需具備的知識

### 2.1 先了解 Redis 的基礎知識

如果有`短時間大量訪問、需要性能提升`的需求，往往會先想到 Redis 這個記憶體資料庫。

- **Redis 為什麼快？**

- **記憶體資料庫**
記憶體讀寫本來就快。

- **單執行緒（single-threaded）**
如果使用`多執行緒`會耗費時間在`上下文的切換`以及`加鎖`上面；而`單執行緒`會依照`請求順序執行`，不需考慮同步以及加鎖帶來的效能問題。
Redis 是基於記憶體操作，所以效能的瓶頸不在 CPU 而在記憶體、網路頻寬；因為 CPU 不是瓶頸，所以可以`開多個 Redis 建立 Cluster` 分散壓力。

- **I/O 多路複用（multiplexing）**
Redis 使用了 `epoll IO 多路複用`，可以用一條執行緒處理併發的網路請求。

**這個有點抽象，我用`繳交作業`為情境說明**：
假設你是一個老師，採用多路復用的原則批改作業，那你批改的順序就是`看誰先繳交作業`，而不是按照學號順序批改（此作法中間有人沒交作業就會卡住）；這樣就能避免大量無用操作，為非阻塞模式的實現。

- **Redis 資料保存方案**
可以使用 `RDB 、AOF` 來做持久化。

- **RDB 持久化（Redis Database）**
在指定的時間間隔內將資料 dump 到硬碟；因為是`定期操作`，如果 Redis `當機會遺失部分資料`，此方案適合`大規模資料恢復`。
- **AOF 持久化（Append Only File）**
這個方案可以`完整紀錄`所有資料的變化，因為採用日誌追加的方式，所以`就算當機也不會影響已經儲存的日誌，災難復原的完成度高`；缺點是`檔案比 RDB 大`、大規模資料`恢復速度較 RDB 慢`。

- **Redis 資料淘汰機制**
Redis 主要`保存的都是熱點資訊`，在儲存資料有限的狀態下（記憶體不足，無法寫入新資料）；就要合理的設定淘汰機制：

- 選擇性移除有設定`過期時間`的資料

- **volatile-lru**：挑選最近較少使用的資料淘汰。
- **volatile-ttl**：挑選即將過期的資料淘汰。
- **volatile-random**：挑選隨機資料淘汰。

- **allkeys-lru**：挑選最近較少使用的資料淘汰。
- **allkeys-random**：挑選隨機資料淘汰。
- **no-enviction**：禁止淘汰；若選用這個設定，當數據到達 maxmemory 時會回傳 OOM 錯誤。

原則上淘汰策略以「`volatile-lru、allkeys-lru`」為主（淘汰較少使用的資料）。

### 2.2 使用 Node.js + Redis 解決高併發秒殺問題

- **目標**

- 產生秒殺商品基礎資訊（商品名稱、庫存）
- 模擬秒殺情境，確認是否會超賣
- 確認有保存購買人資訊

- **使用技術**

- **Redis**
本篇文章使用 Redis 的 Hash、List type 來儲存資訊。

如果完全沒有相關基礎，建議先參考[這篇文章](https://medium.com/dean-lin/8d0b45062f9)來安裝 Redis 以及它的 GUI 工具。

- **Lua 腳本**
為了避免超賣，這裡採用 Lua 腳本；`在 Redis Server 執行 EVAL 指令時，在結果回傳前只會執行當下 Lua 腳本的邏輯`，其他 Client 端的命令須等待直到 EVAL 執行完為止。

Lua 腳本的邏輯應盡量簡單以保證執行效率，否則會影響 Client 端的體驗。

- **程式架構**

- **主程式：redisSecKill.js**

- 先啟用 Redis Client 端（如未安裝相關套件，請在專案資料夾下輸入 `yarn add ioredis`）。
- 用 `prepare` 函式，以 Hash type 建立參加秒殺的產品庫存。
- 用 `secKill` 函式模擬使用者購買行為，緩存並執行 Lua 腳本。

```
const fs = require("fs");
const Redis = require("ioredis");
const redis = new Redis({
  host: "127.0.0.1",
  port: 6379,
  password: "",
});

redis.on("error", function (error) {
  console.error(error);
});

async function prepare(item_name) {
  // 參加秒殺活動的商品庫存
  await redis.hmset(item_name, "Total", 100, "Booked", 0);
}

const secKillScript = fs.readFileSync("./secKill.lua");

async function secKill(item_name, user_name) {
  // 1. 緩存腳本取得 sha1 值
  const sha1 = await redis.script("load", secKillScript);
  // console.log(sha1);

  // 2. 透過 evalsha 執行腳本
  // redis Evalsha 命令基本語法如下
  // EVALSHA sha1 numkeys key [key ...] arg [arg ...]
  redis.evalsha(sha1, 1, item_name, 1, "order_list", user_name);
}

function main() {
  console.time("secKill");
  const item_name = "item_name";
  prepare(item_name);
  for (var i = 1; i < 10000; i++) {
    const user_name = "baobao" + i;
    secKill(item_name, user_name);
  }
  console.timeEnd("secKill");
}
main();

```

- **Lua 腳本：secKill.lua**
在 Lua 腳本執行邏輯：「確認下單數量」➜「取得商品庫存」➜「如果庫存足夠就下單」➜「儲存購買者資訊（List type）」。

```
local item_name = KEYS[1]
local n = tonumber(ARGV[1])
local order_list = ARGV[2]
local user_name = ARGV[3]
if not n  or n == 0 then
  return 0
end
local vals = redis.call("HMGET", item_name, "Total", "Booked");
local total = tonumber(vals[1])
local booked = tonumber(vals[2])
if not total or not booked then
  return 0
end
if booked + n <= total then
  redis.call("HINCRBY", item_name, "Booked", n)
  redis.call("LPUSH", order_list, user_name)
  return n
end
return 0

```

### 2.3 模擬秒殺情境確認商品沒有超賣

- **SETP 1**：在專案資料夾下輸入 `node redisSecKill.js` 模擬秒殺

- **SETP 2**：待執行完後，用 Redis GUI 程式確認商品沒有超賣

- **SETP 3**：確認訂單都有對應的買家

**補充**
文章程式只是 MVP，現實狀況還有很多要設計的：

- 哪些商品被列為秒殺，如何紀錄。
- 設定秒殺開始、結束時間。
- 如何將 Redis 資料存入關聯式資料庫中。

## 3. 衍伸問題

### 3.1 如果有數萬用戶同時查看 TOP100 的點數排行榜，資料庫如何設計？

`考點：對 Redis Zset 這個資料型態的認知與應用`

我會使用 Redis 這個記憶體資料庫，建立一個`有序集合（Zset）`來儲存用戶的資訊。

在設計上，用戶（member）是唯一值，且每個用戶都會關聯一個點數（score），這樣用戶就可以按照分數來排序。

功能實現上會透過 `zrevrange Leaderboard 0 99 withscores` 這段指令來顯示 TOP100 的點數排行榜。

- **zrevrange**：依照點數（score）檢視排行榜
- **Leaderboard**：排行榜名稱（可以自行定義）
- **0 99**：TOP100 的意思
- **withscores**：連點數（score）一起顯示

### 3.2 Redis 跟 Memcached 的差異

`考點：是否了解過同類型的技術以及差異`

- Memcached 只支援`簡單的資料型態`，而 Redis `支援多種資料型態`（String、Hash、List、Set、Zset、Stream）。
- Memcached `不支援資料持久化`，而 Redis `提供 RDB 跟 AOF 兩種方案`。

同樣身為記憶體資料庫，Memcached 提供簡單的使用方式，而 Redis 提供豐富的功能。

感謝大家的閱讀，如果喜歡我的文章可以`訂閱`接收通知；如果有幫助到你，按`Like`可以讓我更有寫文的動力，我們明天見～

### 參考資源

- [手把手帶你在 MacOS 安裝 Redis ＆Another Redis Desktop Manager(筆者部落格)](https://medium.com/dean-lin/8d0b45062f9)
- [用 Node.js + Redis 解決高併發秒殺問題(筆者部落格)](https://medium.com/dean-lin/e814fe26a0f2)
- [redis 之 sorted sets 類型及操作](https://www.huaweicloud.com/articles/6785018d60d6272a49565cb148d661af.html)
- [使用 redis 的有序集合實現排行榜功能](https://www.gushiciku.cn/pl/2MPj/zh-tw)

在許多人的幫助下，本系列文章已成功出版，除了添加新的篇章，更完善了每個案例的應對進退；如果對現在的職涯感到迷茫，也許這本書能帶給你不一樣的觀點～

**天瓏書局：** [https://www.tenlong.com.tw/products/9786263334571](https://www.tenlong.com.tw/products/9786263334571)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202110/fullstack-career-notes-day-20/
