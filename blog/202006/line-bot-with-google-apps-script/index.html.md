# 用Google App Script打造打造自己的lINE機器人

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202006/line-bot-with-google-apps-script/
- 發布：2020-06-10
- 主題：技術教學
- 標籤：Google App Script、LINE Bot、Google Sheets
- 摘要：來打造一個回聲狗吧～

> 為什麼撰寫這篇文章

Google App Script在以前我就非常的喜愛，也曾用他來當我專案後台登入的api來使用，搭配上Google Sheet，你簡直獲得了免費資料庫(不過他執行的速度就…反正就免費的速度XD)

但為了讓技術經過這段日子獲得更多提升，我想在原先資料庫的應用上加點料，比如說串接機器人，讓機器人對你定期提醒

本文章僅僅是示範最簡單的應用層面，實際上應該有非常多的應用等待大家去發掘

> 文章目標

1. 了解Google App Script，可撰寫簡單api並附帶參數
2. Google App Script串接Line機器人，讓你可以自由決定使用功能

> 本文章使用到的技術：

1. Google App Script : 撰寫你想要執行的邏輯
2. LINE developers : 打造自己的lINE機器人

## SETP 1: 建立 LINE Messaging API

我們要先在[這裡申請到line](https://developers.line.biz/zh-hant/)發送訊息所需要的token

除了Channel type要寫Messaging API外其他就自由發揮，反正你填錯或是漏填LINE會逼著你填完

新增完成後在Messaging Api裡面的basic id就是這個機器人在line的id

你就能在line裡面透過id找到他惹

除了id之外最重要的就是token，這個也是在Messaging Api裡面的Channel access token

另外我們不需要Greeting message、Auto response的功能

## STEP 2 : 建立 Google Apps Script 接收 LINE 訊息

你可以在[這裡開始你的Google App Script](https://www.google.com/script/start/)的故事 ，點擊Start Scripting，開啟一個Google App Script的專案吧

將以下程式複製貼上，如果您願意可以自由更改，記得將剛剛取得的line token放上去喔

```
function doPost (e) { //凡事使用post method皆要通過這個function

  var CHANNEL_ACCESS_TOKEN = '你的 Channel access token';
  var msg = JSON.parse(e.postData.contents);

  // 取出 replayToken 和發送的訊息文字
  var replyToken = msg.events[0].replyToken;
  var userMessage = msg.events[0].message.text;

  if (typeof replyToken === 'undefined') {
    return;
  }

  var url = 'https://api.line.me/v2/bot/message/reply';
  UrlFetchApp.fetch(url, {
    'headers': {
      'Content-Type': 'application/json; charset=UTF-8',
      'Authorization': 'Bearer ' + CHANNEL_ACCESS_TOKEN,
    },
    'method': 'post',
    'payload': JSON.stringify({
      'replyToken': replyToken,
      'messages': [{
        'type': 'text', // 格式為文字，line可以傳送多種個格式(carousel、image)，有興趣的可以自己研究
        'text': userMessage, //你目標傳遞的訊息
      }],
    }),
  });
}

```

原始碼：[code.gs](https://gist.github.com/dean9703111/7b90f26418375ed32aec1370a9673059)（GitHub Gist）

接下來要將這個程式發布（如果有授權相關問題必須都接受才會通過）

Project version記得每次都要選擇New(否則不會更新)，並且要允許任何人可以存取這隻app，不然你的程式無法被line機器人執行

接下來複製你這串web app URL給LINE Message Api使用

貼到webhook URL上面後按下Save

## STEP 3: 驗證串接成功

打開你的LINE就可以測試功能是否正常嚕～如果你是按照我的程式碼複製貼上，那他現在應該是回聲狗的功能XD

**今天的教學就到這裡了～串接Google Sheet的部分我會放到[下一篇文章](https://deanlin.net/blog/202006/line-bot-personal-assistant/)，歡迎有興趣的人來觀看喔！**

參考資源：[簡易後端實作 ( Google Apps Script )](https://www.oxxostudio.tw/articles/201805/backend-apps-script.html)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202006/line-bot-with-google-apps-script/
