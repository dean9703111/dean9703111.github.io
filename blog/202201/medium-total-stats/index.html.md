# 3步驟取得自己Medium所有文章的總瀏覽/閱讀/粉絲人數

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202201/medium-total-stats/
- 發布：2022-01-03
- 主題：技術教學
- 標籤：Medium Views、總瀏覽數、Stats、Statistics、Total Views
- 摘要：我想大部分 Medium 平台的創作者，都應該非常關心自己文章的瀏覽數量；但 Medium 提給供創作者的後台並沒有統計出「總數」這塊的資訊，如果要自己土法煉鋼用計算機加總也會讓人崩潰，今天筆者就帶你們用聰明且快速的方法來取得這份資訊吧！

我想大部分 Medium 平台的創作者，都應該非常關心自己文章的瀏覽數量；但 Medium 提給供創作者的後台並沒有統計出「總數」這塊的資訊，如果要自己土法煉鋼用計算機加總也會讓人崩潰，今天筆者就帶你們用聰明且快速的方法來取得這份資訊吧！

  上面的是[ DL 技術世界](https://medium.com/dean-lin) 這個 Publications 的數據喔～

> 建議使用 Chrome 或是 Firefox 來操作下面的步驟

## SETP 1/3：進入自己 Medium STATS 頁面

今天提供的方法可以計算自己 STATS 的頁面([https://medium.com/me/stats](https://medium.com/me/stats))，以及自己有 Owner 權限的 Publications 頁面。

  自己 STATS 的頁面

  有 Owner 權限的 Publications 頁面

## SETP 2/3：打開瀏覽器的「開發者工具(developer tools)」

可以按鍵盤的「F12」或是「滑鼠右鍵 → 檢查」打開瀏覽器的「開發者工具(developer tools)」。

  部落格有很多優質好文(自己在說XD)，歡迎大點閱＆分享＆拍手喔～

開啟後你應該會看到下面的畫面，不要被右邊的程式碼還有下方一堆紅色的警告給嚇到。

## SETP 3/3：貼上程式碼，取得自己 Medium 部落格的總瀏覽/閱讀/粉絲數量！

你要做的事情很簡單，先將下面的程式碼複製起來。

```
// 要抓取第幾個欄位的資訊
const totalTypes = {
    VIEWS: 2,
    READS: 3,
    FANS: 5,
};
// 根據網頁結構取得資訊，並且加總
const getTotal = (tableColumn) => [
    ...document.querySelectorAll(
        `td:nth-child(${tableColumn}) > span.sortableTable-number`
    )].map((e) =>
        parseInt(e.getAttribute("title").replace(/,/g, ""))).reduce((a, b) => a + b, 0);
// 印出各自的總瀏覽數
console.log({
    totalViews: getTotal(totalTypes.VIEWS),// 總瀏覽數
    totalReads: getTotal(totalTypes.READS),// 總閱讀數
    totalFans: getTotal(totalTypes.FANS),// 總粉絲數
});

```

原始碼：[get-medium-total.js](https://gist.github.com/dean9703111/a57fbb67cb29eba87263fedfca7c6378)（GitHub Gist）

然後貼到剛剛瀏覽器的「Console」位置。

接著按下「Enter」就可以在下方看到經過統計的數字嚕！

## ❗特別提醒：如果你的文章很多，請滑至頁面底部再操作

因為 Medium 預設一開始進入頁面只會顯示一部分的文章數據，因此如果你的文章數量很多，**請滑至頁面底部再貼上程式碼**；以筆者的文章為例，若沒有往下滑，得到的數據為：「totalViews: 6140, totalReads: 2905, totalFans: 21」，筆者至今共發表 74 篇文章，儘管非常佛系經營；但如果只有上面的數據真的會痛哭流涕😭😭😭。

我邊滑到頁面底部再做一次操作，透過下圖的數據:：「totalViews: 20597, totalReads: 8871, totalFans: 55」，我們就更得知如果沒滑至底部計算是不準確的喔！

  這個數據還是好可憐ＱＱ，請讀者多多拍手跟分享；讓筆者寫文章的時候能更有溫暖🙏

> 希望今天這篇文章有幫助到 Medium 的創作者取得相關數據，讓自己更有競爭力 💪

參考資源：[How to get total views and reads statistics on Medium](https://www.kevinpeters.net/how-to-get-total-views-and-reads-statistics-on-medium)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202201/medium-total-stats/
