# 免費！提交 Google 表單後，系統自動寄送客製化 email，宣傳效果 Up！

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202208/google-forms-auto-email/
- 發布：2022-08-10（更新：2023-05-05）
- 主題：技術教學
- 標籤：Google Forms、Google Sheets、Apps Script、Email Marketing、Email Notifications
- 摘要：有時我們希望使用者提交 Google 表單後，可以提供給他更多資訊（ex：活動詳細內容、宣傳廣告、友站連結…）；但 Google 表單只有在「設定 → 簡報 → 確認訊息」給你用純文字的方式表達，除了傳送內容受限制外，許多使用者在提交表單後會下意識的關閉網頁…

有時我們希望使用者提交 Google 表單後，可以提供給他更多資訊（ex：活動詳細內容、宣傳廣告、友站連結…）；但 Google 表單只有在「設定 → 簡報 → 確認訊息」給你用純文字的方式表達，除了傳送內容受限制外，許多使用者在提交表單後會下意識的關閉網頁，根本沒注意到裡面寫了些什麼。

## 前言：需求來自於實務

其實會想做這個功能，是因為不久前[出版書籍](https://www.tenlong.com.tw/products/9786263331525)後，陸續有人填寫 Google 表單回傳自己的心得。

而我這次有給填寫表單的讀者「30 分鐘免費諮詢」的回饋，但因為把諮詢的報名連結放在 Google 表單「確認訊息」的位置，導致有些讀者根本沒注意到它的存在，因此便想透過自動寄送 email，來解決這個囧境。

你也可以先[體驗看看筆者完成的 Demo](https://forms.gle/ADvarze9bKrwyUGV6)，只要填寫電子郵件，並在「是否願意收到電子報」的欄位選擇「是」，提交後便會收到自動發送的郵件嚕！

## STEP 1 ：產生 HTML email

其實這個步驟並非必需，但為了讓寄送的 email 有基礎的質感，筆者還是決定製作一個 HTML email。

看到 HTML 先別慌，現在網路上有非常多的資源，只要拖拉點選，一行程式碼都不用寫就能產生出來，這邊筆者是使用 [Beefree](https://beefree.io/) 這個網站提供的服務，在這裡有擁有豐富的版型供你選擇（雖然筆者還是選用空白模板）。

它的使用非常方便，你甚至可以在不註冊的狀態下製作自己的 HTML email；**不過如果你擔心自己會失手關閉網頁，還是建議註冊登入。**

  這個是 Beefree 的 UI 操作畫面

在完成 HTML email 後，點擊右上角的 Download 會獲得一個壓縮檔，解壓縮後你會看到裡面的「beefree-xxx.html」檔，筆者建議重新命名成「email-content.html」方便辨識（這個 HTML 在後面的步驟會用到，請妥善保存）。

**備註：**

1. 免費版在最下面會有 Beefree 的 Logo，它可以在下載後透過編輯 HTML 來刪除。

2. 如果有使用到圖片，建議先上傳到 Imgur，再把產生連結貼回來（ex：https://i.imgur.com/Oonq5Gy.jpg），詳細流程這邊不再贅述。

## SETP 2：建立 Google 表單

建立表單的步驟我就不再示範了，需要注意的是「電子郵件（email）」這個欄位需要設定為必填，不然你也沒有寄信的對象啊！

為了驗證 Google 表單的參數可以作為邏輯判斷使用，我這邊再新增一個「是否願意收到電子報」的問題，依照回答的「是/否」來決定要不要寄送 email。

## SETP 3：從 Google 表單產生 Google 試算表

在「回覆」的 Tab 中，點擊「Google 試算表」的 icon 便會跳出建立 Google 試算表的詢問視窗。

下面是自動產生的 Google 試算表。

## STEP 4：從 Google 試算表建立 Apps Script

點擊工具列的「擴充功能」，再點擊「Apps Script」便會產生嚕！

在 Apps Script 裡面用「新增檔案」建立一個 HTML，並將 STEP 1 產生的 HTML 程式碼貼回去。

接著要撰寫 Apps Script 裡面的程式邏輯（程式碼.gs）。

我們要先抓取 Google Form 裡面提交的值，如果「是否願意收到電子報」的值為「否」，我們就跳出函式；若為「是」，我們便將 HTML email 寄出（相關注意事項請看備註描述）。

```
function onSubmit(datas) {

  // 取得提交的值（你的欄位名稱可能與我不同）
  var data = datas.namedValues;
  var replyEmail = data['電子郵件地址'][0];
  var isReceiveEDM = data['是否願意收到電子報'][0];

  if (isReceiveEDM === '否'){
    // 如果拒絕收到電子報就直接結束
    return;
  }

  // 取得上傳的 HTML 檔
  var edmHTML = HtmlService.createTemplateFromFile('email-content');

  // 設定HTML裡的變數
  edmHTML.data = data;

  // 產出成要發送的 HTML
  var output = edmHTML.evaluate().getContent();

  // 發送信件
  MailApp.sendEmail({
    to: replyEmail,
    subject: "[系統發送] 感謝您填寫「Dean Lin 的回饋表單」",
    htmlBody: output
  });
}

```

原始碼：[email-response.gs](https://gist.github.com/dean9703111/36e9cb9e237c53936ed751d5fb986fae)（GitHub Gist）

## STEP 5：設定觸發條件

在上面的步驟完成後，我們要將 Google 表單的事件與剛剛撰寫的程式綁定；因此我們要到左側的功能列，點擊「觸發條件」。

接著點擊右下角的「新增觸發條件」。

觸發條件可以參考筆者在紅框的選擇，完成後按右下角的「儲存即可」。

接下來 Google 會跳出一堆需要你授權的彈窗，順序大概是「選擇帳戶 → 驗證 app → 允許 app 權限」。

## Final：測試發信

終於走到最後一步惹 🤣

我們來[填寫 Google 表單](https://forms.gle/ADvarze9bKrwyUGV6)，確認是否會順利自動寄信。

  如果選擇「否」就不會收到郵件喔！

如果收到 email，就大功告成嚕！

教學到這邊就結束啦～希望大家按照步驟都能順利完成，提供更多的資訊給目標 TA，讓宣傳效果 Up Up！

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202208/google-forms-auto-email/
