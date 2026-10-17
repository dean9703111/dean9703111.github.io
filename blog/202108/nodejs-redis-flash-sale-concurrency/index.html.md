# 用 Node.js + Redis 解決高併發秒殺問題

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202108/nodejs-redis-flash-sale-concurrency/
- 發布：2021-08-18（更新：2023-05-05）
- 主題：技術教學
- 標籤：Node.js、Redis、Concurrency、高併發、秒殺
- 摘要：商城在高併發環境下除了要考量到使用者體驗外，還要處理「超賣」的問題；Redis 是一種記憶體資料庫，它有高讀寫效率的特性、能承受高 qps，而這篇文章則是運用它獨特的「鎖」解決超賣問題。

商城在高併發環境下除了要考量到使用者體驗外，還要處理「超賣」的問題；Redis 是一種記憶體資料庫，它有高讀寫效率的特性、能承受高 qps，而這篇文章則是運用它獨特的「鎖」解決超賣問題。

```
大綱
一、文章目標
二、使用技術
  ➤ Redis
  ➤ Lua
三、程式架構
  ➤ 主程式：redisSecKill.js
  ➤ Lua腳本：secKill.lua
四、執行結果
五、補充

```

## 一、文章目標

- 產生秒殺商品基礎資訊(商品名稱、庫存)
- 模擬秒殺情境，確認是否會超賣
- 確認有保存購買人資訊

## 二、使用技術

### Redis

- 如果完全沒有相關基礎，建議先參考[這篇文章](https://deanlin.net/blog/202108/install-redis-on-macos/)來安裝 Redis 以及他的 GUI 工具
- 本篇文章使用 Redis 的 Hash、List type 來儲存資訊

### Lua

- 為了避免超賣，這裡採用 Lua 腳本
- 在 Redis Server 執行 EVAL 指令時，在結果回傳前只會執行當下 Lua 腳本的邏輯，其他 Client 端的命令須等待直到 EVAL 執行完為止。

Lua 腳本的邏輯應盡量簡單以保證執行效率，否則會影響 Client 端的體驗

## 三、程式架構

### 主程式：redisSecKill.js

- 先啟用 Redis Client 端（如未安裝相關套件，請在專案資料夾下輸入 `yarn add ioredis`）
- 用 **prepare** 函式，以 Hash type 建立參加秒殺的產品庫存
- 用 **secKill** 函式模擬使用者購買行為，緩存並執行 Lua 腳本

```
const fs = require('fs');
const Redis = require('ioredis');
const redis = new Redis({
    host: '127.0.0.1',
    port: 6379,
    password: ''
});

redis.on("error", function (error) {
    console.error(error);
});

async function prepare (item_name) {
    // 參加秒殺活動的商品庫存
    await redis.hmset(item_name, "Total", 100, "Booked", 0);
}

const secKillScript = fs.readFileSync('./secKill.lua');

async function secKill (item_name, user_name) {
    // 1. 緩存腳本取得 sha1 值
    const sha1 = await redis.script("load", secKillScript);
    // console.log(sha1);

    // 2. 透過 evalsha 執行腳本
    // redis Evalsha 命令基本語法如下
    // EVALSHA sha1 numkeys key [key ...] arg [arg ...]
    redis.evalsha(sha1, 1, item_name, 1, "order_list", user_name);
}

async function main () {
    console.time('secKill')
    const item_name = "item_name"
    prepare(item_name)
    for (var i = 1; i < 10000; i++) {
        const user_name = "baobao" + i
        secKill(item_name, user_name)
    }
    console.timeEnd('secKill')
}
main()

```

原始碼：[redisSecKill.js](https://gist.github.com/dean9703111/64cfadcce24bc32d5fabbafd07da98a6)（GitHub Gist）

### Lua 腳本：secKill.lua

在 Lua 腳本執行「取得商品庫存」➜「如果庫存足夠就-1」➜「儲存購買者資訊(List type)」

```
local n = tonumber(ARGV[1])
local order_name = ARGV[2]
if not n  or n == 0 then
    return 0
end
local vals = redis.call("HMGET", KEYS[1], "Total", "Booked");
local total = tonumber(vals[1])
local blocked = tonumber(vals[2])
if not total or not blocked then
    return 0
end
if blocked + n <= total then
    redis.call("HINCRBY", KEYS[1], "Booked", n)
    redis.call("LPUSH", order_name,ARGV[3])
    return n;
end
return 0

```

原始碼：[secKill.lua](https://gist.github.com/dean9703111/d5981082cc8c93f1c51fb785a3895f99)（GitHub Gist）

## 四、執行結果

**SETP 1**：在專案資料夾下輸入 `node redisSecKill.js` 模擬秒殺

**SETP 2**：待執行完後，用 Redis GUI 程式確認商品沒有超賣

**SETP 3**：確認訂單都有對應的買家

## 五、補充

上面程式只是 MVP，現實狀況還有很多要討論的：

- 哪些商品被列為秒殺，如何紀錄
- 設定秒殺開始時間、結束時間
- 存在 Redis 的資料怎麼存入關連式資料庫中

```
如果你對 Redis 感興趣，筆者這邊也整理了很多相關資源：
1. 一文讓你徹底瞭解 Redis(進階)，史上最全,不看後悔！！！【建議收藏】
2. 【总结】瞬时高并发（秒杀/活动）Redis 方案
3. redis 之 sorted sets 类型及操作
4. node.js 中对 redis 的安装和基本操作
5. Node.js 中实践 Redis Lua 脚本
6. 使用 Redis 搭建电商秒杀系统

```

筆者也是剛學習 Redis 的新人，如果描述有誤，或是有更好的解決方式，再麻煩大神們不吝賜教，感謝～

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202108/nodejs-redis-flash-sale-concurrency/
