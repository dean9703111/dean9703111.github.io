# DALL·E 3 必學的 8 個技巧 ── 輕鬆上手 AI 繪圖

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202311/dalle-3-tips/
- 發布：2023-11-21（更新：2024-10-24）
- 主題：AI 實戰
- 標籤：DALL-E 3、ChatGPT、ChatGPT Plus、GPT-4、AI
- 摘要：我一直是 Midjourney 的忠實粉絲，但隨著 ChatGPT PLUS 引入 DALL·E 3，我的忠誠開始搖搖欲墜。因為 GPT-4 結合 DALL·E 3 的強大功能，為 AI 繪圖帶來了前所未有的便利。

我一直是 Midjourney 的忠實粉絲，但隨著 ChatGPT PLUS 引入 DALL·E 3，我的忠誠開始搖搖欲墜。因為 GPT-4 結合 DALL·E 3 的強大功能，為 AI 繪圖帶來了前所未有的便利。

這篇文章會向大家說明目前 Midjourney 使用的痛點，與 DALL·E 3 的使用技巧。

## 使用 Midjourney 時我們遇到哪些問題？

筆者還是有在持續使用 Midjourney，但目前的使用頻率大幅下降，下面來分析一下 DALL·E 3 的優勢：

- **可以用對話的方式給予提示**：Midjourney 學習成本較高，像是調整圖片比例要用 `–ar 16:9` 這類需要記憶的指令；但 DALL·E 3 直接說 `給我 16:9 比例的圖片` 就好。
- **對中文理解力更好**：Midjourney 也可以使用中文，但目前有較高的機率產生出毫無關聯性的圖片；而 DALL·E 3 對中文提示的接受度較高。
- **可以產出精準文字**：Midjourney 目前無法產出精準文字，但 DALL·E 3 可以。

## 哪裡可以使用 DALL·E 3？

如果你是 ChatGPT PLUS 的會員，那就可以直接在 GPT-4 的模式下使用。

如果你不想付費，可以使用 Bing 的 Image Creator，他也是能使用 DALL·E 3 的喔：[https://www.bing.com/images/create](https://www.bing.com/images/create)?

即使超過產圖數量的限制，也只是降速而已。

## DALL·E 3 必學的 8 個技巧

不過如果想發揮最大效果，筆者還是推薦使用付費版的 GPT-4，因為他可以幫你一條龍搞定所需要的一切！

下面就以「聖誕節賀卡」來向大家做個示範。

### 1. 上傳圖片，以圖生圖

這邊我上傳一張台北 101 的圖片，然後執行 Prompt：「`這是台北 101 的照片，請將他轉為日系動漫的風格`」

### 2. 調整圖片場景

最近快要冬天了，我們就讓他應景的下雪一下吧，Prompt：「`我希望天空飄著雪，但街道有溫暖的感覺。`」

### 3. 調整畫面遠近、角度

如果你希望調整圖片遠近、視角，可以嘗試這麼做，Prompt：「`我想將鏡頭拉遠，並改成稍微俯視的角度。`」

### 4. 取得 Prompt 與 gen_id

儘管我們可以靠對話的方式去調整圖片，但如果想創造出盡可能接近的圖片，就可以問他這張圖片的 Prompt 與 gen_id，作為日後優化使用，Prompt：「`請給我這張圖片的 Prompt 以及 gen_id。`」

gen_id 是後續 referenced_image_ids 可以參考的值

### 5. 搭配 referenced_image_ids 產生類似圖片

接下來我們就用剛剛取得的 Prompt 與 gen_id 來建立圖片，Prompt：「`請使用下面的 Prompt 與 referenced_image_ids 建立圖片，我希望街道上的車多一點。 [貼上取得的 Prompt 與 gen_id]`」

透過這種方式建立的圖片，風格、構圖都會非常相近。

### 6. 圖片合成

這邊我先建立一張聖誕老人的圖片，Prompt：「`請幫我建立一張帥氣聖誕老人的圖片。`」

然後取得對應的 Prompt 與 gen_id，Prompt：「`請給我這張圖片的 Prompt 以及 gen_id。`」

接下來我們嘗試將兩張圖片合成，這邊我讓台北 101 當背景，聖誕老人在街道中出現，Prompt：「`請幫我將下面這兩段 Prompt 合成，第一段 Prompt 與 referenced_image_ids 當背景，第二段 Prompt 與 referenced_image_ids 為街道上的人物。`」

### 7. 調整圖片比例

如果你沒有特別給 ChatGPT 提示，那他就會產生隨機比例的圖片，如果你希望產生特定的比例，可以這麼說，Prompt：「`請使用下面的 Prompt 與 referenced_image_ids 建立比例為 16:9 的圖片，請記得建立圖片時不要修改 Prompt。 [貼上取得的 Prompt 與 gen_ids]`」

### 8. 加上指定文字

如果你想以這張圖產生聖誕節的賀卡，可以直接請他加上文字，Prompt：「`請幫我在圖片上方加上"Merry Christmas"的文字`」

## 結語：AI 繪圖越來越方便了！

看完這篇文章後，相信大家已經感受到 AI 繪圖的門檻快速下降，我們只要用自己熟悉的語言，透過簡單的對話就可以產出我們期待的圖片。

同樣的技巧，你可應用在 Logo、菜單、產品外型、室內設計等領域，讓你用很低的成本、超快的速度產出概念圖。

不過目前如果你想要針對圖片細節調整，或修正局部圖片，還是得靠 Midjourney 或 Stable Diffusion；但 ChatGPT 更新的速度這麼快，也許在不久後就能實現了！

但最近 OpenAI 滿城風雨，搞得我好亂啊…

延伸閱讀：[讓我們一起掌握 AI 帶來的紅利](https://dean-lin.medium.com/list/ddcc9c53a4ac)

**想將 AI 導入自己的職場與生活嗎？歡迎[購買筆者出版的新書](https://www.books.com.tw/products/0011004216)**，幫助您**在 AI 的浪潮中站穩腳步，並在未來的職場競爭中佔據優勢！**

  [https://www.books.com.tw/products/0011004216](https://www.books.com.tw/products/0011004216)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202311/dalle-3-tips/
