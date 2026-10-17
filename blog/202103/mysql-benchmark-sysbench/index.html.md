# 用 sysbench 測試 MySQL 性能 — 在 mac & ubuntu 上的安裝與使用

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202103/mysql-benchmark-sysbench/
- 發布：2021-03-29（更新：2023-05-05）
- 主題：技術教學
- 系列：MySQL 主從架構
- 標籤：Sysbench、MySQL、MySQL 效能、DB 效能測試、安裝教學
- 摘要：sysbench 是我目前使用上最順手的一個測試工具，安裝、使用都非常容易，除了資料庫的測試外他還能對 CPU、Memory…做測試，本篇會詳細說明如何使用它來做 MySQL 性能測試，其他的功能可以參考文末附的連結。

sysbench 是我目前使用上最順手的一個測試工具，安裝、使用都非常容易，除了資料庫的測試外他還能對 CPU、Memory…做測試，本篇會詳細說明如何使用它來做 MySQL 性能測試，其他的功能可以參考文末附的連結。

## 一、在 Mac 上操作 sysbench

### ➤ 安裝 sysbench

如果還沒安裝過 brew 請[參考官網](https://brew.sh/index_zh-tw)

```
brew install sysbench

```

安裝完成後輸入指令確認安裝完成

```
sysbench — version

```

我安裝的版本為 `1.0.20` 這個會跟下面的指令路徑有關，如果你安裝的是其他版本要自行修改這個版本路徑

### ➤ 測試 MySQL 性能

mac 的範例是本地端的測試，針對 read 這塊

**STEP 1**：先建立好測試用的 DB

```
create database test;

```

**STEP 2**：建立測試資料

- **mysql-user、mysql-password**：改成自己 MySQL 的使用者
- **mysql-db**：測試的資料庫
- **range_size、table_size、tables**：測試的規模
- **prepare**：建立測資的意思

```
sysbench /usr/local/Cellar/sysbench/1.0.20/share/sysbench/oltp_read_only.lua \
--db-driver=mysql \
--mysql-user=user_name \
--mysql-password=user_password \
--mysql-db=test \
--range_size=100 \
--table_size=10000 \
--tables=2 \
--threads=1 \
--events=0 \
--time=60 \
--rand-type=uniform \
prepare

```

原始碼：[sysbench_prepare.cmd](https://gist.github.com/dean9703111/d26df5b62cc8b790085ef4d6ee468883)（GitHub Gist）

**STEP 3**：進行測試

只是把指令最後面改成 run

```
sysbench /usr/local/Cellar/sysbench/1.0.20/share/sysbench/oltp_read_only.lua \
--db-driver=mysql \
--mysql-user=user_name \
--mysql-password=user_password \
--mysql-db=test \
--range_size=100 \
--table_size=10000 \
--tables=2 \
--threads=1 \
--events=0 \
--time=60 \
--rand-type=uniform \
run

```

原始碼：[sysbench_run.cmd](https://gist.github.com/dean9703111/a3093daa95c2b7cae4dd327fcae4d9ba)（GitHub Gist）

**STEP 4**：清除測試數據

只是把指令最後面改成 cleanup

```
sysbench /usr/local/Cellar/sysbench/1.0.20/share/sysbench/oltp_read_only.lua \
--db-driver=mysql \
--mysql-user=user_name \
--mysql-password=user_password \
--mysql-db=test \
--range_size=100 \
--table_size=10000 \
--tables=2 \
--threads=1 \
--events=0 \
--time=60 \
--rand-type=uniform \
cleanup

```

原始碼：[sysbench_cleanup.cmd](https://gist.github.com/dean9703111/24b62f7153920bd3fb4773d64f461945)（GitHub Gist）

## 一、在 Ubuntu 上操作 sysbench

### ➤ 安裝 sysbench

```
sudo apt-get install sysbench -y

```

安裝完成後輸入指令確認安裝完成

```
sysbench — version

```

### ➤ 測試 MySQL 性能

ubuntu 的範例是遠端的測試，針對 write這塊

**STEP 1**：先建立好測試用的 DB

```
create database test;

```

**STEP 2**：建立測試資料

如果你的設定跟我的範例一樣可能要稍微等一下，因為範例的測資比較大筆

- **mysql-host**：目標測試的 ip 或是 host name
- **mysql-port**：目標測試的 MySQL port
- **mysql-user、mysql-password**：改成自己 MySQL 的使用者
- **mysql-db**：測試的資料庫
- **range_size、table_size、tables**：測試的規模
- **prepare**：建立測資的意思

```
sysbench /usr/share/sysbench/oltp_write_only.lua \
--mysql-host=127.0.0.1 \
--mysql-port=3306 \
--mysql-user=user_name \
--mysql-password=user_password \
--mysql-db=test \
--db-driver=mysql \
--tables=10 \
--table_size=10000 \
--report-interval=10 \
--threads=128 \
--time=120 \
prepare

```

原始碼：[ubuntu_sysbench_prepare.cmd](https://gist.github.com/dean9703111/4adca32bb1877a41e51c23ff3ff127bf)（GitHub Gist）

**STEP 3**：進行測試

只是把指令最後面改成 run

```
sysbench /usr/share/sysbench/oltp_write_only.lua \
--mysql-host=127.0.0.1 \
--mysql-port=3306 \
--mysql-user=user_name \
--mysql-password=user_password \
--mysql-db=test \
--db-driver=mysql \
--tables=10 \
--table_size=10000 \
--report-interval=10 \
--threads=128 \
--time=120 \
run

```

原始碼：[ubuntu_sysbench_run.cmd](https://gist.github.com/dean9703111/24b504d1f2056b6645b63a55ac2856c4)（GitHub Gist）

**STEP 4**：清除測試數據

只是把指令最後面改成 cleanup

```
sysbench /usr/share/sysbench/oltp_write_only.lua \
--mysql-host=127.0.0.1 \
--mysql-port=3306 \
--mysql-user=user_name \
--mysql-password=user_password \
--mysql-db=test \
--db-driver=mysql \
--tables=10 \
--table_size=10000 \
--report-interval=10 \
--threads=128 \
--time=120 \
cleanup

```

原始碼：[ubuntu_sysbench_cleanup.cmd](https://gist.github.com/dean9703111/957b162ca5a0bbe57a86e7c792e2a085)（GitHub Gist）

以上是今天的分享，想了解更多功能請參考[官方的 github](https://github.com/akopytov/sysbench)

**參考資源**：[Ubuntu 16.04中進行Sysbench環境搭建](https://www.itread01.com/content/1549085775.html)、[sysbench安装和使用](https://blog.51cto.com/13990437/2400940)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202103/mysql-benchmark-sysbench/
