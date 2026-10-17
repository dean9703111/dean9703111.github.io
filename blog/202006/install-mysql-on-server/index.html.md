# 0難度讓你在Server安裝設定MySQL

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202006/install-mysql-on-server/
- 發布：2020-06-16（更新：2023-05-05）
- 主題：技術教學
- 標籤：Database、MySQL、Passwords、Schema、Privilege
- 摘要：這是一篇只要複製貼上，注意小細節就能讓你輕鬆在ubuntu server設定好MySQL的文章

海豚好可愛

## 文章架構如下：

- 安裝MySQL
- 設定MySQL可以讓外網連線
- MySQL防火牆設定
- 建立新DB
- 新增/刪除使用者及權限
- 列出資料庫有權限的使用者列表
- 列出所有User權限表
- 修改MySQL root密碼
- 修改整體密碼規則強度

## 1. 安裝MySQL

在安裝MySQL的時候會問你很多問題，如果你沒有特殊需求就都用y通過，密碼強度部分需要記住自己是怎麼設定的(因為不同密碼強度的規則差異很大，詳情可以看[這篇](http://n.sfs.tw/content/index/13359))

```
sudo apt update
sudo apt install mysql-server
sudo mysql_secure_installation

```

安裝完成後你可以先進入MySQL看看

```
sudo mysql -u root -p

```

## 2. 設定MySQL可以讓外網連線

如果你所架設的MySQL是允許外網或者某個特定ip做連線的，你就做以下調整

```
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
bind-address = 0.0.0.0 //這個是指誰都可以連線的意思
sudo su
service mysql restart //設定完成後要重啟才會生效

```

## 3. MySQL防火牆設定

以下為最基礎的防火牆設定，資料庫慣用的port為3306，22是給ssh登入使用的

```
sudo apt-get install ufw
sudo ufw enable
sudo ufw allow 3306
sudo ufw allow 22/tcp
sudo ufw status

```

## 4. 建立新DB

```
sudo mysql -u root -p

```

進入MySQL後你可以用指令的方式建立DB如下，如果你需要修改他默認編碼也能夠透過ALTER去做修正

```
CREATE DATABASE db_name;
ALTER DATABASE xxx_db CHARACTER SET utf8 COLLATE utf8_bin;

```

## 5. 新增/刪除使用者及權限

新增mysql使用者及賦予權限

```
use mysql; //進入這個資料庫
create user ‘某某人’@‘哪個ip可以(%為全部)’ IDENTIFIED WITH mysql_native_password BY ‘密碼’;
CREATE DATABASE `資料庫名稱`;
grant all privileges on 資料庫名稱(如果沒填就是全部).* to ‘某某人’@‘哪個ip可以’ identified by ‘密碼’;

```

刪除使用者及其權限

```
REVOKE ALL PRIVILEGES, GRANT OPTION FROM ‘某某人’@‘哪個ip可以(%為全部)’;
DROP USER ‘某某人’@‘哪個ip可以(%為全部)’;
flush privileges; //刷新權限
select * from information_schema.user_privileges;//確認該User權限已被刪除

```

## 6. 列出資料庫有權限的使用者列表

```
select user from mysql.db where db=’資料庫名稱’;

```

## 7. 列出所有User權限表

```
select * from information_schema.user_privileges;

```

## 8. 修改mysql root密碼

請切記要修改root的密碼，避免駭客侵入你的伺服器後可以無所忌憚的讀取你的資料

```
use mysql;
// 先指定 plugin 為 mysql_native_password
UPDATE user SET plugin=’mysql_native_password’ WHERE User=’root’;
UPDATE mysql.user SET authentication_string=PASSWORD(‘password’) WHERE USER=’root’;
FLUSH PRIVILEGES;

```

## 9. 修改整體密碼規則強度

如果你在安裝時對於密碼強度的問題是隨便選的，你可以在登入MySQL後用這個指令做修正

```
set global validate_password_policy=0;

```

密碼強度檢查等級，0/LOW、1/MEDIUM、2/STRONG

0 or LOW Length
1 or MEDIUM Length; numeric, lowercase/uppercase, and special characters
2 or STRONG Length; numeric, lowercase/uppercase, and special characters;

預設是1，即MEDIUM，所以剛開始設定的密碼必須符合長度，且必須含有數字，小寫或大寫字母，特殊字元。

修改密碼長度限制

```
set global validate_password_length=6;

```

千萬不要為了圖方便而設計複雜度太低的密碼

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202006/install-mysql-on-server/
