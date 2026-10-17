# 突破限制！用偽代碼讓 ChatGPT 的 GPT-4o 一次生成多張圖片

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202406/gpt-4o-multiple-images-pseudocode/
- 發布：2024-06-17
- 主題：AI 實戰
- 標籤：ChatGPT、GPT-4o、AI、Pseudocode、AI Image Generator
- 摘要：儘管靠 GPT-4o 的 DALL·E 生成圖片很方便，但一直有個痛點。 那就是我需要根據主題想合適的 Prompt，儘管能靠 ChatGPT 幫忙想 Prompt，但操作會多好幾個步驟。 所以今天分享的技巧，就是教大家如何透過偽代碼將任務流程化，一次生成多張不同風格的圖片！

我現在幾乎每張部落格封面都是靠 ChatGPT 的 DALL·E 來生成，儘管很方便，但一直有個痛點。

那就是我需要根據主題想合適的 Prompt，但有時候怎麼下都不對。

靈感枯竭時，我會請 ChatGPT 幫忙想 Prompt，但會多好幾個操作步驟，非常浪費時間（真是不知足）。

因此在前陣子看到尹相志老師的「[GPT-4o偽代碼繪本生成術](https://www.youtube.com/watch?v=3rb-54Q5fig&ab_channel=%E5%B0%B9%E7%9B%B8%E5%BF%97)」影片後，真的是如獲至寶！生產力大增！

這篇文章在原有的 Prompt 上做了一些調整，透過偽代碼將任務流程化後，大家只要輸入圖片主題，就能一次性生成多張不同風格的圖片（文末有完整的 Prompt）！

```
文章大綱

一、什麼是偽代碼（Pseudocode）？
二、偽代碼結構說明
三、在 GPT-4o 用偽代碼ㄧ次生成多張圖片
四、你可能會遇到的問題

```

## 一、什麼是偽代碼（Pseudocode）？

首先，偽代碼不是真正的程式語言。

他是結合了自然語言與程式語言的描述方式，讓我們更直觀的理解演算法，通俗點講就是「用白話文寫程式」。

有時 ChatGPT 對自然語言的理解能力不足，但透過偽代碼來表達，它就更能理解使用者要做的任務。

這篇文章會用到一點程式概念，有下面的基礎觀念會更好

- **變數（Variable）**: 你可以把它想像成是一個儲存資訊的小盒子，可以在需要的時候取出或更改它。比如我們可以用 image_prompts 來儲存生成圖片的指令。
- **函式（Function）**: 它就像是會執行特定任務的機器人，你只要把素材給他就會得到對應的結果。像設計一個 image_generation 的函式，我們只要放入 image_prompt 與 style（特定風格）就能生成圖片。
- **迴圈（Loop）**: 他可以讓程式重複執行特定任務。像這次我們就透過 for 迴圈來產生多張圖片。

## 二、偽代碼結構說明

尹相志老師的 Prompt 已經很完善了，下面是根據個人的實際需求去做的調整。

### PART 1： 讓 ChatGPT 知道如何閱讀你的 Prompt

開頭 3 行很重要，如果沒有特別註明的話失敗率極高。

```
## 以下是**偽代碼 Prompt**，請依照其語意一步一步地來執行，不該有遺漏
## 停用代碼解釋器，但開啟上網瀏覽搜尋以及 Dalle 功能
## 使用繁體中文回覆

```

### PART 2： 設定好變數讓 ChatGPT 知道要生成的主題、風格、描述

topic 可以換成自己想要的主題，style 與 image_prompts 用角色扮演能得到更好的結果。

```
topic= (打破框架)
style= (請扮演一個[擅長製作部落格封面的自媒體經營者]，你知道怎麼樣的部落格設計可以吸引讀者的眼球。請參考 topic 來生成合適的風格)
image_prompts= (請扮演一個[擅長製作部落格封面的自媒體經營者]，你了解怎麼樣的封面描述會讓讀者瘋狂。請根據 topic 產生 10 個不同的描述)

```

### PART 3： 設計函式，讓 ChatGPT 知道要怎麼做事

可以透過 size 來指定圖片的大小，但目前只有 1024x1024、1024x1792、1792x1024 可以選擇。

```
def image_generation(image_prompt, style):
    final_prompt=請根據[image_prompt]以及[style]補充色調、背景描述、具體風格、畫面細節，請至少放 3 個效果詞(光照效果、色彩色調、渲染效果、視覺風格)和 1 個以上的構圖技巧，以構成完整的 Prompt，不要有文字出現在圖中，使用英文撰寫
    請根據以下引數內容{size="1792x1024", prompt=final_prompt}來調用內部工具 Dalle 以生成並展示圖片

```

### PART 4： 透過迴圈執行函式來生成圖片

```
for image_prompt in image_prompts:
    image_generation(image_prompt,style)

```

## 三、在 GPT-4o 用偽代碼ㄧ次生成多張圖片

上面的 Prompt 整合如下：

```
## 以下是**偽代碼 Prompt**，請依照其語意一步一步地來執行，不該有遺漏
## 停用代碼解釋器，但開啟上網瀏覽搜尋以及Dalle功能
## 使用繁體中文回覆

topic= (打破框架)
style= (請扮演一個[擅長製作部落格封面的自媒體經營者]，你知道怎麼樣的部落格設計可以吸引讀者的眼球。請參考 topic 來生成合適的風格)
image_prompts= (請扮演一個[擅長製作部落格封面的自媒體經營者]，你了解怎麼樣的封面描述會讓讀者瘋狂。請根據 topic 產生 10 個不同的描述)

def image_generation(image_prompt, style):
    final_prompt=使用英文撰寫 prompt，並根據[image_prompt]以及[style]補充色調、背景描述、具體風格、畫面細節，請至少放 3 個效果詞(光照效果、色彩色調、渲染效果、視覺風格)和 1 個以上的構圖技巧，以構成完整的 final_prompt，並且不要有文字出現在圖中。
    請根據以下引數內容{size="1792x1024", prompt=final_prompt}來調用內部工具 Dalle 以生成並展示圖片

for image_prompt in image_prompts:
    image_generation(image_prompt,style)

```

打開 ChatGPT，選擇「ChatGPT 4o」就可以開始看他表演了。

從下圖可以看到他一次生成多張圖片。

或者透過影片可以更直觀的感受（有開倍速）：

[YouTube 影片：用偽代碼讓 ChatGPT 的 GPT-4o 一次生成多張圖片](https://www.youtube.com/watch?v=ErCfbqW83n4)

## 四、你可能會遇到的問題

- 雖然靠偽代碼可以一次性大量生成圖片，但 ChatGPT 的額度還是有限制的，到達額度限制後就需要等一段時間才能繼續使用（如下圖）。

- 儘管透過偽代碼，ChatGPT 還是有一定的機率不理你，可能一次僅產生一張圖；此時可透過「請跑完全部 prompt」嘗試讓他執行完畢。

- ChatGPT 可能生成到一半卡住，你可以透過「繼續」等關鍵字讓他跑完。

以上就是今天分享的內容，希望這篇文章可以提升大家的生產力～

延伸閱讀：[讓我們一起掌握 AI 帶來的紅利](https://dean-lin.medium.com/list/ddcc9c53a4ac)

**想將 AI 導入自己的職場與生活嗎？歡迎[購買筆者出版的新書](https://www.books.com.tw/products/0011004216)**，幫助您**在 AI 的浪潮中站穩腳步，並在未來的職場競爭中佔據優勢！**

  [https://www.books.com.tw/products/0011004216](https://www.books.com.tw/products/0011004216)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202406/gpt-4o-multiple-images-pseudocode/
