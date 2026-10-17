# 原來 Midjourney 的 Settings 對產圖有這麼大的影響！？

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202301/midjourney-settings/
- 發布：2023-01-21
- 主題：AI 實戰
- 標籤：Midjourney、Settings、Midjourney Settings、AI、AI Painting
- 摘要：這篇文章筆者將會舉出實際案例，讓你知道 Midjourney 的 Settings 裡面有哪些參數，以及他們會對產出的圖造成什麼影響。

這篇文章筆者將會舉出實際案例，讓你知道 Midjourney 的 Settings 裡面有哪些參數，以及他們會對產出的圖造成什麼影響。

> 尚未接觸過 Midjourney 的朋友，可以先閱讀上一篇文章：「[如何使用 Midjourney，讓 AI 產生絕美圖片](https://deanlin.net/blog/202301/midjourney-getting-started/)」

```
大綱

 呼叫出 Midjourney 的 Settings
 MJ version 1、2、3、4、Niji Mode … 版本間的差異
 Half quality、Base quality、High quality 的差異
 Style low、med、high 的選擇
 Public mode、Private mode 的差異
 Fast mode、Relax mode 的差異
 了解目前帳號剩下的產圖額度
 調整圖片產出的比例

```

## 呼叫出 Midjourney 的 Settings

在下方的訊息輸入「/settings」，就會顯示我們可以調整的參數，以及現在的設定；在設定完成後，接下來產生的圖片都會比照先前的設定。

> 如果你看上去覺得眼花撩亂實屬正常，後續將說明幾個會影響產圖的設定。

## MJ version 1、2、3、4、Niji Mode … 版本間的差異

Midjourney 發展至今有推出多個版本，每個版本都有各自的風格；筆者實驗下來，前面的版本相對抽象（激發你藝術創作的靈感？），而後面的版本更寫實逼真。

  MJ version 1 產出的男孩，人直接被扭曲抽象了

  反觀 MJ version 4 產出的男孩，明明是一樣的 prompt 卻變得如此帥氣

  使用 Niji Mode 會創作出二次元的風格

## Half quality、Base quality、High quality 的差異

這會影響你獲得圖片的品質、渲染的時間、GPU 的成本，筆者實驗下來，覺得選擇默認的 Base quality 就好（High quality 會更快消耗你產圖的額度）。

## Style low、med、high 的選擇

Style low 所產出的圖片風格相對一致，而 Style high 的變化度更高；你可以根據自己的創作需求來調整。

下面筆者以「elegant lady」這個 prompt 為範例，你應該可以明顯感受到 Style high 的風格更加多元。

  Style low 中的 elegant lady 風格相似

  在 Style high 中，同樣是 elegant lady 卻是完全不同的風格

## Public mode、Private mode 的差異

在 Public mode 所產生的圖都是公開的，而 Private mode 的圖則是只有你自己才看得到的。

> 不過選擇 Private mode 會需要額外付費，如果你真的在創造一些超秘密的圖片，就可以考慮一下。

## Fast mode、Relax mode 的差異

Fast 就是不用排隊，下完 prompt 後就會開始產圖；而 Relax 則需要排隊，具體速度竟看當前伺服器的排隊狀況。

## 了解目前帳號剩下的產圖額度

在下方的訊息輸入「/info」會顯示帳號資訊：

- **Fast Time Remaining**：快速模式剩餘的流量。
- **Lifetime、Relaxed Usage**：不同模式下各自算了幾張圖。

  左邊是「體驗版」右邊是「付費版」

## 調整圖片產出的比例

預設產出正方形的圖片，如果想改成長方形，你可以在結尾增加「 — ar 3:2」或「 — ar 2:3」。

> 目前 MJ version 4支援 2:1 以內的長寬比例，常用的比例有： 「16:9、9:16、 2:1、1:2、3:2、2:3」

這邊就舉一個有多個 suffix（後綴詞）的範例吧：「In the dark wine cellar, a ring imbued with magic is placed on top of an ancient and thick book, with a beam of moonlight shining upon it. — ar 3:2 — s 750 — q 0.5」

- **ar**：圖片的比例為 3:2
- **s**：style very high
- **q**：Half quality

  在陰暗的酒窖裡，一枚蘊含魔力的戒指被放在古老又厚重的書本上，一抹月光剛好照在上面。

筆者對右下的圖片最為滿意，就讓他來擔任這篇文章的封面圖吧（雖然跟標題關聯性不大XD）

> 對於 Midjourney 的各項設定，筆者也還在持續實驗與學習中，如果有任何錯誤，還請讀者不吝指教，讓彼此共同成長。

未來筆者會出一系列使用 ChatGPT、openAI 的文章，大家敬請期待！

延伸閱讀：[讓我們一起掌握 AI 帶來的紅利](https://dean-lin.medium.com/list/ddcc9c53a4ac)

**想將 AI 導入自己的職場與生活嗎？歡迎[購買筆者出版的新書](https://www.books.com.tw/products/0011004216)**，幫助您**在 AI 的浪潮中站穩腳步，並在未來的職場競爭中佔據優勢！**

  [https://www.books.com.tw/products/0011004216](https://www.books.com.tw/products/0011004216)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202301/midjourney-settings/
