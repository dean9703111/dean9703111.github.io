# 【Day21】Google Sheets - BUG!爬蟲資料塞錯位置 & 專案出包怎麼辦？

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202010/fb-ig-crawler-day-21/
- 發布：2020-10-06
- 主題：技術教學
- 系列：FB & IG 爬蟲專案從零開始
- 標籤：iT 邦幫忙鐵人賽、Google Sheets、客戶溝通、解決bug
- 摘要：其實這份專案完成到昨天的進度時就給業主做功能上的確認準備結案了 但一個案子通常很難一次就通過，無論這隻程式在你電腦上執行的多麼穩定，放到別人的電腦上總會冒出大大小小的 bug，我大概歸類成以下幾種：

客戶的操作跟你想的不一樣

## 為什麼寫這篇文章？

其實這份專案完成到昨天的進度時就給業主做功能上的確認準備結案了
但一個案子通常很難一次就通過，`無論這隻程式在你電腦上執行的多麼穩定，放到別人的電腦上總會冒出大大小小的 bug`，我大概歸類成以下幾種：

- 作業系統不同導致的

- Windows、MacOS、Ubuntu Linux (ex：爬蟲時需要設定各自的 webdriver)

- 套件版本不同導致

- Node.js版本、npm 套件 (ex：有些 npm套件只能在特定 Node.js版本運行)

- 電腦運行環境

- 網路速度、硬體規格… (ex：如果你的網路環境太差需要去調整爬蟲元件的極限等待時間)

- `客戶的操作跟你預想的不一樣`

第 1、2、3 點的問題我在之前文章實作碰到時有提到過，今天來講講`實際上你會最崩潰的第 4 點`

## 今日目標

### 1. 將客戶發的 bug 做基礎分類

### 2. 引導客戶說出 bug 發生原因及提出解決方案的小技巧

### 3. 重現客戶發生的 bug

### 4. 改寫程式讓新的爬蟲資料塞入正確位置

4.1 取得 Google Sheets 第一欄的粉專名稱：`readTitle`
4.2 讓新的爬蟲資料正確寫入 Google Sheets：`writeSheet`

## 1. 將客戶發的 bug 做基礎分類

與你合作的客戶通常不是資工相關背景，他們所提出來的 bug 有時也會讓你啼笑皆非，我個人會將他們提出的 bug 分成 3 種

- **明顯的 bug**：客戶一報 bug 你就知道哪個環節出問題了，就認命地解決吧 (ex：臉書的爬蟲登入後就卡住不動了)
- **需要討論的 bug**：`客戶報 bug 時你不確定是哪個環節出了問題，需要與客戶討論發生 bug 的操作流程`
- **不合理的 bug**：明顯不合理的要求你可以委婉的跟他說為什麼辦不到 (ex：程式執行到一半把電腦關機問你爬蟲資料為什麼沒有寫入 Google Sheets)

## 2. 引導客戶說出 bug 發生原因及提出解決方案的小技巧

很多時候客戶對於 bug 的陳述是很模糊的，所以你**需要與客戶溝通，引導他們說出自己的操作流程**，以今天發出的 bug 做舉例：

**客戶**：今天爬蟲資料寫入 Google Sheets 表單時塞錯位置了！
**我**：請問這個問題是今天突然發生的，還是過去幾天也發生過通樣的問題呢？ `(釐清 bug 發生的時間點)`
**客戶**：是今天才發生的
**我**：那今天你在跑爬蟲前有做過什麼調整嗎？ `(確認是什麼樣的操作導致 bug)`
**客戶**：我在爬蟲的json 檔裡面新增了幾個粉專，然後把 Google Sheets 上面一些沒有在維護的粉專給砍掉
**我**：了解，我這邊會先重複一遍您的操作來釐清問題，最晚會在兩天後給您答覆 `(自己操作一遍確認有這個bug)`

===我是正在重現bug的分隔線===

**我**：我依照您的方式操作確實遇到這個問題；這邊我提出一個解決方案，這個方案會將你新增的爬蟲粉專寫入 Google Sheets 表單的最下面，不管你爬蟲json 如何調整，追蹤人數都會填入 Google Sheets 的正確位置，詳細邏輯你可以參考下方說明 `(自己提出解決方案，千萬別讓客戶天馬行空亂提)`

- 導致錯誤發生的操作：
1. 人工整理 Google Sheets粉專名稱的欄位：刪除、上下置換
2. 調整爬蟲json 內容：新增/刪除/上下置換
- 解決錯誤的方式：
1. 粉專名稱：寫入前將 Google Sheets 上粉專名稱的欄位與爬蟲json 比對，如果 **爬蟲json 裡面有新的粉專就會新增到最下面，不改變 Google Sheets 上粉專名稱原有排列順序**
2. 追蹤人數：以 **粉專名稱+粉專網址作為寫入追蹤人數欄位的判斷**

**客戶**：好的先這樣做吧，你什麼時候能改好？
**我**：預計三個工作天，如果提早完成會儘早與您聯繫 `(說一個你絕對能完成功能的時間，並表達積極性)`

## 3. 重現客戶發生的 bug

這個步驟非常重要，`如果 bug 無法在自己的環境重現時你會超級頭大`，因為站在客戶的角度就是認為你沒有完成專案，這會導致你結案的日子變得遙遙無期…

今天問題重現起來非常簡單，跟著客戶的說的方式做就好：`調整爬蟲json 及 Google sheet`

- 我們這裡用 fb.json 為例，將**麻糬爸愛亂畫**刪除，然後把**寶寶不說**移到第二個

```
[

    {
        "title": "鹿尼",
        "url": "https://www.facebook.com/%E9%B9%BF%E5%B0%BC-260670294340690/"
    },
    {
        "title": "寶寶不說",
        "url": "https://www.facebook.com/baobaonevertell/"
    },
    {
        "title": "好想兔",
        "url": "https://www.facebook.com/chien760608/"
    },
    {
        "title": "ㄇㄚˊ幾兔",
        "url": "https://www.facebook.com/machiko324/"
    }
]

```

- 執行程式 **yarn start** 後你就會發現excel的表格亂掉了

## 4. 改寫程式讓新的爬蟲資料塞入正確位置

### 4.1 取得 Google Sheets 第一欄的粉專名稱：`readTitle`

- 當這個 sheet 全新的時候內容是 undefine，但是因為我們等等再做對比時用的格式為 array，所以要給他一個初始型別

```
async function readTitle (title, auth) {
    const sheets = google.sheets({ version: 'v4', auth });
    const request = {
        spreadsheetId: process.env.SPREADSHEET_ID,
        ranges: [
            `'${title}'!A:A`
        ],
        valueRenderOption: "FORMULA"
    }
    try {
        let title_array = []
        let values = (await sheets.spreadsheets.values.batchGet(request)).
            data.valueRanges[0].values;
        if (values) {//如果沒資料values會是undefine，所以我們只在有資料時塞入
            title_array = values.map(value => value[0]);
        }
        // console.log(title_array)
        return title_array
    } catch (err) {
        console.error(err);
    }
}

```

### 4.2 讓新的爬蟲資料正確寫入 Google Sheets：`writeSheet`

我們依照客戶接受的`解決方案`來調整這隻函式

- 將爬蟲json 中新增的粉專`補到 Google Sheets 最後面`
- 追蹤人數用`粉專名稱+粉專網址`來塞入對應的位置

```
async function writeSheet (title, result_array, auth) {
  // 取得線上第一欄的粉專名稱
  let online_title_array = await readTitle(title, auth)
  // 如果json檔有新增的粉專就補到最後面
  result_array.forEach(fanpage => {
    if (!online_title_array.includes(`=HYPERLINK("${fanpage.url}","${fanpage.title}")`)) {
      online_title_array.push(`=HYPERLINK("${fanpage.url}","${fanpage.title}")`)
    }
  });

  // "粉專名稱+粉專網址"作為寫入追蹤人數欄位的判斷
  let trace_array = []
  online_title_array.forEach(title => {
    let fanpage = result_array.find(fanpage => `=HYPERLINK("${fanpage.url}","${fanpage.title}")` == title)
    if (fanpage) {
      trace_array.push([fanpage.trace])
    } else {
      trace_array.push([])
    }
  });

  const datetime = new Date()
  if (online_title_array[0] !== title) {//如果是全新的sheet就會在開頭插入
    online_title_array.unshift(title)
    trace_array.unshift([dateFormat(datetime, "GMT:yyyy/mm/dd")])
  } else {//如果不是全新就取代
    trace_array[0] = [dateFormat(datetime, "GMT:yyyy/mm/dd")]
  }

  await writeTitle(title, online_title_array.map(title => [title]), auth)
  let lastCol = await getLastCol(title, auth)
  await writeTrace(title, trace_array, lastCol, auth)
}

```

### 執行程式

本次目標需多次執行程式你才能確保程式運作正確

- 依照自己的想法去編修線上的 Google Sheets、調整爬蟲json 內容

- 在專案資料夾的終端機(Terminal)執行指令

```
yarn start

```

- 在爬蟲跑完後看看線上的 Google Sheets 是否有依照你的更改寫入正確位置

下圖是我多次修改 fb.json 後執行爬蟲的結果

### ℹ️ 專案原始碼

- 今天的完整程式碼可以在[這裡](https://github.com/dean9703111/ithelp_30days/tree/master/day21)找到喔
- 我也貼心地把昨天的把昨天的程式碼打包成[壓縮檔](https://github.com/dean9703111/ithelp_30days/raw/master/sampleCode/day20_sample_code.zip)，你可以用裡面乾淨的環境來解決客戶回報的bug喔

- 請記得在終端機下指令 **yarn** 才會把之前的套件安裝
- windows需下載與你chrome版本相同的[chrome driver](http://chromedriver.storage.googleapis.com/index.html)放在專案根目錄
- 要在tools/google_sheets資料夾放上自己的憑證，申請流程請參考[Day17](https://deanlin.net/blog/202010/fb-ig-crawler-day-17/)
- 調整fanspages資料夾內目標爬蟲的粉專網址
- 調整.env檔

- 填上FB登入資訊
- 填上FB版本(classic/new)
- 填上IG登入資訊
- 填上SPREADSHEET_ID

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
引用時請附上原文網址：https://deanlin.net/blog/202010/fb-ig-crawler-day-21/
