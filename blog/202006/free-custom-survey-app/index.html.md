# 免費打造專屬於您的問卷程式

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202006/free-custom-survey-app/
- 發布：2020-06-03（更新：2023-05-05）
- 主題：技術教學
- 標籤：Google Forms、Google App Script、LINE Notify
- 摘要：Google Form & Google App Script & LINE Notity

## 免費打造結合即時通知&店家推廣的問卷程式

**也許您曾經有這樣的經驗：**

- 顧客填寫完問卷後留下負評但店家未即時處置
- 顧客用餐體驗極佳但您卻沒讓他更近一步地幫店裡宣傳

文章目標：

- 客戶留下負面評價時會即時向店長(老闆)的line發出通知，讓店長(老闆)可以馬上了解狀況並處理
- 客戶留下好的用餐評價會引導他幫店家粉絲團按讚留下好評

**本文章使用到的技術：**

- Google Form表單
- Google App Script
- LINE Notify

接下來將一步一步解說如何從零打造您的問卷程式

## STEP 1 / 4 : 建立Google Form表單

先建立一個[Google Form](https://www.google.com.tw/intl/zh-TW/forms/about/)，選取個人的即可：

我將表單拆成兩個section如下圖，您可以設定自己想要的問題，不過我會建議您先跟我做一樣的步驟：

Section1是店家評分的設定，我們會根據評分不同給予不同的導向

Section2是當客戶不滿意時可以讓他加註的部分

## STEP 2 / 4: 撰寫Google App Script

完成上面Form的設定後就可以幫他加上Google App Script

將以下程式複製貼上，如果您願意可以自由更改，如果看不懂可在下方詢問，LINE Notify 的token會在Step3講解，您取得後再補上去即可

```
function getFormResponse (event) { //event為觸發submit事件時帶入的資訊

  //表單名稱(請填寫對應的名稱，否則google會無法找出對應的form id)
  var FORMNAME = "餐後回饋表單";

  //取得表單
  var formHandle = DriveApp.getFilesByName(FORMNAME).next();

  //由表單 Id 開啟表單
  var form = FormApp.openById(formHandle.getId());

  //取得表單名稱
  var formTitle = form.getTitle();

  //取得表單回覆內容
  var formResponse = event.response;

  //取得表單上的項目
  var itemResponses = formResponse.getItemResponses();

  //傳送訊息設定
  var itemContext = "\n" + formTitle + "\n 電子郵件 : " + Session.getEffectiveUser().getEmail() + "\n";

  //紀錄回覆評分
  var scrore = 0

  //將表單每一項問答組成訊息
  for (var j = 0; j < itemResponses.length; j++) {
    //取得問題標題
    itemContext += itemResponses[j].getItem().getTitle();

    //取得問題回覆內容
    itemContext += " : " + itemResponses[j].getResponse() + "\n";

    //用這裡來取得評分
    if (itemResponses[j].getItem().getTitle() == '店家評分(5分最高)') {
      scrore = itemResponses[j].getResponse()
    }
  }

  //取得填寫表單時間
  itemContext += "\n填寫時間 : " + formResponse.getTimestamp() + "\n\n\n";

  if (Number(scrore) <= 2) {//如果訂單評分低於2分則馬上送出表單到Line讓店家儘速回覆
    sendToLine(itemContext);
    form.setConfirmationMessage('非常抱歉造成您用餐的不愉快，我們店長將儘快與您取得聯繫並致上最誠懇的道歉');
  }
  else if (Number(scrore) >= 4){//如果訂單評分高於4分則馬上送出店家粉絲團爭取按讚機會
    form.setConfirmationMessage('如果本次的用餐讓您感到愉悅，請您給粉絲團一個讚:' + "\n" + 'https://www.facebook.com/profile.php?id=100001123361906');
  }

}

function sendToLine (message) {

  var token = "你自己的LINE Notify";

  var options =
  {
    method: "post",
    payload: "message=" + message,
    headers: { "Authorization": "Bearer " + token },
    muteHttpExceptions: true
  };

  UrlFetchApp.fetch("https://notify-api.line.me/api/notify", options);
}

```

原始碼：[google_reponse](https://gist.github.com/dean9703111/6cc1f1f7ac5526b27b5e2963fc3b125a)（GitHub Gist）

接下來要將這個程式發布（如果有授權相關問題必須都接受才會通過）

Project version記得每次都要選擇New(否則不會更新)

因為我們要觸發script裡面的function所以需要設定觸發的時機點如下：

如果新增成功就會看到下圖

## STEP 3 / 4: 申請LINE Notify token

因為我們表單在判斷評價低分時要向老闆(主管)的LINE推播，所以需要[申請一組LINE Notify的token](https://notify-bot.line.me/zh_TW/)，你可以把它想像成一個允許對你LINE發送訊息的鑰匙

  如果有曾經申請過token的就會在上方顯示

接下來只需要將您申請的這組token貼到Google App Script裡面token的位置就好嚕～記得要重新publish App script的版本喔

## STEP 4/ 4: 展示成果

只要您按造上面的教學文件，如果順理的話應該會得到下面的畫面喔

如果評分低於2分：

如果評分高於4分

## 人生第一篇部落格教學到這裡結束啦～希望大家都有透過這些工具的組合蹦出新的火花喔～

參考資源：[Google App Script (From Service)](https://developers.google.com/apps-script/reference/forms) , [Google 表單與 Line Notify 結合應用](http://white5168.blogspot.com/2018/04/google-apps-script-19-google-line-notify_24.html#.Xtcm-lMzbzI)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202006/free-custom-survey-app/
