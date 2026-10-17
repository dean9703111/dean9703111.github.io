# 用 Midjourney Character Reference（cref） 解決「角色一致性」問題，讓同個角色可以放入不同場景！

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202404/midjourney-character-reference/
- 發布：2024-04-15（更新：2024-10-24）
- 主題：AI 實戰
- 標籤：Midjourney、Midjourney Cref、Character Reference、Cref、AI Image Generator
- 摘要：過去 Midjourney 有個痛點，就是用 AI 生成角色 IP 後，我們無法將他放到不同場景，因為每次生成的角色都長得不一樣。 但 推出「角色一致性」的功能後，你就可以在保留角色特徵的前提下，調整人物的動作、服裝、背景等元素。 也就是說，你可以自由的創造某個角色的故事。

過去 Midjourney 有個痛點，就是用 AI 生成角色 IP 後，我們無法將他放到不同場景，因為每次生成的角色都長得不一樣。

但 Midjourney 推出「角色一致性」的功能後，你就可以在保留角色特徵的前提下，調整人物的動作、服裝、背景等元素。

也就是說，你可以自由的創造某個角色的故事。

## 使用「`–cref`」來保持角色一致性

**Prompt 結構**：「`描述句 –cref 參考圖片的url`」

下圖為參考的圖片：

比如我希望將圖片調整成「主角拿著弓箭準備射擊」，Prompt：「`The protagonist is holding a bow and arrow and preparing to shoot , botw –cref 參考圖片的url `」這邊我指定了「botw（曠野之息）」的畫風，讓生成的結果更加一致。

從生成的結果來看，主角的五官、髮型、服裝、畫風都有維持一制性。

**備註**：如果你想要讓結果混合多個角色的特徵，可以使用這種 Prompt 結構：「`描述句 — cref 參考圖片的url1 參考圖片的url2`」

## 透過「`–cw`」調整參照角色的強度

**Prompt 結構**：「`描述句 –cref 參考圖片的 url –cw 10`」

`–cw` 的範圍在「0 ~ 100」，默認為 100。100 表示完全模仿角色特徵；而 0 則可僅保留角色的五官，通常用於更換角色的服裝、髮型。

下面用「主角坐在王座之上（The protagonist sits on the throne）」示範給大家看：

- cw 為 100 時，細節幾乎相同。

- cw 為 0 時，部分服裝依情節自動改變了。

## 建立四格漫畫

最後分享一個建立四格漫畫的 Prompt：「`imagine 4 panels with continuous`」

你可以透過這個 Prompt 創造屬於主角的故事，接下來就是讓自己想像力遨翔的時刻了。

延伸閱讀：[讓我們一起掌握 AI 帶來的紅利](https://dean-lin.medium.com/list/ddcc9c53a4ac)

**想將 AI 導入自己的職場與生活嗎？歡迎[購買筆者出版的新書](https://www.books.com.tw/products/0011004216)**，幫助您**在 AI 的浪潮中站穩腳步，並在未來的職場競爭中佔據優勢！**

  [https://www.books.com.tw/products/0011004216](https://www.books.com.tw/products/0011004216)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202404/midjourney-character-reference/
