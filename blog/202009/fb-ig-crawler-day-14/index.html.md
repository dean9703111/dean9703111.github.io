# 【Day14】爬蟲執行時又又又崩潰了嗎？來點 try-catch 吧

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202009/fb-ig-crawler-day-14/
- 發布：2020-09-29
- 主題：技術教學
- 系列：FB & IG 爬蟲專案從零開始
- 標籤：iT 邦幫忙鐵人賽、try-catch、Node.js、Errorhandler
- 摘要：在 Day8 selenium-webdriver：爬蟲起手式 有使用到 try-catch 來解決抓不到 chrome driver 的例外事件，今天我們會更清楚的說明如何讓 try-catch 幫助你更高效的 debug 以及增加程式穩定性

> 你的絕望我懂！

## 回憶一下專案之前的應用場景

在 [Day8 selenium-webdriver：爬蟲起手式](https://deanlin.net/blog/202009/fb-ig-crawler-day-08/) 有使用到 try-catch 來**解決抓不到 chrome driver 的例外事件**，今天我們會更清楚的說明如何**讓 try-catch 幫助你更高效的 debug 以及增加程式穩定性**

## 今日目標

### 1. 了解什麼情境下需要用到 try-catch

1.1 try-catch-finally 基礎語法說明

1.2 專案如果少了 try-catch 會發生哪些悲劇

### 2. 在專案中應用 try-catch 實作

2.1 `loginInstagram`：確認登入的步驟是否順利

2.2 `crawlerIG`：利用 **loginInstagram函式** 回傳值判斷是否繼續執行

2.3 `goFansPage`：確認傳入的參數符合網址規則

2.4 `getTrace`：確認網頁能抓到追蹤人數的元件

## 1. 什麼情境下需要用到 try-catch?

### 1.1 try-catch-finally 基礎語法說明

在 javascript 執行過程發成錯誤時會拋出例外，接著尋找有沒有能處理這個例外的程式，如果找不到就會從拋出例外的函式跳出，尋找上一層的函式能否處理這個例外，這個過程直到觸及最外層的函式後終止

如果我們沒有做錯誤處理，程式執行過程中發生意外就會直接在那裡crash，不過我們可以用`try-catch-finally`的語句來避免程式crash

```
try {
  // 我們要執行的程式內容
  // 如果這裡執行發生錯誤就會立即進入 catch
} catch (exception) {
  // 在這裡處理例外狀況
  // 通常都會用console.log印出錯誤訊息
} finally {
  // 無論try-catch怎麼跑，最後這塊都一定會執行到
}

```

### 1.2 專案如果少了try-catch會發生哪些悲劇

- 程式在缺乏 try-catch 的機制下是非常脆落的，只要發生例外事件就容易崩潰，下面舉例幾個讓程式崩潰或是卡住的方式：

1. **把 Facebook 跟 Instagram 登入網址改成不存在的網址** → 會因為網頁不存在而卡在那個畫面
2. **把粉專的網址改成不存在的網址** → 會因為無法抓到要讀取的元件而崩潰
3. **把粉專的網址改成不存在的粉專**(或是移除的粉專) → 會因為無法抓到要讀取的元件而崩潰

- 為了避免這些悲劇的發生，**try-catch** 就是我們的好朋友，以下是應用上要注意的地方：

1. **一個try-catch區塊只判斷一件事有沒有做好**

2. 與[上一篇重構](https://deanlin.net/blog/202009/fb-ig-crawler-day-13/)提到的概念類似，一個函式專注做好一件事，try-catch 也是專注在確認這一件事執行時有沒有發生意外

3. **除了系統自行吐出的 exception 以外，建議你也要`加上console.error`(‘我在哪個步驟錯了’)**

4. 在程式結構龐大且長時間執行的狀態下，系統吐出的 exception 有時多到你眼花，這時你撰寫的 console.error 可以幫助你對發生錯誤的位置快速定位

5. **程式需要設計中斷邏輯避免進入無窮的等待**

6. 我們在抓網頁元件時用了 wait…until 的結構，如果我們沒有設定最多等待幾秒，你等到天荒地老，因為他永遠停在 try 的區塊出不去，所以 try-catch 也幫不了你

7. **透過例外處理減少多餘的步驟**

8. 像是 Instagram 我們一定要登入後才能爬蟲，所以我們就可以設定當登入失敗時(讓函式return false)不會執行後續步驟

## 2. 在專案中應用 try-catch 實作

實踐出真知，下面以 `crawlerIG.js` 裡面的函式為範例

### 2.1 `loginInstagram`

#### 調整參數讓程式GG

1. 把 IG 的登入網址改成非網址格式的字串(如：’error’)

2. 程式會crash

3. 把 IG 的登入網址改成其他網址

4. 找不到網頁上帳號密碼的文字輸入框、點擊登入的按鈕元件，程式會無限期等待

5. 輸入錯誤的登入帳密

6. 因為登入失敗網頁右上角永遠不會出現頭像元件，程式會無限期等待

#### 解決方式

1.
把這段登入的邏輯用 try-catch 包起來

2.
執行成功時回傳 true，如果執行上發生錯誤則回傳 false，終止後續爬蟲動作

3.
為 driver.wait 加上合理的等待時間(3~5秒鐘)

> PS.在程式的世界中通常時間都是以毫秒為單位，填上 3000 就是 3 秒鐘的意思

```
async function loginInstagram (driver) {
    const web = 'https://www.instagram.com/accounts/login';
    try {
        await driver.get(web)

        //為 driver.wait 加上合理的等待時間(3~5秒鐘)
        let ig_username_ele = await driver.wait(until.elementLocated(By.css("input[name='username']")), 3000);
        ig_username_ele.sendKeys(ig_username)
        let ig_password_ele = await driver.wait(until.elementLocated(By.css("input[name='password']")), 3000);
        ig_password_ele.sendKeys(ig_userpass)

        const login_elem = await driver.wait(until.elementLocated(By.css("button[type='submit']")), 3000)
        login_elem.click()

        //為 driver.wait 加上合理的等待時間(3~5秒鐘)
        await driver.wait(until.elementLocated(By.xpath(`//*[@id="react-root"]//*[contains(@class,"_47KiJ")]`)), 5000)
        return true
    } catch (e) {
        console.error('IG登入失敗')
        console.error(e)
        return false
    }
}

```

#### GOTCHA！捕獲野生錯誤

1.
將 IG 登入網址改為錯誤字串

```
//const web = 'https://www.instagram.com/accounts/login';
const web = 'error';

```

> 會因不符合網址格式跳錯誤訊息

2.
將 IG 登入網址改為其他無關網址

```
//const web = 'https://www.instagram.com/accounts/login';
const web = 'https://www.google.com';

```

> 會因找不到使用者輸入框元件超時而跳錯誤訊息

3.
把 .env環境檔裡面的登入帳號密碼改成錯誤的

```
IG_USERNAME='error'
IG_PASSWORD='error'

```

> 會因找不到頭像元件超時而跳錯誤訊息

### 2.2 `crawlerIG`

- 如果 loginInstagram函式回傳 false 代表 IG 登入失敗，後續爬蟲的動作就無需執行
- 如果 goFansPage函式回傳 false 代表該 IG帳號網址無效，就不需要去取得追蹤人數了

```
async function crawlerIG (driver) {
    const isLogin = await loginInstagram(driver, By, until)
    if (isLogin) {//如果登入成功才執行下面的動作
        const fanpage = "https://www.instagram.com/baobaonevertell/"
        const isGoFansPage = await goFansPage(driver, fanpage)
        if (isGoFansPage) {
            await driver.sleep(3000)
            const trace = await getTrace(driver)
            console.log(`IG追蹤人數：${trace}`)
        }
    }
}

```

### 2.3 `goFansPage`

#### 調整參數讓程式GG

- 將傳入的 **web_url** 改成非網址格式的字串(如：’error_page')

- 程式會crash

#### 解決方式

- 把這段登入的邏輯用 try-catch 包起來

```
async function goFansPage (driver, web_url) {
    //登入成功後要前往粉專頁面
    try {
        await driver.get(web_url)
        return true
    } catch (e) {
        console.error('無效的網址')
        console.error(e)
        return false
    }
}

```

#### GOTCHA！捕獲野生錯誤

- 將 IG 帳號網址改為錯誤字串

```
//const fanpage = "https://www.instagram.com/baobaonevertell/"
const fanpage = "error_page"

```

> 會因不符合網址格式跳錯誤訊息

### 2.4 `getTrace`

#### 調整參數讓程式GG

- 上一步 goFansPage 如果導向的並非 IG帳號網址，或者該IG帳號不存在時

> 會因為找不到網頁上追蹤人數的元件，使程式陷入無限期等待

#### 解決方式

1.
把這段登入的邏輯用 try-catch 包起來

2.
為 driver.wait 加上合理的等待時間(5秒鐘)

```
async function getTrace (driver) {
    let ig_trace = 0;//這是紀錄IG追蹤人數
    try {
        const ig_trace_xpath = `//*[@id="react-root"]/section/main/div/header/section/ul/li[2]/a/span`
        //我們採取5秒內如果抓不到該元件就跳出的條件
        const ig_trace_ele = await driver.wait(until.elementLocated(By.xpath(ig_trace_xpath)), 5000)
        // ig因為當人數破萬時文字不會顯示，所以改抓title
        ig_trace = await ig_trace_ele.getAttribute('title')
        ig_trace = ig_trace.replace(/\D/g, '')

        return ig_trace
    } catch (e) {
        console.error('無法抓取IG追蹤人數')
        console.error(e)
        return null
    }
}

```

#### GOTCHA！捕獲野生錯誤

- 將 IG 帳號網址改為不存在帳號的網址(或是無關網址)

```
//const fanpage = "https://www.instagram.com/baobaonevertell/"
const fanpage = "https://www.instagram.com/error_page_ex/"

```

> 會因找不到追蹤人數的元件而跳錯誤訊息

#### 筆者碎碎念

程式越龐大 try-catch 的機制越重要，因為隨著開發的時間軸拉長，你對過去撰寫的程式掌握度會越來越低，甚至會忘記自己曾經寫了這一段程式碼；萬一在遙遠的某一天運轉好好的程式突然崩潰了，沒有撰寫 try-catch 的人在 debug 會浪費非常多的時間，因為他無法掌握是哪裡出錯了，所以建議大家培養撰寫 try-catch 的好習慣

如果有時麼解釋不夠清楚的歡迎在下方留言討論喔

### ℹ️ 專案原始碼

- 今天的完整程式碼可以在[這裡](https://github.com/dean9703111/ithelp_30days/tree/master/day14)找到喔
- 我也貼心地把昨天的把昨天的程式碼打包成[壓縮檔](https://github.com/dean9703111/ithelp_30days/raw/master/sampleCode/day13_sample_code.zip)，你可以用裡面乾淨的環境來實作今天try-catch的部分喔

- 請記得在終端機下指令 **yarn** 才會把之前的套件安裝
- windows需下載與你chrome版本相同的[chrome driver](http://chromedriver.storage.googleapis.com/index.html)放在專案根目錄
- 調整.env檔

- 填上FB登入資訊
- 填上FB版本(classic/new)
- 填上IG登入資訊

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
引用時請附上原文網址：https://deanlin.net/blog/202009/fb-ig-crawler-day-14/
