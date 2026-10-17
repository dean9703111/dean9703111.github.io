# 使用 Visual Studio Live Share 來即時共同開發 — Pair programming

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202106/vscode-live-share-pair-programming/
- 發布：2021-06-21
- 主題：團隊合作
- 標籤：VS Code、VS Code Extension、Visual Studio Live Share、Pair Programming、Coworking
- 摘要：在遠端工作後，我們使用 Teams、Skype、Google meet…

在遠端工作後，我們使用 Teams、Skype、Google meet 來完成日常的會議，但如果使用上述工具來討論專案中的程式碼並不方便，因為所有使用者都聚焦在同一個畫面上；但看到問題時只能用口頭表達：「去app資料架下面找出xxxController，第30行的程式邏輯有問題。」這樣的表達方式欠缺效率且容易帶有情緒；所以今天向大家介紹一款共同開發超棒的工具。

  圖片來源：[Visual Studio官網](https://visualstudio.microsoft.com/zh-hant/services/live-share/?fbclid=IwAR3B2oFFd4kuofFKVm5qh7r5T-QCdOXSSta87b87jBhGGTdob4Ztl3Gp6-U)

```
一、我們現在協作時遇到了哪些困境？
二、這個工具能給我們開發帶來什麼樣的幫助？
三、安裝套件＆登入使用者
四、使用方式

```

## 一、我們現在協作時遇到了哪些困境？

- 使用螢幕分享做 Debug 時，需要依靠大腦來做運算邏輯，想要看哪一支檔案與程式也要靠口述，非常不方便
- 有些程式（Teams），不只螢幕分享來能直接控制對方的電腦做操作，但同一時間只允許一人做操作
- 就算團隊使用 git 版控，你切到對方有 Bug 的 branch 時可能無法順利重現問題發生（很多套件版本及環境參數設定不同）
- 因為無法完全模擬對方程式運作環境導致 Debug 時困難重重

## 二、這個工具能給我們開發帶來什麼樣的幫助？

- 對方能夠很清楚理解自己現在講述的程式在哪個檔案的哪個位置，不再雞同鴨講
- 雙方能自由瀏覽專案下的資料夾與程式，無需依靠口述
- 可以模擬對方的環境進行程式編譯，更清楚問題發生點
- 雙方的游標顏色不同，方便討論時不會產生誤會

## 三、安裝套件＆登入使用者

> 下面的步驟及操作可能會隨著 VScode 改版而有所不同，僅供參考

**SETP 1**：在 VScode 的 EXTENSION 分頁搜尋「Live Share」，點擊「install」即可完成安裝

**SETP 2**：安裝完後會在左下角多出「LIVE SHARE」的icon

**SETP 3**：第一次按「Share」時，會要求你登入 GitHub 或是 Microsoft 的帳號

**SETP 4**：這裡我選擇登入Github，基本上就是一路按接受授權的過程

**SETP 5**：註冊成功後，你就會看到以下畫面

## 四、使用方式

> 完成安裝＆登入的步驟後，下面提供本機模擬的操作方式

### 自己跟自己的連線，模擬操作

**SETP 1**：在本機用 VScode 開啟一個資料夾或是專案

**SETP 2**：點擊「Share」做分享，此時你會得到一組邀請連結

**SETP 3**：此時開用「New Windows」開啟一個空的 VScode，點擊「Join」加入

**SETP 4**：加入後加入後便可以開始共同開發嚕

當 Join 的人對檔案操作時，Share 的人可以看到他的操作

### 了解 Share 的人跟 Join 的人正在操作的畫面

按下右上角的「圖釘」，就是「Follow Participant」觀看對方現在操作的檔案

### 直接在對方的環境下執行程式了解問題

Join的人可以直接在終端機下指令，了解程式執行狀況

> 本篇文章只是很簡略地讓大家了解如何使用「Live Share」這款套件，更多的功能就讓大家[直接去官網](https://visualstudio.microsoft.com/zh-hant/services/live-share/?fbclid=IwAR3B2oFFd4kuofFKVm5qh7r5T-QCdOXSSta87b87jBhGGTdob4Ztl3Gp6-U)了解吧！

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202106/vscode-live-share-pair-programming/
