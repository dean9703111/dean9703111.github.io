# 帶你認識＆安裝「File Browser」這個超棒的雲端文件管理工具

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202112/file-browser-cloud-file-manager/
- 發布：2021-12-23（更新：2023-05-05）
- 主題：技術教學
- 標籤：File Browser、遠端文件管理、雲端文件管理、安裝教學、安裝
- 摘要：因為專案剛好有遠端文件管理的需求，原本是使用 samba 作為解決方案；但在 chrome 瀏覽器要「跳轉顯示檔案總管」時遇到了瓶頸，只能跳出唯獨顯示文檔的頁面。因此轉而去尋找其他合適的工具，最好是操作簡單、一鍵安裝的那種，因而發現這款好用的工具來跟大家分享。

因為專案剛好有遠端文件管理的需求，原本是使用 **samba **作為解決方案；但在 chrome 瀏覽器要「跳轉顯示檔案總管」時遇到了瓶頸，只能跳出唯獨顯示文檔的頁面。因此轉而去尋找其他合適的工具，最好是操作簡單、一鍵安裝的那種，最後發現這款好用的工具來跟大家分享。

  File Browser：[官網](https://filebrowser.org/)

```
一、工具簡介
  ➤ 熟悉的目錄文件結構
  ➤ 簡易文件編輯
  ➤ 分享檔案/資料夾
  ➤ 個人資訊設定
  ➤ 使用者帳號管理
二、安裝＆設定
  ➤ STEP 1：安裝 docker 並建立目錄
  ➤ STEP 2：抓取官方最新 Docker image
  ➤ STEP 3：建立 docker-compose.yml，啟動 File Browser
  ➤ Bonus：增加 docker 安全性、SSL、設定資料儲存目錄
  ➤ 查看 Docker log

```

## 一、工具簡介

基本上筆者就是把它當成一個公司內部存放檔案的管理工具，下面重點介紹我覺得有用的 Feaure。

### 熟悉的目錄文件結構

可以像 Google Drive、Dropbox 一樣拖拉操作，使用直覺；如果要說缺點應該就是不能用滑鼠右鍵來操作新增、刪除這類的操作，但以一款免費工具而言已經非常好用了。

### 簡易文件編輯

如果你新增/上傳的是文件類型（ex：txt/html/md）的檔案，他也可以直接在線上編輯喔！

### 分享檔案/資料夾

其實裡面讓我覺得最有趣的是這個功能，他可以設定分享檔案的「時效、密碼」，當然你也可以都不設定直接分享啦 XD

收到連結的人，打開會長這個畫面

### 個人資訊設定

他支援多國語言體系、使用者可以自行改變帳號密碼的系統喔～

### 使用者帳號管理

如果你的權限是 Administrator，那你就可以新增/刪除帳號，並且設定該帳號的存取權限。

  新增帳號

  帳號列表

其實還有很多功能，但因為大多數使用者都不會用到，這裡便不再贅述，有興趣的朋友可以參考[官網](https://filebrowser.org/)。

## 二、安裝＆設定

下面指令適用於 Linux 的 Ubuntu 系統

### STEP 1：安裝 docker 並建立目錄

```
# 安裝 Docker
apt update && apt install docker docker-compose

# 建立資料夾
mkdir /apps

# 前往資料夾
cd /apps

```

原始碼：[file_browser.sh](https://gist.github.com/dean9703111/dc5d38dce42b62e44f3892ba1a3ac653)（GitHub Gist）

### STEP 2：抓取官方最新 Docker image

```
# 抓取官方最新 docker image
docker pull filebrowser/filebrowser

# 確認抓取的 docker images
docker images

```

原始碼：[file_browser_pull.sh](https://gist.github.com/dean9703111/e2c910b28e824d26e5f94498031f3f73)（GitHub Gist）

### STEP 3：建立 docker-compose.yml，啟動 File Browser

```
# 建立執行的資料夾
mkdir filebrowser

# 建立 docker-compose.yml
vi docker-compose.yml

```

原始碼：[file_browser_folder.sh](https://gist.github.com/dean9703111/86377e4a2825bc6671bc54ac05491643)（GitHub Gist）

完成上述步驟後，貼上如下設定：

```
---
version: '3'
services:
  file-browser:
    image: filebrowser/filebrowser
    container_name: file-browser
    ports:
      - 80:80
    restart: unless-stopped

```

原始碼：[docker-compose.yml](https://gist.github.com/dean9703111/652b821d3f743c7f345540a0e74ce9ed)（GitHub Gist）

docker 相關指令：

```
# 依照 docker-compose.yml 設定啟動服務
docker-compose up -d

# 關閉 docker 服務
docker-compose down

```

原始碼：[docker_command.sh](https://gist.github.com/dean9703111/eddfd5bcf83f9259d058647ab48976d9)（GitHub Gist）

如果啟動順利的話，這個 File Browser 系統就啟動嚕～

  預設的帳號/密碼：admin/admin

  感覺在空空的 File Browser 上面新增檔案吧～

### Bonus：增加 docker 安全性、SSL、設定資料儲存目錄

目前只是讓這個服務啟動而已，但是他並不安全；像是缺少 https 設定，如果 Docker 重啟後檔案、使用者都會消失，因此建議先做如下調整：

```
# 建立外儲存 DB 的檔案
vi docker-compose.yml

# 建立外部儲存檔案的資料夾
mkdir srv

# 調整外部儲存檔案的資料夾權限
chmod 777 srv

# 建立存放 SSL 的資料夾（對應的 .crt、.key 請自己放進去）
mkdir ssl

```

原始碼：[option_config.sh](https://gist.github.com/dean9703111/adf246d9eb1d06a31d0f80e7818ccd69)（GitHub Gist）

同時對 docker-compose.yml 進行如下修改：

```
---
version: '3'
services:
  file-browser:
    image: filebrowser/filebrowser
    container_name: file-browser
    user: 1000:1000
    ports:
      - 443:80
    volumes:
      - /apps/filebrowser/srv:/srv
      - /apps/filebrowser/filebrowser.db:/database.db
      - /apps/filebrowser/ssl:/ssl
    restart: unless-stopped
    security_opt:
      - no-new-privileges:true

```

原始碼：[new-docker-compose.yml](https://gist.github.com/dean9703111/e66e85ba0fe5558cbd08eefec1e0a48d)（GitHub Gist）

完成上述設定後，就可以重新啟動，確認設定是否生效嚕～

  docker ps 可以顯示運行中的 docker

以上就是 File Browser 的簡介以及安裝說明，原則上依照步驟就可以順利安裝及使用嚕～

### 查看 Docker log

因為不久前有同仁使用時遇到錯誤，所以我這邊也把查看 log 的步驟記錄下來。

**STEP 1**：先用 `docker ps` 列出 docker，確認 File Browser 是否還在運行中。

**STEP 2**：透過在「docker-compose.yml」設定的「container_name」來查找 log： `docker logs [container_name]` ，以這個範例來說指令是 `docker logs file-browser` 。

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202112/file-browser-cloud-file-manager/
