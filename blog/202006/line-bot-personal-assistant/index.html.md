# 讓Line機器人變成你的小秘書

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202006/line-bot-personal-assistant/
- 發布：2020-06-10（更新：2023-05-05）
- 主題：技術教學
- 標籤：Google App Script、Google Sheets、Google Sheets API、LINE Bot
- 摘要：機器人的秘書還不夠誘惑嗎ＸＤ

為什麼撰寫這篇文章

在[上一篇](https://deanlin.net/blog/202006/line-bot-with-google-apps-script/)文章已經介紹了Google App Script如何串接Line Message Api的應用，在這裡我將延續之前的議題，讓Line機器人變成你的小秘書

文章目標

- 使用Google Sheet裡面Script的功能
- 使用簡單的SQL語法抓出Google Sheet中你需要的資訊

本文章使用到的技術：

- Google App Script : 這次著重在執行SQL語法
- Google Sheet : 會把它當成讀取/寫入的資料庫

## STEP 1: 結合Google Sheet的衍生應用

Google App Script功能很多，但能做到什麼地步就看個人的想像能力，接下來我提供的只是一條思路，如果大家想到有趣的應用歡迎下方留言

如果大家平常有使用Google sheet當成工作記錄習慣的話，你可以嘗試看看這個應用

這裡我先創建一個Google Sheet，裡面建立一些清單([你可以直接複製我的](https://docs.google.com/spreadsheets/d/1ZdmsEkS27VWR6HF42_aka5rrtgu31saZoUupOD8Xx6s/edit?usp=sharing))：

接著建立Google Sheet的Script

將以下程式複製貼上，如果您願意可以自由更改，記得將[上一篇](https://deanlin.net/blog/202006/line-bot-with-google-apps-script/)取得的line token放上去喔

```
var ss = SpreadsheetApp.getActiveSpreadsheet()//抓取這張excel
function doPost (e) {
    var msg = JSON.parse(e.postData.contents);
    // 取出 replayToken 和發送的訊息文字
    var replyToken = msg.events[0].replyToken;
    var userMessage = msg.events[0].message.text;

    if (userMessage.indexOf("未完成") > -1) {//分析傳遞字串確認是否有“未完成”
        // QUERY(目標文件及範圍,SQL語法)
        // 尋找sheet1範圍A2到C100裡面A跟B欄位的資料且C欄位為FALSE
        var formulaToUse = '=QUERY(Sheet1!A2:C100,"SELECT A,B WHERE C = FALSE")'
        var result = query(formulaToUse)

        var userMessage = '未完成文件如下：\n';//單純的文字組成
        result.forEach(row => {
            userMessage = userMessage + row[0] + ':' + row[1] + '\n'
        })
        return sendLineMsg(userMessage, replyToken);
    }
    if (userMessage.indexOf("已完成") > -1) {//分析傳遞字串確認是否有“已完成”
        var formulaToUse = '=QUERY(Sheet1!A2:C100,"SELECT A,B WHERE C = TRUE")'
        var result = query(formulaToUse)
        var userMessage = '已完成文件如下：\n';
        result.forEach(row => {
            userMessage = userMessage + row[0] + ':' + row[1] + '\n'
        })
        return sendLineMsg(userMessage, replyToken);
    }
    if (userMessage.indexOf("功能") > -1) {//這是列出你有提供功能的列表
        return sendLineMsg('朕不給的你不能搶ㄎㄎ\n• 未完成 ：列出excel未完成清單\n• 已完成 ：列出excel已完成清單', replyToken);
    }
}
function query (request) { //執行SQL語法的地方
    var sheet = ss.insertSheet(); //先插入一張新的sheet(也許有更好的做法，如果有請告訴我ＱＱ)
    sheet.getActiveRange().setFormula(request); //執行SQL
    var result = sheet.getDataRange().getValues(); //把整張表抓下來
    ss.deleteSheet(sheet); //把sheet刪除
    Logger.log(result);//可透過log來看輸出的資訊(上方的View Logs)
    return result; //回傳結果
}
function sendLineMsg (Message, replyToken) {
    var CHANNEL_ACCESS_TOKEN = '你的 Channel access token';

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
                'type': 'text',
                'text': Message,
            }],
        }),
    });
}

```

原始碼：[line_secretary.gs](https://gist.github.com/dean9703111/8f6d1e06b079ae3061d450d459cedc2c)（GitHub Gist）

接下來要將這個程式發布（如果有授權相關問題必須都接受才會通過）

Project version記得每次都要選擇New(否則不會更新)，並且要允許任何人可以存取這隻app，不然你的程式無法被line機器人執行

接下來複製你這串web app URL給LINE Message Api使用

貼到webhook URL上面後按下Save

## STEP 2 : 驗證串接成功

打開你的LINE就可以輸入下面三種文字，機器人就會做相對應的回覆喔！

- 功能
- 未完成
- 已完成

希望大家喜歡這篇文章的應用～

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202006/line-bot-personal-assistant/
