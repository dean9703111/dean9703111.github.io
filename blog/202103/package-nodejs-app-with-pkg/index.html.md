# 把 Node.js 專案打包成執行檔，讓朋友無須安裝環境就能暢快體驗 — pkg的介紹以及實際範例分享

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202103/package-nodejs-app-with-pkg/
- 發布：2021-03-05（更新：2023-05-05）
- 主題：技術教學
- 標籤：Node.js、執行檔打包工具、Pkg、無需環境設定、產生執行檔
- 摘要：如果你是一個很常寫 side project 的人，你一定遇到過想跟朋友炫耀自己完成的 side project；但卻因為雙方電腦環境不同而無法炫耀的問題🤣，今天跟各位介紹一款打包 node 專案成為執行檔的工具 — 『pkg』，同時佐以我過去分享過的 side…

如果你是一個很常寫 side project 的人，你一定遇到過想跟朋友炫耀自己完成的 side project；但卻因為雙方電腦環境不同而無法炫耀的問題🤣，今天跟各位介紹一款打包 node 專案成為執行檔的工具 — 『pkg』，同時佐以我過去分享過的 side project — 『透過爬蟲讓 Airtable 管理你的 Medium 文章』作為此工具的驗證。

```
一、打包工具 pkg
  ➤ 安裝
  ➤ pkg 打包參數說明
  ➤ package.json 參數說明
二、實際範例展示
  ➤ package.json 參數設定
  ➤ 產生執行檔
  ➤ 執行上可能遇到的問題
  ➤ 搭配環境變數執行的方式

```

## 一、打包工具 pkg

### 安裝

全局安裝，讓每個 Node.js 專案都能使用

```
npm install -g pkg

```

### pkg 打包參數說明

安裝完後輸入 `pkg -h` 會顯示他能用的打包參數，下面說明常用的：

- -t：目標打包給哪個系統用的(Linux, macOS and Windows)，如未設定會產生 3 個檔案
- -c：打包會參考 package.json 或是其他 json 檔案
- -o：檔案輸出名稱

### package.json 參數說明

```
{
    "pkg": {
        "scripts": [
            "build/**/*.js"//需要打包的其他js文件
        ],
        "assets": [
            "dist/**/*"//靜態文件目錄
        ]
    }
}

```

## 二、實際範例展示

以下範例的原始碼：[https://github.com/dean9703111/medium_to_airtable](https://github.com/dean9703111/medium_to_airtable)

有時你設計了一個很有趣的功能，但如果他的前置作業太麻煩，非相關領域的人沒法無腦操作時就會阻礙他的推廣；相反如果他毫無使用門檻就會很多人願意嘗試看看。

這裏我就使用之前分享的 side project — 『透過爬蟲讓 Airtable 管理你的 Medium 文章』一步步說明如何操作。

### package.json 參數設定

- **撰寫 scripts**：記得不要去死背套件指令，需要記憶的事情交給電腦就好。這裏我新增了一個『build』的指令，他會以 main.js 作為入口檔，依據 package.json 的設定產生一個給 macOS 使用的執行檔。
- **設定 pkg**：把『node_modeles』的資料夾全部視為靜態檔即可

```
{
...
  "scripts": {
    "build": "pkg main.js -t node12-macos-x64 -c package.json"
  },
  "pkg": {
    "assets": [
      "node_modules/**/*"
    ]
  }
...
}

```

### 產生執行檔

在上一步我們寫好 scripts 後可以用如下指令產生執行檔：

- 使用 npm：`npm run build`
- 使用 yarn：`yarn build`

如果沒有特別指定檔案產生的位置就會出現在資料夾的根目錄喔～

### 執行上可能遇到的問題

你可以直接下載macOS的執行檔玩玩看：[https://github.com/dean9703111/medium_to_airtable/raw/master/app/medium-to-airtable](https://github.com/dean9703111/medium_to_airtable/raw/master/app/medium-to-airtable)

只要用 Terminal 前往檔案資料夾的位置，輸入 `./medium-to-airtable` 即可執行，但應該會遇到如下錯誤：

回去看程式碼會發現錯誤的原因是因為無法讀取 .env 的環境變數

### 搭配環境變數執行的方式

所以我們只需要在執行檔案前輸入原本設定在 .env 檔的資料就能夠順利執行：

```
ENV1=xxx ENV2=yyy ENV3=zzz ./execute_file

```

以這個 side-project 來說會長這個樣子（參數要怎麼填請[參考這篇文章](https://deanlin.net/blog/202101/medium-to-airtable-crawler-part-2/)）：

```
MEDIUM_PAGE="https://medium.com/你的網頁" SHOW_BROWSER=false AIRTABLE_API_KEYKEY='你申請的api' AIRTABLE_BASE='airtable的id' ./medium-to-airtable

```

然後就可以順利執行嚕～

```
如果你對這個專案有興趣可參考相關系列文章：
➤ 透過爬蟲讓 Airtable 管理你的 Medium 文章(上) — 遇到需求時的分析以及執行步驟
➤ 透過爬蟲讓 Airtable 管理你的 Medium 文章(下) — 如何使用這個工具
➤ 透過爬蟲讓 Airtable 管理你的 Medium 文章(補充篇) — 文章字數統計

```

認真說這個打包工具比我想像的還要神，而且使用起來也沒什麼難度，如果手上剛好有熱騰騰的 side-project 趕快用這個套件來炫耀一波吧！

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202103/package-nodejs-app-with-pkg/
