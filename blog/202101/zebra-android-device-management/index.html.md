# Zebra 如何提供管理大量 Android 裝置的產品 — 用逆向工程的角度去思考如何實現技術

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202101/zebra-android-device-management/
- 發布：2021-01-28（更新：2023-05-05）
- 主題：技術教學
- 標籤：Android Device Owner、管理大量裝置、NFC、Barcode、MDM
- 摘要：近年來無論是物流、警衛、超商店員，他們身上都配有一個平板電腦，用來協助進銷庫存盤點、巡邏地點定位；而許多知名的連鎖餐飲業者也將點餐的方式改為讓客人使用平板來選擇，這樣不但能呈現更多的菜色，也能減少許多營運上的人力成本。

近年來無論是物流、警衛、超商店員，他們身上都配有一個平板電腦，用來協助進銷庫存盤點、巡邏地點定位；而許多知名的連鎖餐飲業者也將點餐的方式改為讓客人使用平板來選擇，這樣不但能呈現更多的菜色，也能減少許多營運上的人力成本。

這些大公司使用到的平板少則數百台，多則數萬台；如何管理、設定這些設備就是一個很好的議題，這篇文章我會以 [Zebra](https://www.zebra.com/ap/en/products/software/mobile-computers/mobile-app-utilities/stagenow.html) 這間公司提出的解決方案用逆向工程的角度與大家分享他們是如何實現這個技術的。

```
文章會探討的議題及技術如下：
一、 企業在使用 MDM 的過程中遇到了什麼困擾？
二、 Zebra 公司的解決方案為何？
三、 使 App 變成 Device Owner
四、 有哪些方式可以變成 Device Owner 呢？
五、 結語

```

## 一、 企業在使用 MDM 的過程中遇到了什麼困擾？

- **人力**：需要分配或是僱傭員工去幫平板做安裝及連線設定，有些操作甚至需要經過教育訓練才能完成。
- **時間成本**：依據每個人的熟悉度不同，不但操作的速有差異外，遇到特殊例外的處理能力也有所不同，這在無形中浪費了很多時間。
- **失誤率**：只要是由人所操作的一定會有失誤率，而這些人為所造成的失誤在未來會影響到終端客戶的使用觀感以及公司營收上面；如果事情嚴重可能會有機台退換貨的狀況發生。

## 二、Zebra 公司的解決方案為何？

- **產品功能**：公司提供一個簡單的管理工具，讓管理者操作簡單的介面就可以將這些機器設定成你希望的配置；目前看說明書，他們可以讓使用者設定以下內容：Wi-fi 網路、語言及輸入法、安裝證書、業務用App、作業系統。
- **Android 裝置註冊**：這項產品主打的功能就是讓管理者擁有機器完整的權限，為了達到這個目標他們需要讓**機器進行註冊**，註冊的方式是掃描 barcode 或是 NFC 標籤，這個動作除了讓機器完成註冊外同時也讓他們安裝了一個不可刪除讓管理者達成遠端設置目標的 App。

## 三、 使 App 變成 Device Owner

接下來要思考如何讓 Android 裝置掃描 barcode 或是 NFC 標籤就能獲取完成存取權，原本以為要透過很多複雜的手續，結果一 Google 發現這是 Android 所提供的一個 api。

看參考資料得知，這是在 Android 5.1 之後才擁有的功能，官方名稱為**Provisioning methods**，具體有哪些方法在下一段會詳細說明；基本的概念就是在使用了這個方法後裝置上會安裝一個 Device Owner 的 App，這個 App 有點類似「大管家」，基本上什麼事都能幹。而且成為 Device Owner 的 App 不能被卸載，不能被禁用。

## 四、 有哪些方式可以變成 Device Owner 呢？

根據 [android 官方文件](https://developers.google.com/android/management/provision-device#provisioning_methods)所說，有以下方式可以使用：

- **Sign-in URL**：我猜測 Zebra 就是在掃描 barcode 的時候會跑出一個已經設定好給某個企業使用的登入連結來做綁定
- **QR code**：要 Android 7.0+ 以上才能使用，基本上掃描 QR code 做好註冊就可以了。
- **NFC**：要 Android 6.0+才能使用，基本上製作好 NFC 標籤碰一下就能完成註冊。
- **DPC identifier**：看官方文件說這個方法是在沒辦法透過 QR code、NFC 方式註冊時才使用，需要 IT 人員依照步驟操作，看起來並不是一個好方法。
- **Zero-touch enrollment**：要Android 8.0+ (Pixel 7.1+)才能使用，顧名思義，就是機器購買時就已經安裝好這個 App 了。

## 五、 結語

在看完 Google 官方的文件後就能理解為什麼 Zebra 這間公司為什麼提供這兩種方式了，因為掃描 barcode 這個功能基本上只要是 Android 手機都能夠辦得到，而如果有 NFC 裝置的手機則可以加快機器的註冊效率(畢竟靠近就好了)

以上是我透過分析使用者需求、Zebra 提供的產品、Google 官方文件三者結合的文章，分析產品功能反推使用的技術也是一件挺有趣的事情(雖然實際的技術未必跟我想的一樣)，希望看到這篇文章的觀眾有成功增加一些冷知識XD

```
以下是我整理 MDM 的相關資源，想更進一步了解的朋友可以參考看看：
👍 MDM 是什麼？被應用在哪寫地方？我的生活被 MDM 入侵了嗎？
👍 透過試用 TinyMDM 的服務來一窺公司網管能對你手上的『公司機』做什麼事

```

參考資源：[如何使App變成Device Owner](https://kknews.cc/zh-tw/tech/2k42gqe.html)、[anrdroid官方文件](https://developers.google.com/android/management/provision-device#provisioning_methods)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202101/zebra-android-device-management/
