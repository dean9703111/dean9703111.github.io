# 讓你的 Postman 更專業 — Mock Server & Environments & Publish API doc

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202102/postman-mock-server-environments/
- 發布：2021-02-25（更新：2023-05-05）
- 主題：技術教學
- 標籤：Postman、Mock Server、Environment、API Documentation、Private Mock Server
- 摘要：Postman 除了 api 的基礎驗證外，還有許多加速工作效率的功能，這篇文章你將學會建立 Mock Server，從此不再苦苦等待後端 api；如果你曾經為了測試正式機/測試機/本機而開了一大堆只有網址不同的 request，了解如何設定 Environments…

Postman 除了 api 的基礎驗證外，還有許多加速工作效率的功能，這篇文章你將學會建立 Mock Server，從此不再苦苦等待後端 api；如果你曾經為了測試正式機/測試機/本機而開了一大堆只有網址不同的 request，了解如何設定 Environments 將大幅精簡 Postman 的 request 數量；常常被夥伴抱怨 api 參數問題嗎？Postman 內建的 API doc 讓合作夥伴獲得跟你一樣的環境來使用這些 api！

```
一、建立 Mock Servers
  🤔 為什麼需要學習這個技術？
  STEP 1：建立 Mock Server
  STEP 2：設計 Mock Server response
二、設定 Environments
  🤔 如果不了解怎麼設定會發生什麼蠢事？
  STEP 1：新增 Environments
  STEP 2：設定常用的 Environments variable
  STEP 3：在 request 使用 Environments variable
三、匯出 API documentation
  🤔 為什麼同事常常說我正確的api有問題？
  STEP 1：view documentation
  STEP 2：publish documentation
四、補充說明
  🤫 讓 Mock server 變成私有的

```

## 一、建立 Mock Server

### 🤔 為什麼需要學習這個技術？

過去在前後端分離的架構下，在 api 規格開出來後前端需等待後端完成 api 後才能做後續的開發；**為了不讓前端在這段時間偷懶**😏**，Mock server 應運而生！**他能回傳假資料供前端使用，等後端完成 api 後他只需要切換 end point 就能使用真實資料。

除了上面的原因外，如果你開發環境的 api sever 不太穩定常常因不明原因工作異常，**Mock server 也能保證你的工作效率不受影響**，畢竟不可能每次都等到 Server 修好後在工作吧？

### STEP 1：建立 Mock Server

Postman 改版非常快速，你看到的畫面可能跟我有些許不同

  New > Mock Server

如果不是選擇已經存在的 api Collection ，需要至少填寫一個 **Request URL** 讓Collections 有初始化的 api Request

  Create a new Collection > input url > Next

給 Mock server 取個名字，並建議你勾選儲存 Mock server URL 到環境變數的選項。

建立成功後便可以在『Mock Servers』的分頁中看到剛剛建立的 Server，點進去後能看到這個 Sever 的網址

因為剛剛有儲存 Mock server URL 到環境變數，我們到『Environments』的分頁做個確認

### STEP 2：設計 Mock Server response

在『Collections』的分頁我們能找到剛剛初始化的 request， `{{url}}` 會帶入環境變數所以右上角請選擇剛剛建立的『Mock_User』，按下『Send』後你會發現下面的 response 一片空白，這是因為我們還沒設定 Mock Server response。

你可以看到 request 下面有一個『Default』，他就是用來填寫假資料的 example，下面我們用 JSON 格式寫個簡單的 response 範例後按『Save』

再回到剛剛的 request 按下『Send』就發現會回應剛剛寫的 response 呢！

除了自動產生的 Default 外，我們還可以**自己新增其他的 example**

可以透過加入 Params 讓這個 request 有不同的 response，這裡我們新增『error』、『correct』這兩個來做示範

在 Params 填寫的 DESCRIPTION 日後會顯示在 api 文件上，如果可以也請盡量寫清楚

回到剛剛的 request ，我們將 control_res 加入『Params』，依據 VALUE 的不同按下『Send』就會看到對應的 response 嚕～

## 二、設定 Environments

### 🤔 如果不了解怎麼設定會發生什麼蠢事？

有沒有覺得上面這張圖片看起來很蠢😅，在了解 Environments 怎麼設定前，我的 Collections 就是這麼的複雜😇，因為寫程式時通常環境會有：『本機、開發機、測試機、正式機』，如果每新增一個環境你就複製一份，到最後你會無法進行妥善管理；更何況大多數的 api 其實只有網址的差異而已。

### STEP 1：新增 Environments

之前在建立 Mock Server 時其實就已經用到了 Environments，這裡我在詳細的跟大家說這個東西的好處

到『Environments』的分頁按下『＋』的按鈕，接著輸入環境的名稱即可(我用 local_server 做示範)

### STEP 2：設定常用的 Environments variable

**url**：這個是最實用的環境變數，讓你只需要調整 request 的 Environment 就快速切換網址做測試

**login_user、login_password**：每個環境的使用者登入資訊通常略有不同，且大部分的 api 都需要透過登入取得 token 才能使用，把他們加入環境變數可以加速你的測試。

P.S `不建議把正式機的 password 加入環境變數，且測試機的 password 不要用平常自己會用的密碼`，不然資安很危險的，都已經可以偷懶用 Environments，密碼自己去產生一組吧。

### STEP 3：在 request 使用 Environments variable

想在 request 中使用 Environments variable 只需要用 `{{your_env_var}}` 包起來就好，另外右上角記得要切換到正確的 Environments 才會帶入相應的資料喔！

## 三、匯出 API documentation

### 🤔 為什麼同事常常說我正確的api有問題？

相信後端工程師有一個共同經驗，無論你的 API 文件寫得再清楚功能執行的再穩定；到了同事手中卻常常被說不能 work，緊張的你檢查很多遍也找不出問題，百思不得其解的抵達同事座位才發現他原來漏了某個參數。

如果你有這樣的經驗，Postman 內建的 API documentation 能拯救你，讓你在 publish 文件後可以跟同事幸福快樂的生活。

### STEP 1：view documentation

選擇想要產生文檔的 Collection，按下『view documentation』就會顯示在右側，同時你可以透過切換右上角的環境變數來檢視文檔。

### STEP 2：publish documentation

當你確定文檔內容都沒問題後按『Publish』

接著會跳出一個網頁讓你調整一些參數，調整完後建議先用左側的『Preview Documentation』自己確認看看是否符合

確認一切都沒問題後按下『Publish Collection』就發布出去嚕～

你可以將 URL 複製起來分享給你的合作夥伴，如果你想把這份文檔下架，按『Unpublish』即可

## 四、補充說明

### 🤫 讓 Mock server 變成私有的

在前面 Mock Server 時不知道有沒有人注意到有『Make mock server private』的選項，這做這個設定前這個 mock server 的 url 都是公開的，如果你想為自己的 mock server 多一層防護可以這樣做

### STEP 1 : 編輯 Mock server

從『Mock Servers』分頁打開之前創建的 Server，按下『Edit』做設定

將『Make mock server private』打勾並按下『Save changes』

回到之前的 Request，你會發現按下 Send 後會回傳錯誤訊息，這是因為現在 Mock server 都需要添加 api key 到 request 的 header 才能使用

### STEP 2 : 產生 Postman API key

進去這個** [網址](https://go.postman.co/settings/me/api-keys) ，**點擊『Generate API key』

填上用來識別這個 API key 的名稱後按下『Generate API key』

接著請複製這個 API key，上面已經警告你這是你唯一見到他的機會了

### STEP 3：將 API key 加入 Environment

因為日後 Mock server 的所有 request 都需要這個 API key，所以我們把他加入 Environment，變數名稱為『x-api-key』

### STEP 4：request 的 header 加上 x-api-key

最後回到 request，在 Headers 的分頁下加入『x-api-key』這個參數並將環境變數 `{{x-api-key}}` 作為值後按下『Send』就看到正確的 response 回傳嚕！

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202102/postman-mock-server-environments/
