# 【Day11】舉一反三，帶你了解IG爬蟲不可忽略的細節

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202009/fb-ig-crawler-day-11/
- 發布：2020-09-26
- 主題：技術教學
- 系列：FB & IG 爬蟲專案從零開始
- 標籤：iT 邦幫忙鐵人賽、Instagram、Crawler、Login、Tracefans
- 摘要：有了 Facebook 爬蟲的經驗後，我相信這篇大家是有能力獨自完成的，建議大家先用自己的方式來完成今日目標，這篇文章適合實作遇到問題或是你實作完成後再來參考

> 學習，從複製開始

## 筆者有話先說

有了 Facebook 爬蟲的經驗後，我相信這篇大家是有能力獨自完成的，建議大家先用自己的方式來完成今日目標，這篇文章適合實作遇到問題或是你實作完成後再來參考

## 今日目標

### 1. 完成 IG 自動登入

1.1 分析 IG 登入頁面的元件

1.2 改用 css 抓取元件，完成 IG 自動登入

### 2. 從畫面判斷使用者是否登入成功

### 3. 前往 IG 帳號並取得追蹤人數

3.1 了解響應式網頁對爬蟲的影響

3.2 設定瀏覽器開啟時的視窗大小

3.3 完成前往 IG 帳號並取得追蹤人數的程式

## 1. IG 自動登入

### 1.1 分析 IG 登入頁面的元件

- 我在反覆使用 [IG 登入](https://www.instagram.com/accounts/login) 時發現登入的畫面偶爾會長不一樣(如下圖)導致Xpath路徑錯誤

| 版本1 | 版本2 |
| --- | --- |

|

- 先進入開發者模式確認紅框內元件的特徵

- 在下圖我們觀察到 **電話號碼、用戶名稱或電子郵件** 以及 **密碼** 這兩個元件的 **共通點**：

1.
都是 `input 的 html 標籤`

2.
用 `name 這個 attribute` 來表示他們的作用

- **登入按鈕** 則用以下條件來取出：

1.
使用 `button 的 html 標籤`

2.
用 `type 這個 attribute` 來表示他要執行的動作

### 1.2 改用 css 抓取元件，完成 IG 自動登入

- 根據上面的分析後我們改用 css 抓取 IG 登入的元件來操作

```
//填入ig登入資訊
let ig_username_ele = await driver.wait(until.elementLocated(By.css("input[name='username']")));
ig_username_ele.sendKeys(ig_username)
let ig_password_ele = await driver.wait(until.elementLocated(By.css("input[name='password']")));
ig_password_ele.sendKeys(ig_userpass)

//抓到登入按鈕然後點擊
const login_elem = await driver.wait(until.elementLocated(By.css("button[type='submit']")))
login_elem.click()

```

> 別忘記在 .env檔填寫 IG登入的帳號密碼
> ```
> #填寫自己登入IG的真實資訊(建議開小帳號來實驗，因為帳號使用太頻繁會被官方鎖住)
> IG_USERNAME='ig username'
> IG_PASSWORD='ig password'
> ```

## 2. 判斷使用者是否登入成功

- 與 FB 類似，IG 要登入之後才會有右上角功能列，這裡我使用他作為判斷登入與否

```
//登入後才會有右上角功能列，我們以這個來判斷是否登入
await driver.wait(until.elementLocated(By.xpath(`//*[@id="react-root"]//*[contains(@class,"_47KiJ")]`)))

```

## 3. 抓出 IG 追蹤人數

### 3.1 了解響應式網頁對爬蟲的影響

- **響應式網頁設計(RWD)是什麼？**：網頁會根據瀏覽器解析度的不同重新排版，同一個網頁你在手機、平板、電腦看到的畫面配置都是不一樣的
- 如果你跟我一樣充滿實驗精神，你會發現 IG 的頁面會隨著螢幕寬度更改而重新排版，Xpath 的路徑也會跟著改變

- **寬螢幕** Xpath

```
//*[@id="react-root"]/section/main/div/header/section/ul/li[2]/a/span

```

- **窄螢幕** Xpath

```
//*[@id="react-root"]/section/main/div/ul/li[2]/a/span

```

### 3.2 設定瀏覽器開啟時的視窗大小

- 為了避免因為螢幕寬度不一樣造成 Xpath 不同，所以我們要設定瀏覽器開啟時的視窗大小(本專案以寬螢幕作為範例)，這樣才能保證你的爬蟲在不同電腦有一樣的執行結果

```
// 建立browser的類型
let driver = await new webdriver.Builder().forBrowser("chrome").withCapabilities(options).build();
//考慮到ig在不同螢幕寬度時的Xpath不一樣，所以我們要在這裡設定統一的視窗大小
await driver.manage().window().setRect({ width: 1280, height: 800, x: 0, y: 0 });

```

### 3.3 完成前往 IG 帳號並取得追蹤人數的程式

- 在固定螢幕寬度後，使用 Xpath 就能穩定的抓出帳號追蹤人數

```
//登入成功後要前往粉專頁面
const fanpage = "https://www.instagram.com/baobaonevertell/"
await driver.get(fanpage)
await driver.sleep(3000)

let ig_trace = 0;//這是紀錄IG追蹤人數
const ig_trace_xpath = `//*[@id="react-root"]/section/main/div/header/section/ul/li[2]/a/span`
const ig_trace_ele = await driver.wait(until.elementLocated(By.xpath(ig_trace_xpath)))
// ig因為當人數破萬時文字不會顯示，所以改抓title
ig_trace = await ig_trace_ele.getAttribute('title')
console.log(`追蹤人數：${ig_trace}`)
driver.quit();

```

### 執行程式

1.
在專案資料夾的終端機(Terminal)執行指令

```
yarn start

```

2.
你會看到 Instagram 自動登入 → 跳轉到指定帳號 → 關閉

3.
確認輸出正確的粉專追蹤人數你就成功嚕～

到目前為止我們已經可以抓出 FB & IG 粉專的追蹤人數了，充滿好奇心的讀者可以先試著看看利用爬蟲爬完 FB 粉專後繼續爬 IG

### ℹ️ 專案原始碼

- 今天的完整程式碼可以在[這裡](https://github.com/dean9703111/ithelp_30days/tree/master/day11)找到喔
- 我也貼心地把昨天的把昨天FB爬蟲的程式碼打包成[壓縮檔](https://github.com/dean9703111/ithelp_30days/raw/master/sampleCode/day10_sample_code.zip)，你可以用裡面乾淨的環境來實作今天的功能喔

- 請記得在終端機下指令 **yarn** 才會把之前的套件安裝
- windows需下載與你chrome版本相同的[chrome driver](http://chromedriver.storage.googleapis.com/index.html)放在專案根目錄
- 調整你.env檔填上IG登入資訊

### 參考資源

1. [Filling in login forms in Instagram using selenium and webdriver (chrome) python OSX](https://stackoverflow.com/a/49940401)

> 免責聲明:文章技術僅抓取公開數據作爲研究，任何組織和個人不得以此技術盜取他人智慧財產、造成網站損害，否則一切后果由該組織或個人承擔。作者不承擔任何法律及連帶責任！

> **在許多人的幫助下，本系列文章已出版成書，並添加了新的篇章與細節補充：**
> 加入更多實務經驗，用完整的開發流程讓讀者了解專案每個階段要注意的事項
> 將爬蟲的步驟與技巧做更詳細的說明，讓讀者可以輕鬆入門
> 調整專案架構
> 優化爬蟲程式，以更廣的視角來擷取網頁資訊
> 增加資料驗證、錯誤通知等功能，讓爬蟲執行遇到問題時可以第一時間通知使用者
> 排程部分改用 node-schedule & pm2 的組合，讓讀者可以輕鬆管理專案程序並獲得更精確的 log 資訊
> **有興趣的朋友可以到天瓏書局選購，感謝大家的支持。**
> **購書連結**：[https://www.tenlong.com.tw/products/9789864348008](https://www.tenlong.com.tw/products/9789864348008)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202009/fb-ig-crawler-day-11/
