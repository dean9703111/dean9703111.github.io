# 【Day4】建立Node.js專案 & 全面解析專案管家package.json

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202009/fb-ig-crawler-day-04/
- 發布：2020-09-19
- 主題：技術教學
- 系列：FB & IG 爬蟲專案從零開始
- 標籤：iT 邦幫忙鐵人賽、package.json、Scripts、License、Node.js
- 摘要：前幾天都在分享一些概念以及開發環境的設定，今天我們要正式開始寫程式惹～

## 前情提要

前幾天都在分享一些概念以及開發環境的設定，今天我們要正式開始寫程式惹～

在開始前我會建議你在[github官網](https://github.com/)上面開一個帳號，這個網站是一個管理專案非常方便的工具，透過版本控管，你可以很清楚自己每天做了什麼事情，如果 commit 寫詳細一點會讓你未來想要復活某一個已經刪除的功能時有很大幫助(或是找出 bug 兇手時很有用XD)

昨天所提到 Clean Code 的議題是困擾工程師的日常，我每天也還在為了達到這些目標而努力，而想要掌握 Clean Code 最快的方法就是實戰，你實戰的經驗越多，你的程式也會越朝著 Clean Code 前進

## 今日目標

### 1. 建立 Node.js 專案

### 2. 在終端機下指令執行 Node.js

### 3. 認識 package.json

3.1 了解 package.json 各項參數的意義

3.2 善用 scipts(腳本) 來執行 Node.js

3.3 使用 license 保護自己的權益

## 1. 建立 Node.js 專案

1.
請你先建立好專案的資料夾，並使用 VScode 打開它

2.
接著新增一個 **index.js** 的檔案，內容如下

```
function helloWorld () { // 創建函式
    const content = 'helloWorld' // 給變數內容
    console.log(content) // 用日誌將內容印出來
}
helloWorld() // 觸發

```

## 2. 在終端機下指令執行 Node.js

**STEP1：** 新增完檔案後打開終端機(Termial)如下圖

**STEP2：** 在專案資料夾的終端機(Terminal)輸入指令並按下 Enter 執行

```
node index.js

```

**STEP3：** 如果有印出 `helloWorld` 的字串那就代表你踏出成功的第一步了

## 3. 認識 package.json

他是一種`描述專案的檔案，裡面包含你專案的各項資訊(名稱、描述、版本、作者、協議...)`

在昨天的文章有提到**不要重複造輪子**，多使用別人已經完善的套件，一個專案除了你自己的程式外通常還需要大量引入其他人寫的程式套件，`package.json 能幫助你管理這些套件的版本`(明天文章會詳細說明)

- **初始化 package.json**
請打開終端機(terminal)輸入如下指令，輸入後系統會要求你填寫這個專案相關的參數(這些參數之後都可以修改)

```
npm init

```

### 3.1 了解 package.json 各項參數的意義

> 真實的 package.json 不可以用 //來當備註喔，下面範例加上備註只是為了讓大家方便理解

```
{
    "name": "crawler",// 專案名稱，全部小寫，不允許空格
    "version": "0.0.1",// 須符合x.x.x的格式
    "description": "FB & IG 爬蟲30天鐵人文章",//專案描述
    "author": "dean lin",//作者名字
    "dependencies": {},//在正式環境中會用到的pacakge
    "devDependencies": {},//在開發、測試環境中會用到的pacakge
    "scripts": {},//你可以撰寫支援的command作爲腳本
    "main": "index.js",//入口檔案
    "license": "MIT"//專案協議
}

```

- 參數中的 **scripts** 能增加工作效率、**license** 關乎權益，這兩個在下方會特別介紹

### 3.2 善用 scripts(腳本) 執行程式

隨著專案的成長，執行程式需要輸入的指令會越來越複雜，但記住這些指令並且正確無誤的輸入實在太考驗人類的記憶力了；我們應該要善用 scripts 的功能，有了他我們可以**把精力放在寫程式而不是浪費時間在記住、輸入指令上面**

- **腳本結構**

```
{
    ...
    "scripts": {
        "script名稱": "你要執行的command(指令)",
        "script名稱2": "你要執行的command(指令)",
        ...
    },
    ...
}

```

- **優點**

1. 透過 scripts 讓指令長度大幅縮減
2. 可自定義多組 script 名稱，以處理不同的需求
3. 可以在 package.json 裡面管理指令，再也不用特別開一個記事本來儲存了XD

- **筆者經驗範例**
下面是筆者前端網頁在 merge branch 時所需要下的指令，我想如果是人工來下應該會崩潰吧？

- **執行內容**：移除local端套件→重新安裝套件→更新套件→根據開發環境執行

```
rm -rf node_modules package-lock.json .nuxt yarn.lock yarn-error.log && yarn && yarn upgrade && yarn dev

```

- 但是改寫成腳本後你只需要輸入指令 `yarn clean_start` 就能執行嚕

```
{
    ...
    "scripts": {
        "clean_start": "rm -rf node_modules package-lock.json .nuxt yarn.lock yarn-error.log && yarn && yarn upgrade && yarn dev"
    },
    ...
}

```

- **撰寫本專案執行的 scripts**

- 我們實際在 package.json 增加一個 scripts，並用這個 scripts 讓終端機(Terminal)印出helloWorld

```
{
    ...
    "scripts": {
        "start": "node index.js"
    },
    ...
}

```

- 使用 **npm** 執行 scripts

```
npm start

```

- 如果你有乖乖安裝 **yarn** 的環境，也可以用它執行喔

```
yarn start

```

未來隨著專案成長 scripts 也會隨著需求改寫，就讓我們跟著專案一起做中學吧～

### 3.2 使用 license 保護自己的權益

我想應該不少人建立 package.json 時都是不停按下一步使用默認參數，但如果你正在開發一個商業用專案，卻把這份專案的 license 設定成 BSD 這類可以讓其他人改寫甚至商業化的 license 就搞笑了

選擇 license 前請了解他的作用範圍，因為他會影響到這份專案的使用權益

- **依據流程圖釐清自己的需求**來做選擇，[圖片來源](https://progressbar.tw/posts/61)

- **用表格看是否符合需求**，[表格來源](https://noob.tw/open-source-licenses/)

| 條款 | GPL | LGPL | BSD | APACHE | MIT |
| --- | --- | --- | --- | --- | --- |
| 公開原始碼 | ✓ | ✓ |  |  |  |
| 以同樣方式授權 | ✓ | ✓ |  |  |  |
| 標註修改的部分 | ✓ | ✓ |  | ✓ |  |
| 必須包含 Copyright | ✓ | ✓ | ✓ | ✓ | ✓ |
| 必須包含 License | ✓ | ✓ |  | ✓ | ✓ |

### ℹ️ 專案原始碼

今天的完整程式碼可以在[這裡](https://github.com/dean9703111/ithelp_30days/tree/master/day4)找到喔

> 如果大家對於文章的內容有任何想法歡迎討論，希望這篇文章有給正在學習程式的人一些幫助與方向

### 參考資源

1. [開始使用 GitHub， 註冊與建立repo](https://progressbar.tw/posts/3)
2. [Choose an open source license](https://choosealicense.com/)
3. [常見的五個開源專案授權條款，使用軟體更自由](https://noob.tw/open-source-licenses/)
4. [在開源時代的興起下，如何透過License共享並保有權益](https://progressbar.tw/posts/61)
5. [如何使用 npm script 變成你的建立工具](http://jamestw.logdown.com/posts/1378697-egghead-how-to-use-npm-scripts-as-your-build-tool)

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
引用時請附上原文網址：https://deanlin.net/blog/202009/fb-ig-crawler-day-04/
