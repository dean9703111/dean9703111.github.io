# 【Day25】排程 - 用forever套件來控管排程，背景執行才是王道！

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202010/fb-ig-crawler-day-25/
- 發布：2020-10-10
- 主題：技術教學
- 系列：FB & IG 爬蟲專案從零開始
- 標籤：iT 邦幫忙鐵人賽、Forever npm、背景執行、控制排程、Node.js
- 摘要：我想很多人跟我一樣不小心順手把終端機(Terminal)給關了 (或是很乾脆地把 VScode 關了)，但視窗關閉後排程其實還在背景持續運行，下面提供一個快速的解決方案：

如果一個套件無法解決問題，那就用兩個吧！

## 思考一下昨天的排程有什麼缺點

**昨天完成的排程其實有幾個缺點：**

- 在終端機(Terminal)執行排程後你`必須保持開啟`的狀態你才知道他有沒有正確執行
- 如果不小心把執行排程的終端機(Terminal)關掉你會很困擾，因為你關掉的是終端機不是排程，`你想關掉排程還要另外下幾個指令`
- 管理困難，`啟動、停止、重啟你都要在專案資料夾`下處理
- 如果運行過程中程式`意外關閉需要手動重開`
- `在排程運行中你修改爬蟲json裡面的粉專列表是無效的`，實驗看看就會發現觸發爬蟲時跑的是舊版的粉專列表

## 今日目標

- 砍掉執行中的排程
- 使用套件 `forever` 來管理排程
- 在 package.json 中加入驅動 forever 的 scripts

## 1. 砍掉執行中的排程

我想很多人跟我一樣不小心順手把終端機(Terminal)給關了 (或是很乾脆地把 VScode 關了)，但`視窗關閉後排程其實還在背景持續運行`，下面提供一個快速的解決方案：

```
ps aux | grep [搜尋關鍵字]
kill [程式的PID]

```

### ps (process status) - 顯示進程狀態的指令

- 參數說明

- a : 列出所有使用者與 terminal 無關的所有 process
- u : 以使用者名稱顯示的格式
- x : 列出與 terminal 有關的所有 process (通常與 a 搭配使用)

- 顯示欄位 : USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND

### kill - 刪除執行中的 process

我們需要找到執行程式的 process PID 才能將其刪除 `(PID 就在 ps 進程狀態的第二欄)`

### 實際操作範例

- 找出正在運行的排程

```
ps aux | grep cron.js

```

- 你可以看到執行排程的 PID 是 23888，把他複製起來用 kill 指令刪除

```
kill 23888

```

刪除後查詢進程，確認是否刪除成功

```
ps aux | grep cron.js

```

## 2. 使用套件 `forever` 來管理排程

單純使用 cron 套件會讓你遇到開頭所說的各種問題，所以遇到過這個問題的高手們也寫了一個`讓 Node.js 程式運行永不停止的套件：forever`，這個套件操作極其簡單，不僅能讓排程在背景穩定執行，還能在終端機的任何位置做管理

- 首先要請你`全域安裝`這個套件，這樣才能在任何一個位置執行它

```
[sudo] npm install forever -g

```

- 基礎指令（以操作專案的 cron.js 為範例）

- **啟動程式**

```
forever --minUptime=1000 --spinSleepTime=1000 start tools/cron.js

```

如果你沒加上 `--minUptime=1000 --spinSleepTime=1000` 的參數 會有警告如下圖

- **監聽檔案更動並自動重啟**

```
forever --minUptime=1000 --spinSleepTime=1000 -w start tools/cron.js

```

- 這個指令可以`監控檔案的變更`，解決過去爬蟲json資料變更但是執行時跑舊資料的問題

- 要在專案資料夾下`新增 .foreverignore 的檔案`，被列入的檔案/資料夾在偵測變更時會被忽略

### .foreverignore

```
node_modules
.env
chromedriver.exe
debug.log

```

- **顯示所有運行的狀態**

```
forever list

```

- 可以在 `logfile` 裡面看到排程執行的狀況

- **關閉所有運行的程式**

```
forever stopall

```

- **重新啟動程式**

```
forever restart tools/cron.js

```

- 請注意如果你 `修改了 .env 裡面的資料，則必須要重啟才會生效`

- 執行 forever 時你可能會遇到警告如下

```
(node:30512) Warning: Accessing non-existent property 'padLevels' of module exports inside circular dependency

```

- 這個警告是因為 forever 有相依套件在 node v14 之後不再支援，`但這些警告不影響套件運行`
- Node.js 的版本更新的非常快速，許多套件都可能在更新後不支援，**所以更新版本後請切記要運行一次確保正常**
- 目前 `Node.js 的 LTS 版本為 12.19.0，如果你使用這個穩定的版本就能避免這個問題`

## 3. 在 package.json 中加入驅動 forever 的 scripts

將昨天 cron 的 scripts 改為 forever 的版本

- 為了避免重複啟動，這個 scipts 的執行邏輯為：`先暫停所有運行的 forever 程式，然後再啟動程式`

```
"forever":"forever stopall && forever --minUptime=1000 --spinSleepTime=1000 -w start tools/cron.js"

```

### 執行程式

- 在專案資料夾的終端機(Terminal)執行指令啟動排程

```
yarn forever

```

- 然後再執行指令 **forever list** 確認排程正在背景運行

### ℹ️ 專案原始碼

- 今天的完整程式碼可以在[這裡](https://github.com/dean9703111/ithelp_30days/tree/master/day25)找到喔
- 我也貼心地把昨天的把昨天的程式碼打包成[壓縮檔](https://github.com/dean9703111/ithelp_30days/raw/master/sampleCode/day24_sample_code.zip)，你可以在乾淨的環境嘗試用forever來控管排程吧

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

### 參考資源

- [如果ctrl + c沒法終止怎麼辦?](https://medium.com/mess-up/%E5%A6%82%E6%9E%9Cctrl-c%E6%B2%92%E6%B3%95%E7%B5%82%E6%AD%A2%E6%80%8E%E9%BA%BC%E8%BE%A6-5e720fd66e32)
- [Linux 程序管理 ( ps -l / ps aux / ps axjf )](http://puremonkey2010.blogspot.com/2011/02/linux-linux-ps-l-ps-aux-ps-axjf.html)
- [[Node.js打造API] 使用forever運行API永遠不停止](https://andy6804tw.github.io/2018/01/17/api-forever/)

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
引用時請附上原文網址：https://deanlin.net/blog/202010/fb-ig-crawler-day-25/
