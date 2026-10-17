# Nginx — 你網頁的好夥伴

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202006/nginx-introduction/
- 發布：2020-06-10（更新：2023-05-05）
- 主題：技術教學
- 標籤：Nginx
- 摘要：這個好用的網頁伺服器工具最近兩年被我大量使用，因為他的設定簡單，功能強大，在這裡我會把分幾個部分做說明：

這個好用的網頁伺服器工具最近兩年被我大量使用，因為他的設定簡單，功能強大，在這裡我會把分幾個部分做說明：

- **安裝(ubuntu環境)**
- **防火牆設定**
- **常用操作**
- **曾經遇到的坑**
- **相關參考資源**

## 安裝(ubuntu環境)

```
sudo apt-get update
sudo apt install nginx
sudo systemctl start nginx
sudo systemctl enable nginx

```

## 防火牆設定

```
sudo ufw allow ssh  // 允許ssh登入作業
sudo ufw allow 80  // 允許http
sudo ufw allow 443  // 允許https
sudo ufw allow ‘Nginx HTTP’
sudo ufw enable // 啟動防火牆
sudo ufw status // 列出當前防火牆狀態
sudo ufw status numbered  // 列出當前防火牆規則號碼(通常都是為了刪除)
sudo ufw delete 2 // 刪除防火牆特定規則

```

## 常用操作

```
sudo systemctl restart nginx  // 重啟
sudo /etc/init.d/apache2 stop  // 暫停apache
service php7.3-fpm restart // php的conf修改後需要重啟

```

## 曾經遇到的坑

413 Request Entity Too Large

如果有用nginx當loadbalance，記得/etc/nginx/sites-enabled/default的部分也要做相對應的調整

```
client_max_body_size 2000M;
sudo service nginx restart

```

Git clone through Nginx proxy for Gitlab server is not working

[可參考stackoverflow](https://stackoverflow.com/questions/38920146/git-clone-through-nginx-proxy-for-gitlab-server-is-not-working)

ssh_exchange_identification: Connection closed by remote host

因為/etc/nginx/sites-enabled/default的地方修改無效

所以要在上一層/etc/nginx/nginx.conf做修正

```
stream {
  server {
    listen 223;
    listen [::]:223;
    proxy_pass 192.xxx.xxx.xxx:22; //目標ip:ssh port
  }
}
sudo service nginx restart

```

server關機重啟後頁面 404

因為安裝php的時候apache有時沒注意會被順便安裝起來

首先檢查是否被apache佔用

```
sudo netstat -lpn |grep 80

```

如果是的話可以把apache暫停或是刪除

```
sudo /etc/init.d/apache2 stop  // 暫停apache
// 下面是刪除的方法
sudo apt-get purge apache2 apache2-utils apache2.2-bin apache2-common
sudo apt-get autoremove
whereis apache2
apache2: /etc/apache2 //列出路徑
sudo rm -rf /etc/apache2

```

nginx 502

查看錯誤日誌

```
sudo vi /var/log/nginx/error.log

```

connect() to unix:/var/run/php-fpm.sock failed (11: Resource temporarily unavailable)

如果是以上錯誤

```
sudo vi /etc/php/7.3/fpm/pool.d/www.conf

```

将backlog前面的;去掉，并将其值改为4096.

保存重启fpm

```
service php7.3-fpm restart

```

Nginx使用limit_req_zone對同一ip訪問進行限流

考慮到有可能的DDOS問題，又或是一些不正常的連線次數(在短時間大量call很多次api)，可參考這兩篇文章修改nginx默認的連線設定:[文章1](https://bbs.csdn.net/topics/390871977)、[文章2](https://blog.csdn.net/jek123456/article/details/53334095)

想調整可以先進入設定檔：

```
vi /etc/nginx/sites-available/default

```

針對你想要加上限制的部分加上設定：

```
limit_req_zone $binary_remote_addr zone=reqlimit:10m rate=5r/s;
server {
  listen 80 default_server;
  limit_req zone=reqlimit;
}

```

- **zone=reqlimit**：自己定義的名稱
- **10m**：Server 記憶體要保存多少 ip 的 request 紀錄
- **rate=5r/s**：每秒接受 5 個 requests

設定 no-cache 機制

有時網頁會 cache 到過去的資源，此時你可以在目標路徑下做如下設定

```
location / {
 add_header Cache-Control no-cache;
 add_header Cache-Control private;
 try_files $uri $uri/ /index.html;
}

```

## 相關參考資源

[Nginx SSL 憑證加分的設定調整](https://blog.longwin.com.tw/2015/12/nginx-ssl-https-ca-setup-rating-a-2015/)

[Nginx 設定 SSL 網頁加密](https://www.opencli.com/linux/nginx-setup-ssl)

[ssl key比對](https://www.sslshopper.com/certificate-key-matcher.html)

[ssl驗證網址](https://www.sslshopper.com/ssl-checker.html#hostname=https://qa.device_api.ubiqconn.com/)

[nginx 轉址(loadbalance)](https://www.digitalocean.com/community/questions/nginx-ssl-multiple-domains)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202006/nginx-introduction/
