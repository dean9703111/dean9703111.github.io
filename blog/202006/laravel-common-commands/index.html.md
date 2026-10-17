# Laravel常用指令

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202006/laravel-common-commands/
- 發布：2020-06-16
- 主題：技術教學
- 標籤：Laravel、PHP、Artisan、Commands、Ini
- 摘要：laravel有很多常用的指令，但有時候會忘記或是不想自己全部打完，那這篇文章就是你專門用來複製貼上的大補帖

> 這篇文章會列出我在使用Laraval框架時常用到的指令(部分是套件指令)

## 1. 資料庫相關指令

**Migration 資料庫完全重新設定**

```
php artisan migrate:refresh — seed

```

**備份應用程式包括資料庫，[請確認是否已經安裝plugin](https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/190804/)**

```
php artisan backup:run

```

**備份的資料庫執行**

```
php artisan backup:run --only-db

```

**只需要備份的檔案**

```
php artisan backup:run --only-files

```

**database 還原**

```
mysql -u root -p database_name < backup.sql

```

## 2. 安裝檔遺失相關issue

**composer 全部砍掉重裝**

```
rm -rf vendor/
composer update -v

```

## 3. Laravel 常用指令

**創建model**

```
php artisan make:model Photo

```

**創建controller**

```
php artisan make:controller UserController

```

**創建migration**

```
php artisan make:migration create_users_table — table=users

```

**創建seeder**

```
php artisan make:seeder UsersTableSeeder

```

**創建middleware**

```
php artisan make:middleware CheckAge

```

**創建job**

```
php artisan make:job SendReminderEmail

```

**創建testcase**

```
php artisan make:test UserTest

```

## 4. php相關指令

**尋找php.ini**

```
sudo find / -name ‘php.ini’

```

**更改php相關設定(如果遇到nginx 502也能用這個重啟嘗試看看)**

```
vi /etc/php/7.3/fpm/php.ini
service php7.3-fpm restart

```

## 5. 單元測試

```
./vendor/bin/phpunit
// 如果使用sudo權限來做測試，部分storage會變成admin權限，這個指令可以讓整個資料夾回歸www-data權限
chown -R www-data:www-data storage/

```

## 6. 排程相關

**排程重啟supervisor**

```
// 檔案位置
/etc/supervisor/conf.d
// 每次修改需要執行這幾個指令重啟(開啟)
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl start laravel-worker:*
sudo supervisorctl restart laravel-worker:*

```

**本機local端測試排程job**

下面幾種job為我個人將每個排程進行分類，讓需要較長時間的排程不會影響到其他短時間能完成的排程

```
php artisan queue:work — queue=small_job,default — timeout=600 — delay=3 — sleep=3 — tries=3
php artisan queue:work — queue=big_job,default — timeout=7200 — delay=3 — sleep=3 — tries=3
php artisan queue:work — queue=default — delay=3 — sleep=3 — tries=3

```

## 7. 查看錯誤訊息

如果為 php 的錯誤訊息

```
vi /var/log/php7.3-fpm.log

```

如果為 Laravel 的錯誤訊息

```
vi project_path/storage/logs/laravel-xxx-xx.log

```

> 從 log 就能推斷出大部分的問題，並取得查詢的關鍵字

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202006/laravel-common-commands/
