# [AIVA 教學]不可思議！ AI 竟能模仿其他音樂進行創作 — Influences 功能介紹

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202307/aiva-influences/
- 發布：2023-07-23
- 主題：AI 實戰
- 系列：AIVA AI 作曲
- 標籤：AIVA、Music、AI、AI Music Generator、AIGC
- 摘要：只要提供一首歌，AIVA 就能解析這首歌的樂器、調性、節奏、節拍、時長等資訊，然後基於上傳的音樂，創作出風格類似但聽起來全新的作品。 是不是感覺超級猛？就讓我們透過這篇文章來體驗看看吧！

只要提供一首歌，AIVA 就能解析這首歌的樂器、調性、節奏、節拍、時長等資訊，然後基於上傳的音樂，創作出風格類似但聽起來全新的作品。

是不是感覺超級猛？就讓我們透過這篇文章來體驗看看吧！

> 如果你是第一次接觸 AIVA，可以先看[基礎介紹](https://deanlin.net/blog/202307/aiva-compose-by-style/)、[參數認識](https://deanlin.net/blog/202307/aiva-compose-with-chatgpt/)，對他的功能有初步的認知。

## STEP 1：上傳一首自己喜歡的音樂

這邊我就選一首經典神曲「Two Steps From Hell」

[YouTube 影片：Two Steps From Hell - Victory](https://www.youtube.com/watch?v=hKRUPYrAQoE)

> 使用時請注意版權，這首音樂僅能在非商業用途上使用。

透過黑魔法變成 MP3 後，將它上傳到 AIVA 的 Influences。

在這個畫面將音檔拖拉進來就好了，特別提醒如果音樂長度不足一分鐘會被拒絕喔！

將音檔拖拉進去後，他會詢問你是否知道這個音樂是什麼調性；儘管 AIVA 可以自動偵測，但如果你能提供正確的調性，可以幫助他做出更精準的分析（使用 Auto 其實就可以了）。

如果順利就會顯示下方的畫面：

然後等待上傳完畢：

> 這邊提醒一下讀者，如果上傳的音檔超過 5 分半，他會要求你裁剪使音檔使其低於 5 分半（可直接於介面操作）。

上傳完畢後，讓我們看看 AIVA 對音樂分析的能力如何：

- **INSTRUMENTATION**：Epic Orchestra（史詩管弦樂團）
- **KEY**：D Minor
- **TEMPO**：60
- **METER**： 4/4

## STEP 2：使用 Influences 來模仿音樂創作

接下來我們以這個音樂為基底進行創作，點擊右鍵，選擇「New Composition」。

接著會跳出參數設定的畫面：

如果沒有什麼作曲想法，可以點擊「Create tracks」全權交給 AIVA 自由發揮。

不過筆者個人實驗下來，在 **ENSEMBLE（合奏）**的部分選擇有「Recommended」前綴的，通常得到的結果比較好。

## STEP 3：聽聽看 Influences 創作的結果跟原曲差異有多大？

這邊筆者直接產生 5 首音樂

但實際聽起來，只有兩首稍微接近原曲想表達的風格，其他基本撲街，有興趣的讀者可以試聽看看：

- [https://creators.aiva.ai/publicPlayer?c=64bd1e9ff92e9f0429470658](https://creators.aiva.ai/publicPlayer?c=64bd1e9ff92e9f0429470658)
- [https://creators.aiva.ai/publicPlayer?c=64bd1e9ff92e9f042947065a](https://creators.aiva.ai/publicPlayer?c=64bd1e9ff92e9f042947065a)

後續我有選擇與原作風格一樣的「Epic Orchestra」進行創作，結果好了不少（但有說不出的詭異感）：

- [https://creators.aiva.ai/publicPlayer?c=64bd224cbabaf01f98448cd7](https://creators.aiva.ai/publicPlayer?c=64bd224cbabaf01f98448cd7)

這個實驗結果其實也在筆者的預料之內，儘管 AI 作曲聽起來很讓人興奮，但如果你認真去聽，會發現裡面有一些不太合邏輯的轉折，以及不自然的曲調重複。

如果非相關專業的人員，我們也只能做到這一步了；但如果你本身擁有音樂的專業，AIVA 能幫你大量發想、創作，相信能激發出更多靈感。

> 很多時候過往的知識限制了我們的想像，但 AI 的創意幫我們打開了另一扇窗。

延伸閱讀：[讓我們一起掌握 AI 帶來的紅利](https://dean-lin.medium.com/list/ddcc9c53a4ac)

**想將 AI 導入自己的職場與生活嗎？歡迎[購買筆者出版的新書](https://www.books.com.tw/products/0011004216)**，幫助您**在 AI 的浪潮中站穩腳步，並在未來的職場競爭中佔據優勢！**

  [https://www.books.com.tw/products/0011004216](https://www.books.com.tw/products/0011004216)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202307/aiva-influences/
