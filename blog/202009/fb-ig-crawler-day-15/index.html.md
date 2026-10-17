# 【Day15】json x 爬蟲 = 瑣事自動化，生命應該浪費在美好的事情上

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202009/fb-ig-crawler-day-15/
- 發布：2020-09-30
- 主題：技術教學
- 系列：FB & IG 爬蟲專案從零開始
- 標籤：iT 邦幫忙鐵人賽、JSON、Crawler、Node.js、Batch
- 摘要：專案在需求規格中有一條是在自己的電腦執行，在分析完各種檔案格式的優缺點後我決定使用json格式：

即使客戶不懂程式，你也可以讓他學習一點工程師的知識

## 我們要請客戶將爬蟲的網址列表填寫到什麼檔案呢?

專案在需求規格中有一條是`在自己的電腦執行`，在分析完各種檔案格式的優缺點後我決定使用json格式：

- **txt**

- 優點：在所有作業系統中都能夠打開做編輯
- 缺點：`格式難以律定`，只要使用者多個空格、表點符號…都容易造成程式的誤判

- **excel**

- 優點：這是對使用者而言最友善的一種編輯器了，視覺化良好、操作友善
- 缺點：`不是所有人電腦都有安裝 excel` 編輯器(像我朋友就沒有)

- **json**

- 優點：在所有作業系統中都能夠打開做編輯，`key-value 的格式易於理解、程式可直接讀取`
- 缺點：使用者需要花幾分鐘時間理解 json 格式的規則

## 今日目標

### 1. 了解 json 格式與基本規則，並建立 json 格式的粉專清單

1.1 json 格式與基本規則
1.2 在專案中建立 json 格式的粉專清單

### 2. 讓程式讀取 json 裡面的資料進行爬蟲

2.1 在專案引入 json 檔案
2.2 批量爬蟲程式撰寫

## 1. 了解 json 格式與基本規則，並建立一份 json 格式的粉專清單

今天說的這個部分應該是大部分小編最關心的，要每天手動瀏覽那麼多的粉專想想都很崩潰
所以要來跟大家介紹的一個很棒的文本格式：**json** (JavaScript Object Notation) ，`他能幫助我們批量爬蟲，同時也是網頁 api 最常見的一種回傳格式`

### 1.1 json格式與基本規則

json 可以包含 `object (物件)` 與 `array (陣列)`

- **物件**

- 為 key-value 的格式，用`{...}`包起來

```
{ "key" : "value" }

{ "姓名" : "寶寶不說" } //通常用於描寫一項屬性的數值

```

- 多個物件使用 , 做為分隔

```
{ "key" : "value" , "key" : "value" }

{ "姓名" : "寶寶不說" , "性別" : "不明" } //多個物件可表達一個東西的多種屬性

```

- **陣列**

- 多個 value 之間使用 , 分隔，用`[...]`包起來

```
[ value, value ]

// 陣列與物件裡面value可以使用多種資料型態如下
[ 0 , { "傳奇寶寶" : null } , "string" , false , ['test'] ]

```

### 1.2 在專案中建立json格式的粉專清單

**建構邏輯：**

- 我們有`許多粉專需要去追蹤`，所以最外層會用 `陣列(array)`

- 而`每個粉專都有自己的屬性`(title為粉專名稱、url為粉專網址)，所以陣列裡面的 value 採用 `物件(object)`

```
[
    {
        "title": "粉專A",
        "url": "粉專A連結"
    },
    {
        "title": "粉專B",
        "url": "粉專B連結"
    }
]

```

**在專案建立的步驟：**

- 在專案資料夾中`建立 fanspages 的資料夾`，專門存放粉專頁面爬蟲要使用的json檔案
- 在 `fanspages 資料夾中新增 ig.json、fb.json` 這兩個檔案，並依照上述json結構填上你要爬蟲的資訊
- 你可以先複製下方範例做測試(筆者是以下貼圖的狂熱愛好者，歡迎一起追蹤按愛心XD)

#### ig.json

```
[
    {
        "title": "寶寶不說",
        "url": "https://www.instagram.com/baobaonevertell/"
    },
    {
        "title": "松尼",
        "url": "https://www.instagram.com/sweethouse.sl/"
    },
    {
        "title": "麻糬爸",
        "url": "https://www.instagram.com/mochi_dad/"
    },
    {
        "title": "好想兔",
        "url": "https://www.instagram.com/chien_chien0608/"
    },
    {
        "title": "ㄇㄚˊ幾兔",
        "url": "https://www.instagram.com/machiko324/"
    }
]

```

## 2. 讓爬蟲程式讀取 json 裡面的資料

### 2.1 在專案引入 json 檔案

- 引入 json 檔案時請注意相對路徑，建議先用 `console.log 確認是否有正確引入`
- 因為這個檔案是人工撰寫，可能會不小心複製到重複粉專，所以使用 `filter 來過濾`

```
let fanpage_array = require('../fanspages/ig.json');
//過濾掉重複的粉專頁面，減少資源浪費
fanpage_array = fanpage_array.filter((fanpage, index, self) =>
    index === self.findIndex(f => f.url === fanpage.url)
)
console.log(fanpage_array)

```

### 2.2 批量爬蟲程式撰寫

- 將前往粉專頁面與爬蟲的部分用 `for/of迴圈` 包起來(因迴圈內用到 **async/await**)，讓他依序前往粉專頁面抓資料

```
for (fanpage of fanpage_array) {
    // 撰寫你要對fanpage做的事
}

```

- 我們要當個好爬蟲，每個頁面設定爬蟲停留 3-6 秒，`不要因為自己的爬蟲造成別人伺服器的負擔`

這裡我們使用Math.random()取得區間亂數

```
await driver.sleep((Math.floor(Math.random()*4)+3)*1000)

```

- 統整後完整程式如下

#### crawlerIG.js

```
let fanpage_array = require('../fanspages/ig.json');
//過濾掉重複的粉專頁面，減少資源浪費
fanpage_array = fanpage_array.filter((fanpage, index, self) =>
    index === self.findIndex(f => f.url === fanpage.url)
)
const ig_username = process.env.IG_USERNAME
const ig_userpass = process.env.IG_PASSWORD
const { By, until } = require('selenium-webdriver')
exports.crawlerIG = crawlerIG;

async function crawlerIG (driver) {
    const isLogin = await loginInstagram(driver, By, until)
    if (isLogin) {
        console.log(`IG開始爬蟲`)
        for (fanpage of fanpage_array) { // 用迴圈爬蟲每個粉專
            let trace = null
            const isGoFansPage = await goFansPage(driver, fanpage.url)
            if (isGoFansPage) {
                //每個頁面爬蟲停留3~6秒，不要造成別人的伺服器負擔
                await driver.sleep((Math.floor(Math.random() * 4) + 3) * 1000)
                trace = await getTrace(driver, By, until)
            }
            if (trace === null) {
                console.log(`${fanpage.title}無法抓取追蹤人數`)
            } else {
                console.log(`${fanpage.title}追蹤人數：${trace}`)
            }

        }
    }
}
.//後面程式一樣
.
.

```

### 執行程式

- 在專案資料夾的終端機(Terminal)執行指令

```
yarn start

```

- 確認畫面執行順序如你預期：Instagram自動登入 → 依序跳轉到指定帳號 → Facebook自動登入 → 依序跳轉到粉絲頁 → 關閉

- 大概裝完一壺水的時間後，你就能看到 FB & IG 的追蹤人數嚕～

搭配上json後有沒有覺得自己功力大增XD，上面的是IG的範例，大家可以自己嘗試看看FB部分的如何改寫喔～

### ℹ️ 專案原始碼

- 今天的完整程式碼可以在[這裡](https://github.com/dean9703111/ithelp_30days/tree/master/day15)找到喔
- 我也貼心地把昨天的把昨天的程式碼打包成[壓縮檔](https://github.com/dean9703111/ithelp_30days/raw/master/sampleCode/day14_sample_code.zip)，你可以用裡面乾淨的環境來實作今天json改寫的部分喔

- 請記得在終端機下指令 **yarn** 才會把之前的套件安裝
- windows需下載與你chrome版本相同的[chrome driver](http://chromedriver.storage.googleapis.com/index.html)放在專案根目錄
- 調整.env檔

- 填上FB登入資訊
- 填上FB版本(classic/new)
- 填上IG登入資訊

- 調整fanspages資料夾內目標爬蟲的粉專網址

### 參考資源

- [JSON 格式與 JavaScript 解析教學範例](https://www.footmark.info/programming-language/javascript/json-format-and-javascript/)
- [用Math.random()取得亂數的技巧](https://ithelp.ithome.com.tw/articles/10197904)

免責聲明:文章技術僅抓取公開數據作爲研究，任何組織和個人不得以此技術盜取他人智慧財產、造成網站損害，否則一切后果由該組織或個人承擔。作者不承擔任何法律及連帶責任！

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
引用時請附上原文網址：https://deanlin.net/blog/202009/fb-ig-crawler-day-15/
