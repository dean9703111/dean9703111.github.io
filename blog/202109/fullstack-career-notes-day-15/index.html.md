# [面試][後端]在正式 API 完成前，如何讓要串接的工程師不要空等？

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202109/fullstack-career-notes-day-15/
- 發布：2021-09-30
- 主題：職涯成長
- 系列：全端工程師生存筆記
- 標籤：iT 邦幫忙鐵人賽、後端面試、Postman、Mock Server、API Document
- 摘要：建立 Mock Server 後，前端工程師就失去耍廢時間不需要等後端做好才能串接。

建立 Mock Server 後，前端工程師就失去耍廢時間不需要等後端做好才能串接。

前端工程師心裡 OS：「其實我是想要空等的。」

## 大綱

- **在正式 API 完成前，如何讓未來要串接的工程師不要空等？**

- 1.1 面試官為什麼會問？
- 1.2 面試官想從答案確認什麼？
- 1.3 筆者提供的簡答

- **回答問題所需具備的知識**

- 2.1 Mock Server 是什麼？學它有什麼好處？
- 2.1 用 Postman 建立 Mock Server
- 2.3 設計 Mock Server Response
- 2.4 設定 Environment 讓你快速切換執行環境
- 2.5 匯出 API documentation 給同事使用指引
- 2.6 讓 Mock Server 變成私有的

## 1. 在正式 API 完成前，如何讓要串接的工程師不要空等？

### 1.1 面試官為什麼會問？

這問題是想`了解你現在的工作是如何運行的`，因為許多專案都採取前後端分離的架構，前端常常要等後端完成後才能串接，如果後端提供的服務不穩定，又或是沒按造需求規格開發就容易導致前端空等。

### 1.2 面試官想從答案確認什麼？

- 你現在工作時，前端與後端是如何協作的
- 你有用什麼工具，讓前端不用等你寫完 API 就能提前串接
- 你有提供什麼文檔供前端串接時參考
- 如果回答的是某個工具（ex：Postman），會再詳細詢問有使用到工具的哪些功能

### 1.3 筆者提供的簡答

我會先依照需求規格整理出一份讓雙方`共同遵守的開發文檔`，同時使用第三方驗證工具「`Postman`」建立 `Mock Server`，並根據 Request 的參數給予對應的 Result 結果，讓前端不需要等後端 API 完成後才能開始串接的作業。

## 2. 回答問題所需具備的知識

市面上提供 Mock Server 以及產生 API doc 的工具很多，不過「Postman」是筆者用起來最順手的；Postman 除了 API 的基礎測試外，還有許多加速工作效率的功能，就讓我們透過這篇文章讓自己的 Postman 更專業吧！

### 2.1 Mock Server 是什麼？學它有什麼好處？

在前後端分離的架構下，前端在完成網頁版面佈局後，需等待後端完成 API 才能做後續的開發；為了不讓前端在這段時間偷懶苦等，Mock Server 應運而生！`它能回傳自訂的資料結構供前端使用`，等後端完成 API 後，前端只需要切換網址就能使用真實資料。

除了上面的原因外，如果你開發環境的後端 Sever 不太穩定常常罷工，Mock Server 也能保證你的工作效率不受影響，畢竟不可能每次都等到 Server 修好後再工作吧？

### 2.2 用 Postman 建立 Mock Server

Postman 改版非常快速，你看到的畫面可能跟我有些許不同

- **STEP 1**： 建立 Mock Server

- New > Mock Server

- Create a new Collection > input url > Next
如果不是選擇已經存在的 API Collection，就需要`至少填寫一個 Request URL` 讓 Collections 有初始化的 API Request

- 給 Mock Server 取個名字，並建議你勾選`儲存 mock server URL 到環境變數`的選項。

- **STEP 2**： 檢查 Mock Server 是否建立成功

- 建立成功後便可以在「Mock Servers」的分頁中看到剛剛建立的 Server，點進去後能看到這個 Sever 的網址。

- 接著我們到「Environments」的分頁確認，mock server URL 是否有儲存到環境變數，

### 2.3 設計 Mock Server Response

在 Mock Server 建立後，我們要設計不同 API Request 回傳的資料結構。

- **STEP 1**：先用 Request 的 Default example 來了解使用方式

- 到「Collections」的分頁找到剛剛初始化的 Request， `{{url}}`會帶入環境變數所以右上角請選擇剛剛建立的「Mock_User」，按下「Send」後你會發現下面的 Response 一片空白，這是因為我們還沒設定 Mock Server Response。

- 在左側 Request 下面有一個「Default」，`它就是用來自訂 Response 資料結構的 example`，這邊我們用 JSON 格式寫個簡單的 Response example 後按「Save」。

- 回到剛剛的 Request，按下「Send」後會發現 Response 的資料結構跟剛剛填寫的範例相同。

- **STEP 2**：新增其他的 Response example

- 除了最初的 Default 外，我們還可以`自己新增其他的 Response example`。

- 可以透過`加入 Params 讓這個 Request 有不同的 Response`，這裡我們新增「error」、「correct」這兩個 example 並加入「control_res」這個 Params 來做示範。

在「DESCRIPTION」填寫的資訊會顯示在 API 文件上，寫清楚一點不但方便自己記憶，也更容易讓合作者了解。

- 回到剛剛的 Request ，我們在「control_res」填入不同的 VALUE，按下「Send」就會看到對應的 Response 嚕～

### 2.4 設定 Environment 讓你快速切換執行環境

這是 Postman 重要的功能之一，它可以加快開發速度，讓你`不用為每個環境都建立一個 Request`。

- **如果沒設定會發生什麼蠢事？**

有沒有覺得上面這張圖片看起來很蠢，在了解 Environment 怎麼設定前，我的 Collections 就是這麼的複雜；`因為開發時通常環境會分成：「Local、Dev、QA、Prod」`，如果每新增一個環境你就複製一份，到最後你會無法進行妥善管理。

- **STEP 1**：新增 Environment
之前在建立 Mock Server 時其實就已經用到了 Environment，這裡我再詳細的跟大家說這個東西的好處。
請先到「Environments」的分頁按下「＋」的按鈕，接著輸入名稱來新增環境（我用 local_server 做示範）。

- **STEP 2**：設定常用的 Environment variables

- **url**
這個是最實用的環境變數，讓你只需要調整 Request 的 Environment 就快速切換網址做測試。

- **login_user、login_password**
每個環境的使用者登入資訊通常略有不同，且大部分的 API 都需要透過登入取得 token 才能使用，把他們加入環境變數可以加速你的測試。

**警告**：`不建議把正式機的 password 加入環境變數，且測試機的 password 不要用平常自己會用的密碼`，不然資安很危險的，都已經可以偷懶用 Environment，密碼自己去產生一組吧。

- **STEP 3**：在 Request 使用 Environment variables
想在 Request 中使用 Environment variables 只需要用 `{{your_env_var}}` 包起來就好，另外右上角記得要切換到正確的 Environment 才會帶入相應的資料喔！

### 2.5 匯出 API documentation 給同事使用指引

相信後端工程師有一個共同經驗，無論你的 API 文件寫得再清楚、功能執行的再穩定；到了同事手中卻常常被說不能 Work，緊張的你檢查很多遍也找不出問題，百思不得其解的抵達同事座位後，才發現他原來`漏了某個參數`。

如果你有這樣的經驗，Postman 內建的 API documentation 能拯救你，讓你在 Publish 文件後可以跟同事幸福快樂的生活。

- **STEP 1**：View documentation
選擇想要產生文檔的 Collection，按下「View documentation」後文檔就會顯示在右側，你可以透過切換右上角的環境變數來檢視文檔。

- **STEP 2**：Publish documentation

- 當你確定文檔內容都沒問題後按「Publish」。

- 接著會要你調整一些參數，調整完後建議先用左側的「Preview Documentation」自己確認看看是否符合。

- 確認一切都沒問題後按下「Publish Collection」就發布出去嚕～

- **STEP 4**：Share API Doc URL
你可以將 URL 複製起來分享給你的合作夥伴，如果你想把這份文檔下架，按「Unpublish」即可。

### 2.6 讓 Mock Server 變成私有的

前面在建立 Mock Server 時，不知道有沒有人注意到「`Make mock server private`」的選項，在做這個設定前 Mock Server 的 URL 都是公開的，如果你想為自己的 Mock Server 多一層防護可以跟著下面步驟操作：

- **STEP 1** : 編輯 Mock server

- 在「Mock Servers」分頁打開之前創建的 Server，按下「Edit」。

- 將「Make mock server private」打勾並按下「Save changes」。

- 回到之前的 Request，你會發現按下「Send」後會回傳錯誤訊息，這是因為現在 `Mock Server 都需要添加 API key 到 Request 的 Header 才能使用`。

- **STEP 2** : 產生 Postman API key

- 進去這個 [網址](https://go.postman.co/settings/me/api-keys) ，點擊「Generate API key」。

- 填上用來識別這個 API key 的名稱後按下「Generate API key」。

- 接著`請複製這個 API key`，上面已經警告你這是唯一見到它的機會了。

- **STEP 3**：將 API key 加入 Environment
因為日後 Mock Server 的所有 Request 都需要這個 API key，所以我們把他加入 Environment，變數名稱為「x-api-key」。

- **STEP 4**：在 Request 的 Header 加上 x-api-key
最後回到 Request，在 Headers 的分頁下加入「x-api-key」這個參數並將環境變數 `{{x-api-key}}` 作為值，最後按下「Send」就能看到正確的 Response 回傳嚕！

感謝大家的閱讀，如果喜歡我的文章可以`訂閱`接收通知；如果有幫助到你，按`Like`可以讓我更有寫文的動力，我們明天見～

### 參考資源

- [讓你的 Postman 更專業(筆者部落格)](https://medium.com/dean-lin/afc5c04742e0)

在許多人的幫助下，本系列文章已成功出版，除了添加新的篇章，更完善了每個案例的應對進退；如果對現在的職涯感到迷茫，也許這本書能帶給你不一樣的觀點～

**天瓏書局：** [https://www.tenlong.com.tw/products/9786263334571](https://www.tenlong.com.tw/products/9786263334571)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202109/fullstack-career-notes-day-15/
