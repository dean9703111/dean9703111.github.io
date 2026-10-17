# 【Day26】排程 - 重開機後排程不見惹？簡單幾個步驟，從此以後完全自動

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202010/fb-ig-crawler-day-26/
- 發布：2020-10-11
- 主題：技術教學
- 系列：FB & IG 爬蟲專案從零開始
- 標籤：iT 邦幫忙鐵人賽、排程設定、Shellscript、觸發node程式
- 摘要：昨天的教學讓你的排程穩定的在背景執行，但是你重開機在終端機(Terminal)輸入 forever list 的指令你會發現 forever 根本沒有 forever

解決問題的方法 ≠ 最好的方法；我只是提出我的解法，了解問題發生的原因後你可以想想自己如果遇到了會怎麼解決

## 為什麼寫這篇文章

昨天的教學讓你的排程穩定的在背景執行，但是你重開機在終端機(Terminal)輸入 **forever list** 的指令你會發現 `forever 根本沒有 forever`

為了決這個問題，今天會教你如何在`電腦啟動時執行 script 讓 forever 跑起來`的方法，這塊我會分成 windows 跟 mac 來說明，請大家安心服用

## 今日目標

### 1. 設定 mac 排程

1.1 撰寫執行排程的 shell script ：`crawler_cron.sh`
1.2 將 crawler_cron.sh 設定為可以被執行
1.3 設定 crawler_cron.sh 在使用者登入後自動執行
1.4 重新登入確認 forever 是否運行

### 2. 設定 windows 排程

2.1 進入排程設定頁面
2.2 設定觸發條件
2.3 打開 windows 歷程紀錄
2.4 重開機確認 forever 是否運行

## 1. 設定 mac 排程

### 1.1 撰寫執行排程的 shell script ：`crawler_cron.sh`

- 在專案目錄下建立存放 shell script 的 `sh 資料夾`，並`新增 crawler_cron.sh`

- shell script 寫法很簡單，平常你怎麼在終端機下指令就怎麼寫

- **專案資料夾路徑**可用 `pwd` 指令取得

#### crawler_cron.sh

```
# 進入專案資料夾
cd /專案資料夾路徑

# 啟動排程服務
yarn forever

```

- 執行 crawler_cron.sh 確認運作邏輯跟你想的一樣

```
sh crawler_cron.sh

```

### 1.2 將 crawler_cron.sh 設定為可以被執行

建立的檔案`一開始只有讀寫功能`，我們要將 crawler_cron.sh `設定為可以被執行`，才不會在自動化時因權限不足執行失敗

- 我們先觀察 **crawler_cron.sh** 的權限

```
# 進入資料夾
cd sh
# 列出資料夾地下檔案權限
ll

```

- 目前 crawler_cron.sh 的權限是 `-rw-r--r-- `

- 權限要把他切分成 3 段來看：owner/group/others，所以代表檔案的 owner 有讀(r)寫(w)的權限，而 group/others 只有讀(r)的權限

- 而我們要讓這個檔案可以被執行(x)需要下指令更改權限

```
chmod 755 crawler_cron.sh

```

想要深入了解 chmod 檔案權限概念的請參考[這篇文章](https://shian420.pixnet.net/blog/post/344938711-%5Blinux%5D-chmod-%E6%AA%94%E6%A1%88%E6%AC%8A%E9%99%90%E5%A4%A7%E7%B5%B1%E6%95%B4!)

### 1.3 設定 crawler_cron.sh 在使用者登入後自動執行

- 我們要先確認 crawler_cron.sh 的檔案資訊

- 將打開檔案的應用程式設定成 **iTerm** (如果設定成 mac 原生的 **終端機** 也可以)

- `shell script 必須在終端機相關應用程式下執行`

- 設定 crawler_cron.sh 在使用者登入後自動執行

- 進入 **系統偏好設定** 後，選擇 **使用者與群組**

- 選擇要觸法這個 shell script 的使用者，在 **登入項目** 下按 **+** 來新增觸發的項目

- 請選擇剛剛設定為可以 **被執行的 crawler_cron.sh**

- 加入項目後把 **隱藏** 打勾

### 1.4 重新登入確認 forever 是否運行

請你重新登入後在終端機(Trerminal)輸入 **forever list** 的指令確認是否正常運行

## 2. 設定 windows 排程

### 2.1 進入排程設定頁面

- 在鍵盤同時按下 : WIN + R

- 輸入 : taskschd.msc

### 2.2 設定觸發條件

- 建立工作

- 選擇以`最高權限執行`，這裡我選擇 **不論使用者登入與否均執行** 是因為我要在開機時就觸發，而剛開機的時候使用者是還沒登入的；這塊可以依照自己的實際需求去做調整

- 在這裡設定為`啟動時`會開始工作

- 編輯動作的部分有幾個需要注意

- 程式或指令碼 : 你要找到當初安裝 yarn 的路徑，及 yarn.cmd 的檔案

- 新增引數 : 填上 **forever** ，因為我們是用 yarn forever 啟動程式的

- 開始位置 : 把專案的路徑複製上去即可

### 2.3 打開 windows 歷程紀錄

- windows 需打開歷程紀錄否則不會執行該排程

- 我們看到建立的工作在歷程紀錄旁邊顯示 **(已停用)**，我們需要把他開啟

- 在左上角 **工作排程器(本機)** 按右鍵，選擇 **啟用所有工作歷程紀錄**

- 確認你要執行工作的歷程紀錄旁邊 **沒有(已停用)**，就代表設定完成嚕

### 2.4 重開機確認 forever 是否運行

請注意因為使用的是最高權限執行 **yarn forever**，所以你在 cmd 裡面用 **forever list** 是看不到的

- 必須要用`系統管理員身分執行 cmd`

- 在這個權限下輸入指令 **forever list** 才能確認是否正常運行

### ℹ️ 專案原始碼

- 今天的完整程式碼可以在[這裡](https://github.com/dean9703111/ithelp_30days/tree/master/day26)找到喔
- 我昨天的把昨天的程式碼打包成[壓縮檔](https://github.com/dean9703111/ithelp_30days/raw/master/sampleCode/day25_sample_code.zip)，你可以在乾淨的環境試試看排程是不是重開機後依舊存在呢~

- 請記得在終端機下指令 **yarn** 才會把之前的套件安裝
- windows需下載與你chrome版本相同的[chrome driver](http://chromedriver.storage.googleapis.com/index.html)放在專案根目錄
- 要在tools/google_sheets資料夾放上自己的憑證，申請流程請參考[Day17](https://deanlin.net/blog/202010/fb-ig-crawler-day-17/)
- 調整fanspages資料夾內目標爬蟲的粉專網址
- 調整.env檔

- 填上FB登入資訊
- 填上FB版本(classic/new)
- 填上IG登入資訊
- 填上SPREADSHEET_ID
- 填上爬蟲執行時間(CRONJOB_TIME)

- 在終端機下指令 **npm install forever -g** ，讓你在終端機的任何位置都能管控排程
- 在sh資料夾中設定執行排程的shell script

### 參考資源

- [[LINUX] chmod 檔案權限大統整](https://shian420.pixnet.net/blog/post/344938711-%5Blinux%5D-chmod-%E6%AA%94%E6%A1%88%E6%AC%8A%E9%99%90%E5%A4%A7%E7%B5%B1%E6%95%B4!)
- [工作排程器 啟用歷程記錄 原顯示(已停用) windows 2012](https://cjy998.pixnet.net/blog/post/63190869-%E5%B7%A5%E4%BD%9C%E6%8E%92%E7%A8%8B%E5%99%A8-%E5%95%9F%E7%94%A8%E6%AD%B7%E7%A8%8B%E8%A8%98%E9%8C%84-%E5%8E%9F%E9%A1%AF%E7%A4%BA%28%E5%B7%B2%E5%81%9C%E7%94%A8%29--win)

**在許多人的幫助下，本系列文章已出版成書，並添加了新的篇章與細節補充：**

- 加入更多實務經驗，用完整的開發流程讓讀者了解專案每個階段要注意的事項
- 將爬蟲的步驟與技巧做更詳細的說明，讓讀者可以輕鬆入門
- 調整專案架構

- 優化爬蟲程式，以更廣的視角來擷取網頁資訊
- 增加資料驗證、錯誤通知等功能，讓爬蟲執行遇到問題時可以第一時間通知使用者
- 排程部分改用 node-schedule & pm2 的組合，讓讀者可以輕鬆管理專案程序並獲得更精確的 log 資訊

**有興趣的朋友可以到天瓏書局選購，感謝大家的支持。**
**購書連結**：[https://www.tenlong.com.tw/products/9789864348008](https://www.tenlong.com.tw/products/9789864348008)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202010/fb-ig-crawler-day-26/
