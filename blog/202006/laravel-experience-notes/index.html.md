# Laravel經驗

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202006/laravel-experience-notes/
- 發布：2020-06-17
- 主題：技術教學
- 標籤：Laravel、PHP Developers、Laravel Framework、Laravel Issue、Laravel Expert
- 摘要：希望這篇文章可以幫助你快速通過我在開發Laravel時所遇到的各種坑(如果有遇到新的我會持續更新)

### 為什麼總是要 composer dump-autoload ?

剛開始接觸用 composer 的時候一直被這個問題蠱惑。很不理解為什麼總是要打這句命令才能不報錯，現在終於知道根結了。

因為 database 文件夾使用 classmap 來做加載的。所以只有在打了 composer dumpautoload 之後 composer 才會更新 autoload_classmap 的內容。

### 遇到無法composer可用這個

```
sudo apt-get install php-mbstring
sudo apt-get install php-xml

```

### 如果遇到log讀寫權限不足

```
vim /etc/nginx/nginx.conf

```

去看nginx的權限user www-data;

Laravel的資料夾權限需要跟nginx一樣

```
chown -R www-data.www-data device-api
sudo chgrp -R www-data storage bootstrap/cache
sudo chmod -R ug+rwx storage bootstrap/cache

```

### php-fpm錯誤: WARNING: [pool www] server reached pm.max_children setting (5), consider raising it

[https://blog.csdn.net/encircles/article/details/80503756](https://blog.csdn.net/encircles/article/details/80503756)

### timeout相關(包含nginx, php-fpm)

[https://blog.goodjack.tw/2018/08/increase-php-fpm-script-execution-time-with-nginx.html](https://blog.goodjack.tw/2018/08/increase-php-fpm-script-execution-time-with-nginx.html)

### composer 版本太舊無法更新問題

https://lequ7.com/2019/06/19/richang/composer-fu-wu-qi-an-zhuang-kuo-zhan-shi-bai-ban-ben-guo-di-de-sheng-ji-guo-cheng/

### nginx 出现504 Gateway Time-out的解决方法

```
sudo vi /etc/nginx/nginx.conf

```

系統有超過 300~500mb的影片，这了，超時時[間調整為30分鐘；這裡有更詳細的說明](https://blog.csdn.net/fdipzone/article/details/79355576)

```
proxy_connect_timeout 1800s;#nginx跟後端服務器連接超時時間(代理連接超時)
proxy_send_timeout 1800s;#後端服務器數據回傳時間(代理發送超時)
proxy_read_timeout 1800s;#連接成功後，後端服務器響應時間(代理接收超時)
fastcgi_connect_timeout 1800s;#指定nginx與後端fastcgi server連接超時時間
fastcgi_send_timeout 1800s;#指定nginx向後端傳送請求超時時間（指已完成兩次握手後向fastcgi傳送請求超時時間）
fastcgi_read_timeout 1800s;#指定nginx向後端傳送響應超時時間（指已完成兩次握手後向fastcgi傳送請求超時時間）

```

### Laravel 同時間大量update會有數據對不上的問題(transaction issue)

[https://blog.scottchayaa.com/post/2019/01/09/how-to-handle-the-high-concurrency-on-laravel/](https://blog.scottchayaa.com/post/2019/01/09/how-to-handle-the-high-concurrency-on-laravel/)

### Php的mysql沒有安裝完整的錯誤

PHP Warning: PHP Startup: Unable to load dynamic library ‘pdo_mysql’ (tried: /usr/lib/php/20170718/pdo_mysql (/usr/lib/php/20170718/pdo_mysql: cannot open shared object file: No such file or directory), /usr/lib/php/20170718/pdo_mysql.so (/usr/lib/php/20170718/pdo_mysql.so: undefined symbol: pdo_parse_params)) in Unknown on line 0

```
sudo apt-get install php-common php-mysql php-cli

```

### 如果出現php版本跑掉的狀況，不要擔心

```
sudo update-alternatives — config php

```

### 確認現在php支援databsae的相容性

php artisan tinker

DB::availableDrivers()

### 只想要讓某個 function 可以執行長時間的任務（ex：上傳檔案）

在 function 裡面加上 `ini_set(‘mac_excution_time’,‘600’)` ，其中「600」的單位是秒數。

```
public function long_excution_function(Request $request) {
  ini_set('max_execution_time', '600');
  // your code
}

```

原始碼：[long_excution_function.php](https://gist.github.com/dean9703111/966cd5c90443b8d5a2308df9968d5cf8)（GitHub Gist）

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202006/laravel-experience-notes/
