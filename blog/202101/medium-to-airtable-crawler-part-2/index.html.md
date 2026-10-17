# 透過爬蟲讓 Airtable 管理你的 Medium 文章(下) — 如何使用這個工具

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202101/medium-to-airtable-crawler-part-2/
- 發布：2021-01-16（更新：2023-05-05）
- 主題：技術教學
- 系列：Medium 文章管理工具
- 標籤：Airtable、Medium、Medium 爬蟲、文章管理、Medium 匯入 Airtable
- 摘要：如果你正在因為 Medium 文章太多不知道如何管理的話，這是一個很方便的工具，下面會完整說明使用這個工具的步驟

如果你對這個專案的起源感興趣，請參考上一篇：[透過爬蟲讓 Airtable 管理你的 Medium 文章(上) — 遇到需求時的分析以及執行步驟](https://deanlin.net/blog/202101/medium-to-airtable-crawler-part-1/)

```
文章大綱：
一、完成 Airtable 的設定
二、安裝工具將指定 Medium 的部落格匯入 Airtable
三、驗收成果

```

如果你正在因為 Medium 文章太多不知道如何管理的話，這是一個很方便的工具，下面會完整說明使用這個工具的步驟

## 一、完成 Airtable 的設定

- 請[註冊或是登入airtable](https://airtable.com/login)

2. 請[使用我已經創建好的template](https://airtable.com/shrqrLtvUxDyWFLjK/tblrYddyJH1cFQLm6/viwv6U7hnzgvKTZ14?blocks=hide)，進去後按右上角的 Copy base

3. 選擇你要放 base 的位置(使用默認的即可)

4. 點擊剛剛創建的 base，確認欄位都有複製過來

5. [進入accout頁面](https://airtable.com/account)產生 API key，產生後請將 key 妥善保存，之後程式會用到這個參數(AIRTABLE_API_KEYKEY)

6. [進入api頁面](https://airtable.com/api)選擇剛剛新增的 base

7. 依據下圖步驟找到 base 的代碼，並將他紀錄起來，之後程式會用到這個參數(AIRTABLE_BASE)

## 二、安裝工具將指定 Medium 的部落格匯入 Airtable

開發所需要的前置環境設定我寫在這篇文章：[工程師的開發環境大補帖：Node.js、NVM、git、yarn、VSCode](https://deanlin.net/blog/202101/developer-environment-setup/)

- 從 github下載

```
git clone https://github.com/dean9703111/medium_to_airtable.git

```

2. 使用 VSCode 開啟資料夾，並點擊 TERMINAL 進入資料夾終端機

3. 安裝套件

```
yarn

```

4. 設定專案環境變數 將`.env.exmaple`複製一份到`.env`檔案(須自行新增)

### 變數說明：

- MEDIUM_PAGE：你要爬的 Medium 文章網址，你可以先用預設的值，確定程式能跑起來後再改成自己的網址
- SHOW_BROSWER：設定為 true 時會有瀏覽器跳出來，設定為 false 時則是讓爬蟲背景執行
- AIRTABLE_API_KEYKEY：在上面步驟申請到的 API key
- AIRTABLE_BASE：你目標要儲存 BASE 的代碼

5. 執行程式

```
yarn start

```

我們可以從 TERMINAL 看到程式執行的過程

結束後會顯示總共花費時間

## 三、驗收成果

去 Airtable 確認是否有正確匯入資料，[此為範例](https://airtable.com/shrR0v4IDFB2rwUDj)

希望大家都成功將 Medium文章匯入 Airtable 惹~如果有任何問題歡迎在下方詢問，如果覺得這篇文章對你有幫助可以幫我按拍手給我鼓勵喔~

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202101/medium-to-airtable-crawler-part-2/
