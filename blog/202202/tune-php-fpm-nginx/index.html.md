# 調整 php-fpm & nginx 參數，讓 Server 可以承受更大的流量

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202202/tune-php-fpm-nginx/
- 發布：2022-02-25
- 主題：技術教學
- 標籤：PHP-FPM、Nginx、Concurrency、大流量、高併發
- 摘要：因為準備上線的系統有可能遇到大流量的情境，所以上線前先用 Apache Bench 來做測試；不測還好，一測發現在連線數 100、request 數量 10000 的條件下 Call POST API，居然有 50% 的 request…

因為準備上線的系統有可能遇到大流量的情境，所以上線前先用 Apache Bench 來做測試；不測還好，一測發現在連線數 100、request 數量 10000 的條件下 Call POST API，居然有 50% 的 request fail，如果把這樣的系統上線肯定會出包，所以來研究如何調校 php-fpm & nginx 的參數，讓 Server 在硬體不升級的狀態下盡可能承受最大的流量。

> 如果對壓力測試不熟悉的朋友，可以參考筆者先前分享的文章：「[Apache Bench — 壓力測試好夥伴](https://deanlin.net/blog/202110/apache-bench-load-testing/)」

```
大綱
一、了解目前 Server 資源利用率
二、調整 php-fpm 參數
三、調整 nginx 參數增加併發數量

```

## 一、了解目前 Server 資源利用率

進入 Server 後，你可以用 `top` 這個指令來觀察 Server 的各項參數；它所呈現參數是動態的，非常易於理解與判斷。

接下來你可以執行一段 Apache Bench 的指令（建議是 POST），來測試看看他怎麼爆掉的：

```
ab -n 10000 -c 100 -p post.txt -T application/json -H ‘Authorization: Token your_token’ https://your_test_ip/

```

上面的指令執行後，可以得到類似下面的報表：

在上面報表中可以看到居然有 50% 的 request fail，可是用 `top`觀察 Server 的資源時，發現無論是 CPU 、記憶體距離極限還有一段距離，且 idle 都還有剩：

所以下面我教大家調整哪些 php-fpm & nginx 的設定，盡量讓這台 Server 發揮出他的極限。

> **備註 1**：通常 POST 對資源的消耗遠超 GET，如果 API 會修改資料庫內容、進行複雜計算就會消耗更多資源，這些都是要考量進去的。
> **備註 2**：我們的目標是**追求承受最大流量，而不是努力把 CPU 和 MEM 耗盡**；經實驗，有時把他們耗盡反而會有更多的 request fail，這部分有興趣的讀者可以自己實驗看看。

## 二、調整 php-fpm 參數

### STEP 1：先進入 php-fpm 的 conf 設定檔

```
sudo vi /etc/php/7.4/fpm/pool.d/www.conf

```

然後認識以下參數各自有什麼作用：

- **pm**：預設為「dynamic」，如果設定為「static」下面的參數只有「pm.max_children」會生效。
- **pm.max_children**：設置子進程最大數值，子進程數量太少會影響處理連線的速度，太多可能會導致資源耗盡。
- **pm.start_servers**：php-fpm 啟動起始進程數，數值需小於 pm.max_children。

> 建議為：(pm.max_spare_servers — pm.min_spare_servers)/2

- **pm.min_spare_servers**：php-fpm 的最小空閒進程數。
- **pm.max_spare_servers**：php-fpm 的最大空閒進程數。
- **pm.max_requests**：當子進程的 request 數量到達後關閉，再開一個新的。

下面提供的參數**僅為範例**，請依照 Server 實際狀況調整，並且用 `top` 這個指令觀察資源消耗狀態（主要看 CPU、MEM 使用率）。

```
pm = dynamic
pm.max_children = 200
pm.start_servers = 150
pm.min_spare_servers = 100
pm.max_spare_servers = 200
pm.max_requests = 1000

```

### STEP 2：完成後後重啟 php fpm

執行下面指令才會生效喔～

```
service php7.4-fpm restart

```

如果剛剛的參數設定上有問題或是錯誤，上面會有錯誤提示，跟著提示操作就好了：

## 三、調整 nginx 參數增加併發數量

調整完 php-fpm 的參數後，Server 相較之前可以承受更大的流量；但如隨著系統使用者的成長，你很快會遇到下一個瓶頸，在決定花錢升級 Server 硬體水平前，可以先嘗試調整 nginx 的參數，看看是否能在支撐一段時間ＸＤ

### STEP 1：確認 Server 有多少個 CPU 核心

下面這段指令可以知道這台 Server 有多少個 CPU 核心（worker_processes）：

```
grep processor /proc/cpuinfo | wc -l

```

### STEP 2：進入 nginx 的 conf 設定檔

接著進入**nginx.conf**，查看目前 nginx 默認使用多少 CPU 核心數。

```
vi /etc/nginx/nginx.conf

```

你可以將 **worker_processes** 調整成 CPU 核心數，或是用 auto 讓他自動偵測：

```
worker_processes auto;

```

如果系統會遇到高併發場景，可以設置 **events** 的參數如下：

- **worker_connections**：設定 worker 最大可承受連線數（最大值為 65535）。

> 連線的總數為 worker_processes * worker_connections。

- **multi_accept**：收到一個 request 後，盡可能多接收其他的 request。
- **use epoll**：利用很棒的演算法提升效能。

```
events {
    worker_connections 4096;
    multi_accept on;
    use epoll;
}

```

### STEP 3：設定完記得要重啟 nginx

```
nginx -s reload

```

> 希望這篇文章能讓讀者初步了解在 php-fpm & nginx 的設定中，哪些參數可以讓 Server 承受更大的流量，如果讀者有使用其他妙招也歡迎分享喔～

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202202/tune-php-fpm-nginx/
