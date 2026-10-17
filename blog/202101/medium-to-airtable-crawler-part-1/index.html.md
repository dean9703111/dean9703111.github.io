# 透過爬蟲讓 Airtable 管理你的 Medium 文章(上) — 遇到需求時的分析以及執行步驟

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202101/medium-to-airtable-crawler-part-1/
- 發布：2021-01-15（更新：2023-05-05）
- 主題：技術教學
- 系列：Medium 文章管理工具
- 標籤：Medium 文章管理、Airtable、Airtable API、Crawler Medium
- 摘要：我有一位每天寫部落格的朋友，他的部落格至今應該已經累積了500+以上的文章了，他與他的粉絲們遇到了一個問題：『文章太多了，找不到要找的文章。』

```
這篇文章分享的內容大綱：
Side Project 緣起
一、分析需要呈現的資訊及技術
二、技術實踐步驟
三、程式執行步驟

```

## Side Project 緣起

我有一位每天寫部落格的朋友，他的部落格至今應該已經累積了500+以上的文章了，他與他的粉絲們遇到了一個問題：『文章太多了，找不到要找的文章。』

因為這個朋友主要的文章都放在 Medium 上面，為了要能更好的管理文章也開始使用 Airtable 這個表格管理工具，但因為文章的數量太大，單純靠手動去搬移及維護實在是不方便且容易有遺漏，所以就詢問我有什麼方法可以靠程式把 Medium 文章搬到 Airtable 裡面。

在與朋友細聊並且觀察他整理的 Airtable 後，我整理了下面幾點：

### 痛點分析

- Medium只能以標題搜尋文章，但有時只記得文章的tag
- 就算記得文章的類別，但同一個類別的文章太多要尋找舊文不易
- 搜尋功能無法把有相同tag的文章列出來
- 想要把把所有文章整理成表格方便自己/讀者查詢，但是光想就好累…

### 需求

- 提供簡潔好操作表格讓讀者搜尋/過濾/連結到文章
- 表格可以輕鬆維護，無需太多人工處理
- 過去的舊文也能直接匯入表格

  此為專案目標呈現的[範例](https://airtable.com/shrKfNZbU5p7rQ8xQ/tblFU6QIEZLGPekaV/viwHZDXZcUzG1ChZm?blocks=hide)

## 一、分析需要呈現的資訊及技術

### 使用 Airtable 呈現資料

- 擁有比 Google sheet 更好的視覺美感，並且對使用者來說易用性極高
- 資料的操作邏輯非常接近資料庫，官方提供的 api 也簡單好懂

### 使用 selenium 來爬蟲 Medium 的資訊

- 我們會以 Medium 的 Publication 作為根目錄往下搜尋所有文章

ps.有很多方法可以抓到資訊，我只是選擇了自己比較熟悉的工具

## 二、技術實踐步驟

STEP 1 : 先確認爬蟲能否爬到想要的資訊，如果爬不到想要的資訊就萬事皆休

STEP 2 : 閱讀 Airtable api 文件，能否達到我們的需求(讀取/寫入/刪除)

STEP 3 : 在爬蟲能抓到資料、Airtable api 能使用後，嘗試將他們串接起來

STEP 4 : 透過爬不一樣的 Medium 部落格確認是否有 bug

STEP 5 : 確認工具可以正常使用後撰寫技術文件讓大家能夠理解並使用

### 執行過程中遇到的問題

- 因為 Medium 有提供排版的功能，所以每個人的版型都略有不同，甚至有些人還是混合的版型，這在一開始造成了我漏抓一些文章
- Airtable 的 api 文件簡單易懂，但是提供的功能只有CRUD的功能而已
- 目前 Airtable 似乎還無法透過撰寫腳本來做 migration 之類的資料庫操作

## 三、程式執行步驟

下面我會簡單說明程式的執行演算邏輯，如果有興趣看原始碼的朋友可以來[我的github觀看](https://github.com/dean9703111/medium_to_airtable)喔(如果可以順手按個星星就更好惹)

STEP 1 : 抓取線上 Airtable 儲存的資訊，我們會需要這些已知的資訊來協助我們判斷有哪些文章要新增到 Airtable ，或是 Airtable 上面有哪些資訊是要被刪除的

STEP 2 : 爬蟲進入使用者指定的 Medium publication 頁面，先抓取這個頁面下有多少個 feature page(在 Airtable 會以主分類顯示)

STEP 3 : 依序進入每個 feature page 將文章的**標題、連結、發布時間**抓起來

STEP 4 : 接著會將這些文章與線上 Airtable 的資訊比對，確認是否為要寫入的新文章，如果是新文章就抓出裡面的tags

STEP 5 : 先將 feature page(主分類)、medium tags 寫入各自的 table，因為文章資訊會需要關聯這兩個部分

STEP 6 : 將新文章寫入 table

STEP 7: 如果發現 Airtable 上的資訊已經不存在於 Medium 上則會刪除該紀錄

### 如果你對這個工具感興趣，請參考下一篇：[透過爬蟲讓 Airtable 管理你的 Medium 文章(下) — 如何使用這個工具](https://deanlin.net/blog/202101/medium-to-airtable-crawler-part-2/)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202101/medium-to-airtable-crawler-part-1/
