# Nginx高頻率使用功能

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202006/nginx-common-features/
- 發布：2020-06-22
- 主題：技術教學
- 標籤：Nginx、Nginx Proxy、Proxy、Load Balancing、Load Balancer
- 摘要：我將在這裡記錄Nginx時常被問到但很難說清楚的問題及實作方式，文章參考許多前輩們的文獻，在此先感謝他們的無私貢獻

### 相關議題

1. 反向代理伺服器(reverse proxy)
2. 負載平衡(load balancer)
3. 虛擬主機(Virtual Host)

## 1. 反向代理伺服器(reverse proxy)

> 正向代理伺服器：用來解決訪問限制問題
> 反向代理伺服器：提供負載均衡、安全防護等作用

你可以理解為你網頁的前端實際使用server的port是3000，但是透過Nginx可以把對方80 port的連線導向3000 port，應用如下：

```
vi /etc/nginx/sites-enabled/default

```

```
server {
    listen 80;   //外部nginx監聽port
    location / {
        proxy_pass http://127.0.0.1:3000; #你可以把它當成local端轉port的服務
        # proxy_pass http://172.1.1.1; #這是實際訪問的ip位址
    }
}

```

原始碼：[reverse_proxy.conf](https://gist.github.com/dean9703111/169bc78205976713c685ef3e1611f069)（GitHub Gist）

實務上野可能會直接把它當成一個自動轉址的中介Service，像是外面所輸入的domain name實際上都是指向同一台nginx server，經過這台nginx 的設定分析過後再導向真正的server

```
server {
    listen         *:443 ssl; #https
    server_name   your.domain.name1; #外面輸入的網址

    client_max_body_size 2000M; #上傳檔案大小限制
    fastcgi_read_timeout 3600s; # nginx執行時間
    proxy_read_timeout 3600s;
    ssl_certificate /var/www/cert.chain.crt; # ssl檔案位置
    ssl_certificate_key /var/www/www_ruggon_com.key;
    location / {
        proxy_pass https:#172.6.5.8; # 目標導向的服務位址
    }
}
server {
    listen         *:443 ssl;
    server_name   your.domain.name2; #外面輸入的網址
    ssl_certificate /var/www/cert.chain.crt;
    ssl_certificate_key /var/www/www_ruggon_com.key;
    location / {
        proxy_pass https:#172.6.5.10; # 目標導向的服務位址
    }
}
#下面為讀取80 port http轉https的功能
server {
        listen 80;
        listen [::]:80;
        server_name your.domain.name1;
        return 301 https:#$host$request_uri;
}
server {
        listen 80;
        listen [::]:80;
        server_name your.domain.name;
        return 301 https:#$host$request_uri;
}

```

原始碼：[redirect.conf](https://gist.github.com/dean9703111/5956b633bb1bf9af45f93e416f6e9a41)（GitHub Gist）

## 2. 負載平衡(load balancer)

如果一個網站的訪問量非常大，一台server已經無法負擔時，你可以透過load balance(多幾台server來分攤這些流量，當某一台server掛掉的時候你的網站還是活得很好)處理

Nginx實務上你可以根據server實際上的硬體條件去分散每個server的流量，在這裡我以透過Nginx分流到兩個server上面做解釋：

SETP 1/3 : server A的Nginx設定

```
vi /etc/nginx/sites-enabled/default

```

```
server {
    listen 80;

    #Proxy的位置，提供給使用者連線
    server_name  server_a.com;

    access_log /var/log/nginx/server_a.com main;
    error_log /var/log/nginx/server_a.com warn;

    location / {
        root   /usr/share/nginx/html;# 靜態文件路徑
        index  index.html index.htm;
    }
}

```

原始碼：[server_a.conf](https://gist.github.com/dean9703111/9f199fd23e615fdcf568702f86698cfd)（GitHub Gist）

SETP 2/3 : server B的Nginx設定

```
vi /etc/nginx/sites-enabled/default

```

```
server {
    listen 80;

    #Proxy的位置，提供給使用者連線
    server_name  server_b.com;

    access_log /var/log/nginx/server_b.com main;
    error_log /var/log/nginx/server_b.com warn;

    location / {
        root   /usr/share/nginx/html; #靜態文件路徑
        index  index.html index.htm;
    }
}

```

原始碼：[server_b.conf](https://gist.github.com/dean9703111/eb788190ed0de8adff45c3da1f64bf73)（GitHub Gist）

STEP 3/3 : nginx來處理負載平衡

```
vi /etc/nginx/sites-enabled/default

```

```
# weight是資源分散的權重，可根據server本身硬體條件做調整
upstream your_web {
    server server_a.com weight=3;#前面設定的server_name，或者用存粹的ip也可以()
    server server_a.com weight=2;
}
server {
    listen       443 ssl;
    server_name  server.com;
    access_log /var/log/nginx/server.com.access.log main;
    error_log /var/log/nginx/server.com.error.log warn;
    ssl_certificate server.com.crt;
    ssl_certificate_key server.com..key;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    location / {
        proxy_pass http://your_web;#真實網址
        proxy_set_header   Host    $host;
        proxy_set_header   X-Real-IP   $remote_addr;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}

```

原始碼：[nginx.conf](https://gist.github.com/dean9703111/c59dd2d76bd61d9f7e74749aa9959b57)（GitHub Gist）

## 3. 虛擬主機(Virtual Host)

如果單一網站的流量太小，你會覺得他獨佔一台server有點浪費，所以除了負載平衡(load balance)這種導流量的做法外，你也可以讓一台server在不同的port開啟多個服務

```

server {  
    listen 80 default_server;  
    server_name _;  
    return 444; # 過濾其他domain name返回444
}

server {  
    listen 80;  
    server_name your.domain.name1;

    location / {  
        proxy_pass http://localhost:8080; # 內部對應的port
    }
}

server {  
    listen 80;  
    server_name your.domain.name2;
    location / {  
        proxy_pass http://localhost:8081; # 內部對應的port
    }  
}

```

原始碼：[VirtualHost.conf](https://gist.github.com/dean9703111/25717edfe9b85681caddbb0827f8b389)（GitHub Gist）

參考資源：[使用 Nginx 做 Load Balancer](https://blog.dtask.idv.tw/Nginx/2018-07-31/)、[Nginx簡介](https://blog.csdn.net/qq_25843323/article/details/81902015)、[正向代理和反向代理解釋](https://kknews.cc/zh-tw/tech/k66p2gb.html)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202006/nginx-common-features/
