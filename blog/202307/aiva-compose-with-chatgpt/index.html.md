# [AIVA 教學] 提出需求，讓 ChatGPT 協助 AI 作曲

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202307/aiva-compose-with-chatgpt/
- 發布：2023-07-14
- 主題：AI 實戰
- 系列：AIVA AI 作曲
- 標籤：AIVA、AI、ChatGPT、Music、AI Music Generator
- 摘要：AIVA 的操作介面對外行人來說還是有點難上手，我們並不知道這些參數代表的意思，以及調整後的預期結果；在過去我們會需要系統性地學習這些音樂知識，但現在可以靠 ChatGPT 幫我們解釋、給予建議，讓我們更快生成出自己期待的樂曲！

如果還沒體驗過選擇風格就能請 AI 作曲的快感，那可以先瀏覽[上一篇文章](https://deanlin.net/blog/202307/aiva-compose-by-style/)；但因為過去所有配置都是讓 AI 自由發揮，我們對音樂實際產出的掌控力極低，總不能靠碰運氣來生成合適的音樂吧？

但 AIVA 的操作介面對外行人來說還是有點難上手，我們並不知道這些參數代表的意思，以及調整後的預期結果；在過去我們會需要系統性地學習這些音樂知識，但現在可以靠 ChatGPT 幫我們解釋、給予建議，讓我們更快生成出自己期待的樂曲！

## 從操作介面認識樂曲的基礎參數

這邊直接採用上一篇文章生成的樂曲來做範例說明：

  [https://creators.aiva.ai/publicPlayer?c=64af62cc40fbb32bd5bf8cb6](https://creators.aiva.ai/publicPlayer?c=64af62cc40fbb32bd5bf8cb6)

- **TITLE**：樂曲名稱（可自行調整）
- **SOURCE：**Fantasy 奇幻音樂（我們先前從 Preset Styles 選擇的風格）
- **INSTRUMENTATION**：Soft Piano Solo（使用的樂器）
- **KEY：**F Major
- **TEMPO**：80
- **METER**： 4/4
- **CREATED**：Jul 13, 2023
- **MODIFIED**：若有調整會顯示時間點
- **DURATION**：樂曲長度

把基礎的參數列出來後，其實有些資訊我有看沒有懂，所以…就詢問 ChatGPT 啦！

## 不知道參數怎麼調？說出需求讓 ChatGPT 幫你調！

即使 ChatGPT 已經清楚說明各種參數的意義，但像筆者這種非專業人士，還是不知道如何填入合適的參數；所以我靈機一動，如果把可調整的參數提供給 ChatGPT，他是不是就能依照我的需求給予設定上的建議？

請你擔任一個專業的編曲師，我想要透過 AIVA 創作一首 [FANTASY] 風格的音樂，我希望[激勵鬥志，有史詩般壯闊的感受]，以下是可以調整的參數，請給我一些建議：

- **KEY SIGNATURE**：Any Major、Any Minor
- **PACING**：Slow、Medium、Fast
- **INSTRUMENTATION**：Dragonborn Ensemble、Symphonic Orchestra、Solo Strings、Strings & Woodwinds、String Ensemble、Strings & Chorus、Small Tavern Band、Piano Solo、Soft Piano Solo
- **DURATION**：30~330 秒

接著我就依照 ChatGPT 的建議來做設定：

> 大家可以看到我在「OF COMPOSITIONS」的參數填寫 5，這樣 AIVA 就可以一次生成 5 首，大量創作有更高機率獲得期待的音樂（類似手遊 10 連抽的概念）。

  在上圖看到有紅點的就是新產出但尚未聆聽的樂曲喔！

等幾秒鐘後，生成好的樂曲就會出現在列表上啦！僅管他們的參數都一樣，但實際上聽起來的感受差異很大，下面放兩首讓大家體驗一下：

- [https://creators.aiva.ai/publicPlayer?c=64b09ebdd62e70b62f030816](https://creators.aiva.ai/publicPlayer?c=64b09ebdd62e70b62f030816)
- [https://creators.aiva.ai/publicPlayer?c=64b09ebdd62e70b62f030819](https://creators.aiva.ai/publicPlayer?c=64b09ebdd62e70b62f030819)

## 不同風格能選的參數也不同

如果將 Preset Styles 切換到不同風格，你會發現能選擇的參數也不同，比如我選擇「**Modern Cinematic**」時，就多了「**TIME** **SIGNATURE**」的參數可以調整節拍。

並且能選擇的樂器種類（**INSTRUMENTATION**）也不太一樣：

簡單來說，就像是中餐、西餐、快餐的區別；僅管都是餐點，但是對於同一種食材的料理差異很大；以「雞」做舉例，中餐是「糖醋里肌」、西餐是「烤雞」、快餐則是「炸雞」。

## 結語

沒有人是全能的存在，我們也沒必要讓自己變成全知全能。

很多時候我們的需求並不是要達到專業水準，而僅僅是堪用即可；在這樣的條件下，AI 工具可說是你的最佳選擇。

過去你可以找藉口說自己看不懂操作介面，不知道參數怎麼調整；但有了 ChatGPT 的幫助，這一切都變得簡單起來，就看你願不願意去嘗試而已。

延伸閱讀：[AI 實戰](https://deanlin.net/categories/ai/)

**想將 AI 導入自己的職場與生活嗎？歡迎[購買筆者出版的新書](https://www.books.com.tw/products/0011004216)**，幫助您**在 AI 的浪潮中站穩腳步，並在未來的職場競爭中佔據優勢！**

  [https://www.books.com.tw/products/0011004216](https://www.books.com.tw/products/0011004216)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202307/aiva-compose-with-chatgpt/
