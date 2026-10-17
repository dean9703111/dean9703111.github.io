# 【Day18】Google Sheets - 加入版控後被github警告了，官方範例做了什麼？

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202010/fb-ig-crawler-day-18/
- 發布：2020-10-03
- 主題：技術教學
- 系列：FB & IG 爬蟲專案從零開始
- 標籤：iT 邦幫忙鐵人賽、Google Sheets、Node.js、GitHub
- 摘要：在跟著官方步驟完成後，這些也是我們要注意的事情

盡信書不如無書

## 你知道官方範例在做什麼事情嗎?

在跟著官方步驟完成後，這些也是我們要注意的事情

- **我們安裝了什麼套件?**

- 如果你有用 github 版控在做這份專案，`你應該會收到 github 給你的警告通知`，今天會說明如何閱讀這個通知並解決他

- **官方的範例程式在做什麼?**

- 會教你如何觀察官方的範例程式，知道他運行的邏輯，`學會改寫，拒絕只當 copycat`

## 今日目標

### 1. 了解 github 警告通知的原因並處理

### 2. 改寫官方範例讀取自己指定的 Google Sheets

2.1 分析官方範例程式
2.2 取得自己的 spreadsheetId 並加入程式
2.3 撰寫函式 `listMySheet` 來讀取自己的 sheet

## 1. 了解 github 警告通知的原因並處理

- 昨天雖然我們按照官方給的範例完成了，但你把 code 上傳到 github 後應該會收到警告通知如下

- 我們先點開 **high severity(高嚴重性)** 的通知，閱讀後發現這是`相依套件版本過低造成的警告`

- 接著我們以 **node-forge** 為關鍵字在 **yarn.lock** 檔案裡面搜尋，發現真的版本過低

- 同時發現這個套件是跟 **google** 有關聯性的，一路順藤摸瓜往上查會發現`根源就是昨天安裝的 googleapis` 這個套件

- 來回顧一下昨天`官方是指定我們安裝 39` 這個版本的

- 接著我們直接搜尋 **googleapis npm** 來看看套件的資訊，你就會發現`最新的套件已經來到了 59` 這個版本

- 官方建議安裝的套件版本跟最新的套件版本`整整差了 20 個大版`，雖然每次更新版本都會有 release note，但我相信會認真看的沒有幾個，所以這20個大版本改了多少功能也是我們所不清楚的，`固定的版本可以給你穩定的功能，但是潛在的漏洞你無法知曉，所以如果在確認最新版本功能穩定的狀態下我會更新套件`

- 輸入指令安裝最新版本的 googleapis 套件

```
yarn add googleapis@59

```

- `每次更新版本後記得執行程式確認有無異常`

```
node tools/google_sheets/index.js

```

- 可能遇到的的錯誤

- 在 google 搜尋 **googleapis export { default as v1 } from ‘./v1.js’;** 就能找到問題是出在 node.js 版本上面，輸入指令安裝並套用新版本(請選用[LTS版本](https://nodejs.org/en/))就能解決嚕

```
nvm install 12.19
nvm use 12.19

```

## 2. 改寫官方範例讀取自己指定的 Google Sheets

### 2.1 分析官方範例程式

以下說明建議你[打開昨日的範例程式](https://github.com/dean9703111/ithelp_30days/blob/master/day17/tools/google_sheets/index.js)方便相互印證

- 首先要理解 Google 提供範例程式，`理解程式最快的方式的就是從輸出結果的位置開始回推`，所以我反推的順序會是：

- 找到輸出結果 **console.log()** 的位置 → console.log(‘Name, Major:’);
- 這個結果是在哪個 **function** 輸出的 → listMajors(auth)
- 程式在哪裏呼叫 **listMajors** 這個 function → authorize(JSON.parse(content), listMajors)
- `程式的根源`：**要先要取得憑證才能 authorize(授權)** → fs.readFile(’tools/google_sheets/credentials.json’…)

- 根據上面的架構分析，我們先觀察輸出console.log()的函式： `listMajors`，裡面有兩個參數我們要特別注意：

- **spreadsheetId**：你的Google Sheets id
- **range**：你指定讀取的範圍

- 知道要處理的參數後我們就能開始改寫了，步驟：

- 取得自己的 **spreadsheetId** 並加入程式
- 撰寫函式：`listMySheet` 讀取自己的sheet

### 2.2 取得自己的 spreadsheetId 並加入程式

- 我們先觀察官方對 `listMajors` 這隻函式的註解

```
/**
 * Prints the names and majors of students in a sample spreadsheet:
* @see https://docs.google.com/spreadsheets/d/1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgvE2upms/edit
* @param {google.auth.OAuth2} auth The authenticated Google OAuth client.
*/

```

- 再觀察裡面 `spreadsheetId` 的參數

```
spreadsheetId: '1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgvE2upms'

```

就會發現 `spreadsheetId` 這個參數在範例網址中間：
docs.google.com/spreadsheets/d/ `1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgvE2upms` /edit

- 我們建立好 Google Sheets 後`把 spreadsheetId 替換成自己的`(實際網頁位置如下圖紅框處)

```
spreadsheetId: process.env.SPREADSHEET_ID

```

- 因為 spreadsheetId 並不適合公開放到git上面(你應該不會想公布這份 Google Sheets 給全世界吧)，所以我們要把`spreadsheetId 複製起來放到 .env 裡面設定為環境變數`，以下是目前環境檔需要做的設定

#### .env.exmaple

```
#填寫自己登入IG的真實資訊(建議開小帳號來實驗，因為帳號使用太頻繁會被官方鎖住)
IG_USERNAME='ig username'
IG_PASSWORD='ig password'

#填寫自己登入FB的真實資訊(建議開小帳號來實驗，因為帳號使用太頻繁會被官方鎖住)
FB_USERNAME='fb username'
FB_PASSWORD='fb password'

#FB跑經典版還是新版(classic/new)
FB_VERSION='new'

#填寫你目標放入的spreadsheetId
SPREADSHEET_ID='your spreadsheetId'

```

### 2.3 撰寫函式 `listMySheet` 來讀取自己的 sheet

- 你可以由官方範例的[Google Sheets](https://docs.google.com/spreadsheets/d/1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgvE2upms/edit) 發現昨天程式印出來的是這個 Sheets **第一欄、第五欄** 的值

- 看看官方範例是如何印出這兩欄的數值

```
console.log('Name, Major:');
// Print columns A and E, which correspond to indices 0 and 4.
rows.map((row) => {
    console.log(`${row[0]}, ${row[4]}`);
});

```

- 接下來我們**把原本 Google 範例程式的 listMajors() 刪除**，`改寫成非同步函式listMySheet()`

```
async function listMySheet (auth) {
    const sheets = google.sheets({ version: 'v4', auth });
    const title = '我的sheet'//請你更改成自己設定的sheet(工作表)名稱
    const request = {
        spreadsheetId: process.env.SPREADSHEET_ID,
        range: [
            //這是指抓取的範圍，你也可以改寫成A1:A300(抓第1欄的第1列到第300列)
            `'${title}'!A:ZZ`
        ],
        valueRenderOption: "FORMULA"//FORMATTED_VALUE|UNFORMATTED_VALUE|FORMULA
    }
    try {
        //這裡改寫為await，之後會有順序執行的需求
        let values = (await sheets.spreadsheets.values.get(request)).data.values;
        console.log(values)
    } catch (err) {
        console.error(err);
    }
}

```

**valueRenderOption** 這個參數是把資料抓出來時的類型，感興趣可參考[Google官方文件](https://developers.google.com/sheets/api/reference/rest/v4/ValueRenderOption)

### 執行程式

- 先在自己的 Google Sheets 表格上隨機輸入文字

- 在專案資料夾的終端機(Terminal)執行指令

```
node tools/google_sheets/index.js

```

- 看看輸出的結果是否與你 Google Sheets 上的一致呢～

### ℹ️ 專案原始碼

- 今天的完整程式碼可以在[這裡](https://github.com/dean9703111/ithelp_30days/tree/master/day18)找到喔
- 我也貼心地把昨天的把昨天的程式碼打包成[壓縮檔](https://github.com/dean9703111/ithelp_30days/raw/master/sampleCode/day17_sample_code.zip)，你可以用裡面乾淨的環境來改寫Google Sheets的範例喔

- 請記得在終端機下指令 **yarn** 才會把之前的套件安裝
- 要在tools/google_sheets資料夾放上自己的憑證，今天的程式才能執行喔，申請流程請參考[Day17](https://deanlin.net/blog/202010/fb-ig-crawler-day-17/)

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
引用時請附上原文網址：https://deanlin.net/blog/202010/fb-ig-crawler-day-18/
