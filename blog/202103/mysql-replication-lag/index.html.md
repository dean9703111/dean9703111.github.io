# 解決 MySQL 主從複製的延遲問題 (Replication lag)

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202103/mysql-replication-lag/
- 發布：2021-03-29
- 主題：技術教學
- 系列：MySQL 主從架構
- 標籤：MySQL、主從複製、主從複製延遲問題、Replication Lag、Sysbench
- 摘要：Slave DB 上的資料從 Master DB 複製過來時會有延遲，所以會造成用戶看到舊的資訊；在過去想解決這個問題真的需要 18 般武藝，但到了 MySQL 5.7 版後官方說只要調整一個設定就能解決，具體做法讓我們看下去。

在『MySQL Master Slave Replication 主從式架構設定教學』這篇文章的結尾有提到因為 Slave DB 上的資料從 Master DB 複製過來時會有延遲，所以會造成用戶看到舊的資訊；在過去想解決這個問題真的需要 18 般武藝，但到了 MySQL 5.7 版後官方說只要調整一個設定就能解決，具體做法讓我們看下去。

```
一、模擬主從複製延遲的情境
二、了解發生問題的原因
三、解決方案：開啟 MySQL 5.7 的多 process 的功能
四、確認 Slave 已經沒有延遲

```

## 一、模擬主從複製延遲的情境

> 這裡我們利用 sysbench 進行情境模擬，如果不清楚如何操作的朋友請參考這篇文章：『[用 sysbench 測試 MySQL 性能 — 在 mac & ubuntu 上的安裝與使用](https://deanlin.net/blog/202103/mysql-benchmark-sysbench/)』

下面是我在 ubuntu 的測試指令，如果你是 mac 需要自己更換 .lua 的路徑

```
sysbench /usr/share/sysbench/oltp_write_only.lua \
--mysql-host=127.0.0.1 \
--mysql-port=3306 \
--mysql-user=user_name \
--mysql-password=user_password \
--mysql-db=test \
--db-driver=mysql \
--tables=10 \
--table_size=100000 \
--report-interval=10 \
--threads=128 \
--time=120 \
prepare

```

原始碼：[sysbench_lag.cmd](https://gist.github.com/dean9703111/46688419066279598ee33553f5ae9c45)（GitHub Gist）

指令輸入後你就會發現資料庫好像頓住不會動了…

一段時間後我們進入到 Slave 的 MySQL 中會發現他與 Master 的延遲很嚴重

```
mysql> show slave status\G

```

- **Seconds_Behind_Master**：延遲的時間

## 二、了解發生問題的原因

因為我們模擬的情境會製作出相當大量的 SQL command，執行內容包含：table 內容大量更新、index 建立；可是 Slave 去讀取 Master 的 process 卻只有一個，這樣使 Master 累積了太多尚未同步到 Slave 的 SQL command，因此造成了兩邊資料庫的內容不一致。

## 三、解決方案：開啟 MySQL 5.7 的多 process 的功能

**STEP 1**：先暫停 Slave，不然無法修改變數

```
mysql> stop slave;

```

**STEP 2**：更改 slave_parallel_type

先看看預設值

```
mysql> show variables like ‘slave_parallel%’;

```

修改變數

```
mysql> set global slave_parallel_type='logical_clock';
mysql> set global slave_parallel_workers=10; #根據實際需求設定

```

啟動 Slave 功能

```
mysql> start slave;

```

查看剛剛設定的多 process 是否生效

```
mysql> show processlist;

```

## 四、確認 Slave 已經沒有延遲

在上面的設定後稍等一段時間再去查看 Slave 的延遲狀態

```
mysql> show slave status\G

```

就發現他已經變成 0 嚕～

```
如果你想了解資料庫主從式架構的設定歡迎參考我的文章：
👍 MySQL Master Slave Replication 主從式架構設定教學
👍 Ubuntu 用 Atlas 實現 MySQL Proxy 讀寫分離 read/write splitting

```

**參考資訊**：[mysql master-slave 複寫延滯同步問題](https://jerryw.blog/2021/01/07/mysql-master-slave-%E8%A4%87%E5%AF%AB%E5%BB%B6%E6%BB%AF%E5%90%8C%E6%AD%A5%E5%95%8F%E9%A1%8C/)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202103/mysql-replication-lag/
