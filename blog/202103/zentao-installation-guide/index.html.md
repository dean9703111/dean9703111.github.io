# 禪道 Zentao 安裝教學 & 重要系統設定，建議公司導入的專案管理系統

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202103/zentao-installation-guide/
- 發布：2021-03-10（更新：2023-05-05）
- 主題：團隊合作
- 標籤：ZenTao、禪道、管理系統、安裝教學、系統設定
- 摘要：過去安裝一些內部使用的 Server ，像 Gitlab、Redmine 都讓人在安裝的過程中遇到許多的挫折；直到最近同事向我推薦一款專案管理系統 — 禪道，聽到這詭異的名字讓我對他深表懷疑，抱著嘗試的心態安裝與試用了一下，讓我頓時非常後悔當初為什麼要研究…

過去安裝一些內部使用的 Server ，像 Gitlab、Redmine 都讓人在安裝的過程中遇到許多的挫折；直到最近同事向我推薦一款專案管理系統 — **禪道(Zentao)**，聽到這詭異的名字讓我對他深表懷疑，抱著嘗試的心態安裝與試用了一下，讓我頓時非常後悔當初為什麼要研究 Redmine😭😭😭，這套專案管理系統的安裝過程讓我理解了何謂『選擇比努力更重要😳』

```
一、Server 安裝環境
二、安裝步驟
STEP 1：下載官網壓縮檔
STEP 2：解壓縮安裝檔並啟用系統
STEP 3：輸入網址開始暢遊～
三、自選設定
➤ HTTPS 設定
➤ 系統 email 通知
➤ 修改預設入口頁面路徑
➤ 移除url中的『zentao』路徑

```

## 一、Server 安裝環境

- Processor：使用 VMWARE 的虛擬單核心
- RAM：2G
- ROM：300G
- OS：ubuntu 20.04
- Zentao：12.5.3

## 二、安裝步驟

建議使用英文版本的官網，中文版本的官網瀏覽速度極慢

### STEP 1：下載官網壓縮檔

- 請在 [**官網**](https://www.zentao.pm/download.html) 找最新版的開源版本下載到 server

- 官網有提供不同環境的安裝檔，請依據自己的環境選擇

- 複製下載連結，下載安裝檔到 server 上

```
sudo wget https://dl.cnezsoft.com/zentao/12.5.3/ZenTaoALM.12.5.3.int.zbox_64.tar.gz

```

### STEP 2：解壓縮安裝檔並啟用系統

- 將安裝檔解壓縮 opt 資料夾

```
sudo tar -zxvf ZenTaoALM.12.5.3.int.zbox_64.tar.gz -C /opt

```

- 啟動 Apache 和 MySQL

```
sudo  /opt/zbox/zbox start

```

### STEP 3：輸入網址開始暢遊～

- 在網址輸入你 server 的網址就能夠使用嚕！

- 第一次登入請使用默認的帳號(admin)、密碼(123456)做登入，確認操作是否都正常

- 登入成功後會先請你修改帳號密碼

- 大功告成！接下來就是開心的使用嚕！

## 三、自選設定

稍微使用一下後覺得有些設定還是說明一下

### ➤ HTTPS 設定

- 先進入 apache 的設定檔

```
vi /opt/zbox/etc/apache/httpd.conf

```

- 在裡面貼上如下資訊

記得 ServerName、SSLCertificateFile、SSLCertificateKeyFile 要自己調整喔！

```
<VirtualHost *:443>
   ServerAdmin zentao@local.net
   DocumentRoot "/opt/zbox/app/zentaoep/www/"
   ServerName your_ip_or_domain_name
#  ServerAlias  *.xxx.com

   SSLEngine On
   SSLCertificateFile /opt/zbox/etc/apache/ssl/your.crt
   SSLCertificateKeyFile /opt/zbox/etc/apache/ssl/your.key
   <Directory "/opt/zbox/app/zentaoep/www">
     AllowOverride all
     Require all granted
   </Directory>
   ErrorLog "/opt/zbox/logs/apache_error_log"
   CustomLog "/opt/zbox/logs/apache_access_log" combind
 </VirtualHost>

```

原始碼：[httpd.conf](https://gist.github.com/dean9703111/5eaf1f305175df1a41f4e6941069bc1b)（GitHub Gist）

- 重啟 zentao ~

```
sudo /opt/zbox/zbox restart

```

  順利的話就可以看到**左上角的鎖頭**，成功建立安全連線嚕～

### ➤ 系統 email 通知

以下為 Gmail 信箱的設置

- **發送信箱**：你的 Gmail
- **發信人**：你想要的寄件者名字
- **smtp 帳號**：你的 Gmail，但後面去掉@gmail.com
- **smtp 密碼**：你的 Gmail 密碼

  建立後可以用**測試發信**喔！

### ➤ 修改預設入口頁面路徑

系統預設的路徑會讓首頁導向到下面這個畫面：

其中『ZenTao Pro』、『ZenTao Ent』對使用開源版本的我們來說其實是多餘的，我們可以藉由以下設定改變預設路徑

**STEP 1**：前往 Zentao 安裝的目錄下，將『zentaoep』、『zentaopro』的資料夾註解或是移除，完成這個步驟後回到網頁點擊『ZenTao Pro』、『ZenTao Ent』會發現網頁回報 404 錯誤。

  可以用 mv(重新命名) 或是 rm(移除) 的指令

**STEP 2**：調整 apache 的 config 檔案

```
vi /opt/zbox/etc/apache/httpd.conf

```

將 `DocumentRoot “/opt/zbox/app/htdocs”` 註解改成 zentao 的路徑 `DocumentRoot “/opt/zbox/app/zentao/www/"`

**STEP 3**：重新啟動 Apache 和 MySQL

```
sudo /opt/zbox/zbox restart

```

**STEP 4**：重新輸入網址就會發現入口頁面變成禪道的登入頁面嚕～

### ➤ 移除url中的『zentao』路徑

我們明明已經在zentao底下了，還有一個 `zentao` 的路徑真的多此一舉，移除他其實很簡單（官方給的教學基本上無效…搞了我好久）

**STEP 1**：打開 zentao 的設定檔

```
vi /opt/zbox/app/zentao/config/my.php

```

**STEP 2**：調整網路的路徑，將原本 `$config->webRoot` 的那行註解，貼上 `$config->webRoot = ‘/’;` 後儲存離開

**STEP 3**：重新登入 Zentao 的管理系統後會發現多餘的路徑通通不見嚕～

  『zentao』真的消失惹呢！

### ➤ 備份方式

- 備份完整資料夾

```
sudo /opt/zbox/zbox stop    #停止zbox服务
sudo cp -prf /opt/zbox /opt/zbox_bak    #備份整个zbox目錄

```

2. 其實系統內建凌晨 00:30 分會備份，路徑在

```
/opt/zbox/app/zentao/tmp/backup/

```

相比之前安裝遇到各種挫折的 Redmine、Gitlab，Zentao 真的是完勝，目前榮登我這輩子安裝最順暢的管理系統！

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202103/zentao-installation-guide/
