# 【Midjourney 新手常見問題】圖片合成、排除物件、透明背景、以圖生圖

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202304/midjourney-faq/
- 發布：2023-04-12（更新：2024-10-24）
- 主題：AI 實戰
- 標籤：Midjourney、AI、Technology、Questions、Summary
- 摘要：在這篇文章中，筆者收錄了幾個最常被問的 Midjourney 操作問題（圖片合成、排除物件、透明背景、以圖生圖），希望能給剛入門的新手一些幫助與使用指引。

在這篇文章中，筆者收錄了幾個最常被問的 Midjourney 操作問題（圖片合成、排除物件、透明背景、以圖生圖），希望能給剛入門的新手一些幫助與使用指引。

## 圖片合成

**方法 1**：用「/imagine」將兩個網址用「 , 」區隔即可合成。

這邊我用維基百科上找到的「拿破崙」與「莫扎特」做合成。

  圖片來源：維基百科

合成後，網址會變轉換成短網址；同時我們可以獲得「莫扎特」與「拿破崙」的合體照（聽起來怪怪的XD）。

**方法 2**：用「/blend」方法，將圖片貼上合成（最多 5 張）。

這邊我選用燈泡跟兔子做為材料。

最終合成出一個兔子燈的成果。

## 排除物件

可以透過「 — no xxx」來排除物件。

- `–no people`：不要人。
- `–no chair`：不要椅子。

這邊我用「Interior of modern coffee cafe（現代咖啡館的內部）」作為 Prompt，會得到擺設含有椅子的咖啡館。

如果我希望擺設不含椅子，我就加上 `–no chair` 的參數，Prompt 改成「Interior of modern coffee cafe –no chair」。

從下圖我們可以看到咖啡廳就沒有椅子了。

## 白色背景、去背

我們可以用「in white background」來取得乾淨的背景。

這邊我先用「battle of two warriors（兩個戰士的戰鬥）」作為 Prompt，你會獲得不好去背的背景。

調整成「battle of two warriors `in white background`」作為 Prompt，你會獲得乾淨的背景。

這個時候透過去背服務的工具如：[https://www.remove.bg/](https://www.remove.bg/)

便可輕鬆去背。

## 以圖生圖

我們可以自己提供圖片，然後加上文字敘述改變圖片風格，Prompt 結構：「`jpg 或 png 結尾的 url` `描述`」，中間要記得空一格！

下面我就用筆者的照片作為範例，請他幫我用 Miyazaki Hayao（宮崎駿）的風格呈現。

從產出的結果來看，感覺瞬間 +40 歲 XD

下面改成用 botw（曠野之息）的風格，又瞬間 -10 歲惹 XD

延伸閱讀：[讓我們一起掌握 AI 帶來的紅利](https://dean-lin.medium.com/list/ddcc9c53a4ac)

**想將 AI 導入自己的職場與生活嗎？歡迎[購買筆者出版的新書](https://www.books.com.tw/products/0011004216)**，幫助您**在 AI 的浪潮中站穩腳步，並在未來的職場競爭中佔據優勢！**

  [https://www.books.com.tw/products/0011004216](https://www.books.com.tw/products/0011004216)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202304/midjourney-faq/
