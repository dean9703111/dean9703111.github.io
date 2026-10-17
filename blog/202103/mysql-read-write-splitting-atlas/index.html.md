# Ubuntu 用 Atlas 實現 MySQL Proxy 讀寫分離 read/write splitting

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202103/mysql-read-write-splitting-atlas/
- 發布：2021-03-29
- 主題：技術教學
- 系列：MySQL 主從架構
- 標籤：Atlas、Ubuntu、MySQL、讀寫分離、Rw Splitting
- 摘要：如果單純把 DB 分成 Master 及 Slave 並不能真正解決平衡負載問題，因為每台 server 是獨立的 ip，使用者在存取資料庫的時候並不知道要使用哪一台 server 😅；這篇文章會教你如何設定一台 Proxy server，準確地將 SQL 指令正確分流

在『[MySQL Master Slave Replication 主從式架構設定教學](https://deanlin.net/blog/202103/mysql-master-slave-replication/)』這篇文章的結尾有提到如果單純把 DB 分成 Master 及 Slave 並不能真正解決平衡負載問題，因為每台 server 是獨立的 ip，使用者在存取資料庫的時候並不知道要使用哪一台 server 😅 ，除非你的 Slave server 只是用來做資料分析 🤔；這篇文章會教你如何設定一台 Proxy server，準確地將 SQL 指令分流到正確的 server。

  此為設計架構圖

```
一、前置作業
二、MySQL Proxy 的選擇
三、Atlas 功能介紹
四、在 Ubunut 安裝 Atlas
五、連到 Atlas 管理 server
六、實際測試透過 Atlas Proxy 是否達成讀寫分離

```

## 一、前置作業

> OS version、MySQL 是筆者安裝時的環境，ip 是給大家參考對應的

### 準備一台要當 MySQL Proxy 的 ubuntu server

- OS version：ubuntu 16.04
- MySQL：5.7
- ip ：172.16.9.39

### 準備一台 Master server 負責寫入

- OS version：ubuntu 16.04
- MySQL：5.7
- ip ：172.16.9.32

### 準備一台以上的 Slave server 負責讀取

> 準備兩台以上測試 load balance 才會準確，這塊有興趣的讀者可以自己嘗試

- OS version：ubuntu 16.04
- MySQL：5.7
- ip ：172.16.9.32

### 在 Master Server 裡面增加給 Proxy server 使用的使用者，並創建測試用 table

```
/*建立proxy server使用者*/
mysql> CREATE USER 'proxy_account'@'%' IDENTIFIED BY 'proxy_password';
/*給使用者權限*/
mysql> GRANT ALL ON *.* TO 'proxy_account'@'%';
/*使權限生效*/
mysql> FLUSH PRIVILEGES;
/*創建測試用資料庫*/
CREATE DATABASE proxy_test;

```

原始碼：[create_proxy_user.sql](https://gist.github.com/dean9703111/3c818b9e9a1294cd7e33f108985884e0)（GitHub Gist）

## 二、MySQL Proxy 的選擇

- **MySQL Proxy**：原本想使用官方工具來處理讀寫分離，但一上**官網發現連他自己都不推薦使用**😱，所以就不在多費心力研究。

- **MySQL Router**：根據官方的指引認真的研究這個工具後發現雖然設定上簡單，但並不是很符合需求，他的**讀寫分離是同一台伺服器分成兩個 port 來處理**，雖然現在主流框架都支援這樣的操作，但我認為這樣的方式並不優雅
- **kingshard**：是基於 Go 語言開發的 MySQL Proxy，在 github 上面擁有高人氣；但實際使用心得真的是滿滿的陰影…**官方安裝指引非常的爛**，鬼才知道要自己切換 branch 才能使用啊？設定檔根本要用錯誤嘗試法，沒說清楚必填選填，上面這些問題也就罷了，安裝成功後居然告訴我只有 windows 的電腦可以測試 [mac 電腦不行](https://github.com/flike/kingshard/issues/591#issuecomment-776367532)😡，作者是在開玩笑嗎？總之我非常的不推薦。
- **Atlas**：是基於 MySQL-Proxy 0.8.2 版本開發的工具，由 360 公司推出，在 github 上面找到最新的版本是 2015 年推出的，至於為什麼不再更新的理由，360 公司表示因為這個工具已經夠棒了🥴，沒什麼繼續優化的必要性了；事實上這個工具也是筆者最後選用的，儘管安裝過程滿滿的坑😓

## 三、Atlas 功能介紹

1. 讀寫分離
2. slave server 的 load balance
3. IP filter
4. DBA 可以輕鬆的上下線 DB（有指令支持）
5. 自動移除當機的 DB（但如果當機的是 master server，就只剩讀取功能）

## 四、在 Ubunut 安裝 Atlas

> 筆者在安裝過程中感受到滿滿的惡意

**STEP 1**：下載 & 安裝 Atlas

你可以在官方 [github release ](https://github.com/Qihoo360/Atlas/releases)中找到 deb 的安裝包下載他

```
wget https://github.com/Qihoo360/Atlas/releases/download/2.2/Atlas-2.2-debian6.0-x86_64.deb

```

執行安裝

```
dpkg -i Atlas-2.2-debian6.0-x86_64.deb

```

**STEP 2**：調整 Atlas 的設定檔

Atlas 的安裝路徑是/usr/local/mysql-proxy ，執行時的參數會參考 conf 目錄下的 test.cnf 檔案

```
vi /usr/local/mysql-proxy/conf/test.cnf

```

下面幾個需要特別調整，其他用默認的就可以了

- **proxy-backend-addresses**：填寫 Master server 的 ip

- **proxy-read-only-backend-addresses**：填寫 Slave server 的 ip

- **pwds**：請使用於前置作業 Master server 新增專門給 Proxy Server 使用的帳號密碼，密碼要用下面的指令作加密

```
# 前往加密的目錄
cd /usr/local/mysql-proxy/bin
# 對密碼加密
./encrypt proxy_password

```

- **sql-log**：預設是 OFF 不會紀錄，但為了方便觀察讀寫是否分離，我們將它設定為 REALTIME

**STEP 3**：啟動 Atlas

輸入下面指令即可啟動 Atlas

```
/usr/local/mysql-proxy/bin/mysql-proxyd test start

```

不過應該有 99% 的人會在這個步驟失敗

  這是另外一種啟動方式，反正只是給大家看會有什麼錯誤而已

你可以在 log 裡面看錯誤訊息

```
vi /usr/local/mysql-proxy/log/test.log

```

**解決方式：**

```
# 安裝套件相關套件
apt-get install libmysqlclient-dev
apt-get install mysql-client
# 下載缺失的 libmysqlclient.so.16 套件，我是在這個網站找到的
wget http://files.directadmin.com/services/debian_5.0_64/libmysqlclient.so.16
# 將 libmysqlclient.so.16 搬移到 lib 目錄下
mv libmysqlclient.so.16 /usr/lib/x86_64-linux-gnu/

```

再次啟動 Atlas

```
/usr/local/mysql-proxy/bin/mysql-proxyd test start

```

看到 **OK: MySQL-Proxy of test is started** 的提示就成功嚕！

檢查 Atlas 服務狀態

```
/usr/local/mysql-proxy/bin/mysql-proxyd test status

```

## 五、連到 Atlas 管理 server

Atlas 預設的管理 port 為 2345，ip 填寫 Proxy server ip，如果 admin 的帳號密碼沒換就是跟下面一樣

```
mysql -h172.16.9.39 -P2345 -uuser -ppwd

```

### 查看 Atlas 的管理命令

```
mysql> select * from help;

```

### 查看 MySQL 讀寫分離設定是否生效

rw 為讀寫權限，ro 代表只有讀的權限

```
mysql> select * from backends;

```

到這步其實就可以完結撒花惹 😊😊😊😊😊😊

## 六、實際測試透過 Atlas Proxy 是否達成讀寫分離

Atlas 預設的proxy port 為 1234，ip 填寫 Proxy server ip，登入的帳號密碼填寫你在 Master 申請給 Atlas Proxy 使用的那組

```
mysql -h172.16.9.39 -P1234 -uproxy_account -pproxy_password

```

### 測試寫入功能

```
# 先選擇之前創建的測試資料庫
mysql> use proxy_test;
# 創建表並塞入一寫資料
mysql> CREATE TABLE department (id INT,name varchar(50));
mysql> INSERT INTO department VALUES (1, 'Sales');
mysql> INSERT INTO department VALUES (2, 'Operator');

```

### 測試讀取功能

```
# 先選擇之前創建的測試資料庫
mysql> use proxy_test;
# 讀取剛剛創建的 table
mysql> SELECT * FROM department;

```

### 觀察 sql 運行的 log

> 記得把 Atla 設定檔的 ssql-log 參數調整為 REALTIME 才會有喔！

前往 log file

```
tail /usr/local/mysql-proxy/log/sql_test.log

```

從下圖我們可以觀察到，負責變更資料庫的是 Master server(172.16.3.32)，讀取的則是 Slave Server(172.16.3.33)，恭喜自己大功告成

希望這篇文章有幫助到使用 Ubuntu 架設 MySQL 並且想做到讀寫分離的開發人員，網路上對這塊的資訊相對來說比較零散，有些甚至都是錯誤，整理起來這些文件也真的很費力，如果對你有幫助的話可以幫我按個拍手👏給我一點繼續分享技術文章的動力。

```
如果你想了解資料庫主從式架構的設定歡迎參考我的文章：
👍 MySQL Master Slave Replication 主從式架構設定教學
👍 在 MySQL 5.7 解決主從複製的延遲問題

```

**參考資訊**：[下載libmysqlclient.so.16檔案](https://web.archive.org/web/20170127083654/http://files.directadmin.com:80/services/debian_5.0_64/libmysqlclient.so.16)、[讀寫分離測試](https://www.jianshu.com/p/67d4b34b2af4)、[Mysql访问中间件 — Atlas初探](https://zhuanlan.zhihu.com/p/94279867)、[Atlas](https://www.jianshu.com/p/b68e429d09c7)、[Atlas Github](https://github.com/Qihoo360/Atlas/blob/master/README_ZH.md)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202103/mysql-read-write-splitting-atlas/
