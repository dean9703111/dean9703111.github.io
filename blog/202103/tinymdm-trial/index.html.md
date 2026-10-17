# 透過試用 TinyMDM 的服務來一窺公司網管能對你手上的『公司機』做什麼事

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202103/tinymdm-trial/
- 發布：2021-03-23
- 主題：技術教學
- 標籤：MDM、TinyMDM、Mobile Device Management、MDM 範例、MDM 教學
- 摘要：如果你不了解 MDM 可以做什麼，推薦你先看我的上一篇文章 — 『MDM 是什麼？被應用在哪寫地方？我的生活被 MDM 入侵了嗎？』，如果沒時間看就把 MDM 當成一個能對『公司機』於取予求的霸道總裁吧(誤)

如果你不了解 MDM 可以做什麼，推薦你先看我的上一篇文章 — 『**[MDM 是什麼？被應用在哪寫地方？我的生活被 MDM 入侵了嗎？](https://deanlin.net/blog/202103/what-is-mdm/)』**，如果沒時間看就把 MDM 當成一個能對『公司機』於取予求的霸道總裁吧(誤)

> 現在有很多間公司都有提供 MDM 的服務，筆者試用這間單純是因為他試用的時候不用綁信用卡，信箱電話公司地址全都是假的也沒問題～

```
一、前置作業
二、實作步驟
  STEP 1：註冊 TinyMDM 帳號，免費試用 30 天不需綁信用卡
  STEP 2：註冊 Company
  STEP 3：創建 Users
  STEP 4：創建並設定 Policies
  STEP 5：將過去沒在用的 Android 完全重置後註冊
三、確認給 Device 設定的 Policy 有生效
四、測試遠端控制功能
  ➤ 確認 device 能收到 admin 發送的訊息
  ➤ 遠端螢幕監控
  ➤ 行動裝置 APP 管理
五、體驗心得

```

## 一、前置作業

1. **準備 Android 版本 6.0 以上的手機**：因為其中一個步驟要重置手機(factory reset)，所以請用那些不再使用的測試機。
2. **Gmail**：一定要是 Gmail，因為其中一個動作是在 Google Play 上面註冊公司(可以亂寫)

## 二、實作步驟

### STEP 1：註冊 TinyMDM 帳號，免費試用 30 天不需綁信用卡

進入[官方首頁](https://www.tinymdm.net/)，點擊『Free trial』試用

建議你註冊一個專門用來測試的 Gmail 來做這個系統的體驗，點擊『Sign up with Google』

接下來會要求你填寫個人相關資訊，這些選項都是必填，但填不填真實的資訊看你自己

看到下面的訊息就代表註冊成功嚕～

第一次登入系統會提醒：『當你創建第一個 User 後就開始計算試用期』

### STEP 2：註冊 Company

MDM 是以公司管理內部行動裝置為概念出發的，所以要先註冊公司

點擊會跳轉到 Google Play 頁面

『公司名稱』自己創建一個吧！有沒有開公司的感覺 😊，後面需要填寫公司詳細聯絡資訊的部分**可以不填寫**。

看到這個頁面就代表你註冊成功嚕～

點擊『完成註冊程序』便會跳轉到 TinyMDM 的頁面

### STEP 3：創建 Users

新增 User 有兩條路：

- **Anonymous user**：User 的資訊可以隨意填寫
- **Send an email to the user（建議）**：需要額外填寫 email，當你完成 User 創建時會收到含有 QR code 的 email，機器可以透過掃描這個 QR code 來系統註冊。

新增後就會看到使用者的資訊嚕～

如果 User 有綁定 email 的話會收到內含 QR code 能讓 device 註冊的信

### STEP 4：創建並設定 Policies

行動裝置的操作權限是依據你設定的 Policies 所決定，所以要有 Policies 後才能夠新增 Devices

下面你所看到的就是 TinyMDM 所提供的服務，請記得幫 Policy 命名及增加描述方便區分

這裡我做幾個簡單的設定方便晚點的 Demo：

- **Users in policy**：設定遵照這個 Policy 的 Users

- **Internet Filtering**：這裡設定為『Whitelist』，只要拜訪的網址不在白名單都無法拜訪

- **Connectivity management**：我們將『藍芽傳送』以及『飛航模式』的功能關閉

### STEP 5：將過去沒在用的 Android 完全重置後註冊

先將手機完全重置（再次提醒要拿沒在用的手機喔！）

  **每支手機的設定方式不一樣，但大體上邏輯如上圖**

重置後在一開始的畫面於空白處狂點6次，接著會進入一個刷 QR code 的拍照畫面

  image from:https://www.tinymdm.net/help-resources/quick-start/

這個 QR code 你可以用收到的 email 或是在『Users and Groups』的『Show QR code』都可以

刷完後手機會讓你設定 WIFI 連線之類的東西，依照提示輸入按下一步即可

  image from:https://www.tinymdm.net/help-resources/quick-start/

完成上面的步驟後會請你核准 TinyMDM 這個 App 的各項條約與權限

  image from:https://www.tinymdm.net/help-resources/quick-start/

如果一切順利在『Devices』裡面就能看到新增的機器嚕～

## 三、確認給 Device 設定的 Policy 有生效

> 上面在 Policy 的地方設定了兩個最直觀的設定

- **Connectivity management**：我們我們可以看到手機的『藍芽』、『飛航模式』都被禁止

- **Internet Filtering**：因為沒有設定任何的白名單，所以你搜尋網站時所有網站都是無法打開的

## 四、測試遠端控制功能

### 確認 device 能收到 admin 發送的訊息

建議先用 message 的功能通知 device 確認傳訊息是沒問題的在進入下面的測試

### 遠端螢幕監控

admin 可以看到移動裝置即時的畫面

第一次使用時會需要手機打開相關的權限

每次觀看螢幕時還是需要詢問使用者是否接受（不過有員工敢拒絕嗎？）

### 行動裝置 APP 管理

管理者可以再不經過使用者的同意下遠端強制安裝 APP，我們在先在『Enterprise approved apps』裡面增加 APP

然後到『Policies』的『Apps management』中確認 App 存在

按下『Install』後就會發現移動裝置自動安裝了

## 五、體驗心得

- **功能面**：如果公司是電子元件加工廠、軍警消單位、金融機敏資料領域的話，我認為 MDM 的管理是必要的，因為這樣對於公司的資安是最安全的（至少阻擋了大部分資料外洩的可能），同時政令宣達、軟體更新也非常及時
- **介面 UI 設計**：功能分割的很清楚，但像是 APP 的一些安裝移除還是要摸過才會理解
- **功能穩定性**：老實說他的功能穩定性好像沒有到非常好，像是安裝/移除 APP 就沒有到非常的穩定(不過今天測試的時候都很穩)

```
以下是我整理 MDM 的相關資源，想更進一步了解的朋友可以參考看看：
👍 MDM 是什麼？被應用在哪寫地方？我的生活被 MDM 入侵了嗎？
👍 Zebra 如何提供管理大量 Android 裝置的產品 — 用逆向工程的角度去思考如何實現技術

```

參考文件：[TinyMDM 官方教學文件](https://deanlin.net/blog/202103/tinymdm-trial/#https://www.tinymdm.net/help-resources/quick-start/)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202103/tinymdm-trial/
