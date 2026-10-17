# 手把手帶你用 Pika Labs 免費生成 AI 影片

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202404/pika-labs-ai-video/
- 發布：2024-04-03（更新：2024-10-24）
- 主題：AI 實戰
- 標籤：Pika、Pika Labs、AI Video Generator、AIGC、AI Video Creator
- 摘要：你可以讓自己的名字在雲朵上出現，也能讓森林以你的輪廓刻畫。 有了這款 AI 工具，想像力就是你的超能力！ 目前 Pika Labs 不但可以「免費」無限生成，而且每分鐘最多可以生成 10 次。 如果你對 AI 生成影片感興趣，Pika Labs 是你絕對不能錯過的好工具。

## 用 Pika Labs 創造專屬於你的 AI 影片

你可以讓自己的名字在雲朵上出現，也能讓森林以你的輪廓刻畫，甚至讓沙灘上出現指定的 Logo。

有了這款 AI 工具，想像力就是你的超能力！

目前 Pika Labs 不但可以「免費」無限生成，而且每分鐘最多可以生成 10 次。

如果你對 AI 生成影片感興趣，Pika Labs 是絕對不能錯過的好工具。

```
文章大綱

ㄧ、加入 Pika Labs 官方 Discord
二、「/create」文字生成影片
三、「/animate」圖片生成影片
四、「/encrypt_text」讓影片出現加密文字（ex: 讓自己的名字在雲朵上出現）
五、「/encrypt_image」讓影片出現加密圖片（ex: 讓森林以你的輪廓刻畫）
六、其他可用參數

```

## ㄧ、加入 Pika Labs 官方 Discord

Pika Labs 是在 Discord 上面操作的：[https://discord.com/invite/pika](https://discord.com/invite/pika)

如果你還沒有 Discord 的帳號，需要綁定一個 Email 後才可以使用。

加入 Pika Labs 官方 Discord 後，會有新手引導教學（不做也沒關係）。

在「generate 1–10」的頻道可以看到很多人嘗試讓 AI 產生影片，如果靈感匱乏可以到這邊逛逛。

不過筆者並不建議在這邊進行操作，因為畫面會一直被別人的指令洗掉；建議發一個訊息給 Pika Labs，這樣就可以在私人對話中生成影片了。

## 二、「/create」文字生成影片

結構「/create prompt:」，你可以在 prompt 中描述想要生成的影片，記得包含動態描述。

比如我想要建立「蝴蝶在花間飛舞」，就輸入 `/create prompt: Butterfly flying among flowers`

除了輸入描述文字外，你還可以附上給文字參考用的圖片

## 三、「/animate」圖片生成影片

「/animate」只要上傳圖片就可以生成影片，但筆者嘗試下來，建議搭配對應的 prompt 效果會比較好（其實跟「/create」用法概念相同）。

比如我提供一張「跑車在賽道上」的圖片，然後輸入 `/animate prompt: Sports car running on the track`

## 四、「/encrypt_text」讓影片出現加密文字

讓「文字」出現在影片中，目前測試下來建議低於 15 個英文字，可使用的參數如下：

- **message**：要加密的文字。
- **prompt**：描述影片主題。
- **image**（可選）：影片主題的參考圖像。（輸出結果可能長得完全不同）
- **font**（可選）：有 5 種字體可以選擇。

這邊我就將自己的英文名「Dean」印到天空的雲朵上，先用純文字的展示給大家看看效果 `/encrypt_text message:Dean prompt:White clouds churn, combined into text, sunny, blue sky and white clouds, medium to long range composition, high clarity, wide-angle lens, natural light, bright colors, outlook, relaxed and enjoyable. -motion 4 font:SANS SERIF`

下面再給大家看看搭配圖片後的效果

## 五、「/encrypt_image」讓影片出現加密圖片

讓「圖片」出現在影片中，比如自拍、Logo 這類，可使用的參數如下：

- **message**：要加密的圖片。
- **prompt**：描述影片主題。
- **「-w」**：加密圖片的權重，越高越明顯（範圍：0–2，預設：1），要加在 prompt 後面。
- **image**（可選）：影片主題的參考圖像。（輸出結果可能長得完全不同）

這邊筆者上傳自己的自拍照，讓森林以我的輪廓刻畫 `The breeze blows through the forest -w 2 -motion 4`

下面再給大家看看搭配圖片後的效果 `The breeze blows through the forest -w 2 -motion 4 -camera zoom in`

最後我們嘗試讓 Apple 的 Logo 出現在沙灘上 `Strong wind blowing on the beach, top view -w 2 -motion 4`

## 六、其他可用參數

除了上面的基礎操作外，還有一些參數可以讓我們更近一步的控制影片生成方向：

- **「-motion」**：數值越高，影片動態感越多（範圍：0–4，預設：1）
- **「-gs」**：數值越高，結果越接近你的指令（範圍：8–24，預設：12）
- **「-fps」**：每秒幀數，數值越高，影片越流暢（範圍：8–24，預設：24）
- **「-neg」**：你不希望出現的內容（ex: -neg “tree chairs” ）
- **「-ar」**：長寬比（ex: -ar 16:9）
- **「-camera」**：運鏡，縮放（-camera zoom in/out）、平移（-camera pan up/down/left/right）

這些參數我就不再各自 Demo 了，有興趣的朋友可以嘗試看看喔！

延伸閱讀：[讓我們一起掌握 AI 帶來的紅利](https://dean-lin.medium.com/list/ddcc9c53a4ac)

**想將 AI 導入自己的職場與生活嗎？歡迎[購買筆者出版的新書](https://www.books.com.tw/products/0011004216)**，幫助您**在 AI 的浪潮中站穩腳步，並在未來的職場競爭中佔據優勢！**

  [https://www.books.com.tw/products/0011004216](https://www.books.com.tw/products/0011004216)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202404/pika-labs-ai-video/
