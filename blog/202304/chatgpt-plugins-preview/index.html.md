# ChatGPT plugins 新功能搶先看（網頁瀏覽、外掛產圖&發文、程式直譯）

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202304/chatgpt-plugins-preview/
- 發布：2023-04-25（更新：2024-10-24）
- 主題：AI 實戰
- 標籤：ChatGPT、ChatGPT Plugins、OpenAI、OpenAI ChatGPT、ChatGPT Plus
- 摘要：OpenAI 聯合創辦人Greg Brockman 在 4 月 TED 演講時，向大家分享 ChatGPT 將推出的新功能（Browsing、Code Interpreter、Plugins）。這篇文章將會向大家展示用ChatGPT產圖、線上購物、發送twitter貼文…

[OpenAI 聯合創辦人Greg Brockman 在 4 月 TED 演講時](https://www.youtube.com/watch?v=C_78DM8fG6E)，向大家分享了 ChatGPT 即將推出的新功能。

筆者撰文時，這些功能尚未完全開放；目前預計開放時會先讓付費用戶體驗（ChatGPT PLUS），在文末有附上加入等候清單的官網連結。

  ChatGPT付費版新功能

從展示的下拉選單中，我們可以看到多出了「Browsing、Code Interpreter、Plugins」他們各自能完成什麼任務呢？讓我們看下去！

```
大綱

 在ChatGPT產圖、線上購物、發送twitter貼文
 連到網路驗證自己的內容是否正確
 分析Excel內容，產出驚人圖表
 結語：如果想用 AI 做產品請三思

```

## 在 ChatGPT 產圖、線上購物、發送 twitter 貼文

這邊用到的是「Plugins」中「DALL.E APP」的功能。

上圖講者輸入「Suggest a nice post-TED meal, and draw a picture（推薦一頓TED演講後的餐點，並將他畫出來）」後，ChatGPT 先用文字建議。

最後直接產生一幅美味晚餐的圖片

如果你想知道這個美麗晚餐的背後是什麼 Prompt，ChatGPT 也可以直接秀出 DALL.E 的 Prompt。

上面的功能已經很驚人了對吧？接下來的這個更猛！

講者選用了「Instacart（美國電商）、Zapier（連結各式應用的自動化工具）」這兩個 Plugins，然後要求 ChatGPT 將他之前建議好吃的東西列一個購物清單，然後在推特上發布給所有 TED 觀眾。

**STEP 1**：ChatGPT 先做檢索

**STEP 2**：然後產生對應的購物連結

**STEP 3**：進入網站後是已經選好物品的購物車

**STEP 4**：隨後透過 Zapier 這個外掛導向到tweet發文前的畫面

**STEP 5**：然後就真的發文成功了！

在上面這段演示中，我們看到了「產生圖片、購買商品、發布貼文」等操作都幾乎靠**自動化**實現了！

## 連到網路驗證自己的內容是否正確

過去 ChatGPT 最為人詬病的問題之一就是會胡說八道，針對這個問題，講者也用「詢問兩篇部落格時間的間隔」直接示範給大家看。

針對這個問題，ChatGPT 回答前者於 2017 / 04 / 24 發佈，而後者在 2017 / 06 / 27 發佈，因此兩者大約相隔兩個月。

不過這真的是事實嗎？講者將 Model切換到「Browsing」，然後把一樣的問題丟給ChatGPT。

這個模式下的 ChatGPT 沒有直接回應，而是一步步說明自己在做的事情（跟 New Bing有點像）。

**STEP 1**：上網搜尋

**STEP 2**：依序告訴你他執行的順序

**STEP 3**：最終將搜尋到的內容整合出答案

在遇到事實性的問題時，原本的ChatGPT 容易給出錯誤的答案；但加上「Browsing」後，他就能給出前者於 2017 / 04 / 6 發佈，而後者在 2017 / 06 / 13 發佈，因此兩者相隔兩個月又ㄧ週的正確結論。

儘管驗證事實這塊人類也能做，但真的沒有任何生產力且繁複，ChatGPT能幫我們完成這個任務真的幫到很大的忙。

## 分析 Excel 內容，產出驚人圖表

講者秀出了一個彙過去 30 年人工智慧論文的 Excel 表（約 16.7萬篇），然後告訴我們 ChatGPT 如何分析這些數據。

  人工智慧論文的 Excel 表

這次他選用「Code Interpreter（程式直譯）」的 Model，並上傳剛剛的 Excel 表。

然後 ChatGPT 就能夠很神的分析出這個 Excel 每個欄位所代表的意義，然後詢問使用者要對數據做分析或是視覺化。

這時講者要求ChatGPT給一些「探索性的圖（exploratory graph）」，面對這種模糊的需求，聰明的ChatGPT 給他3種呈現方式：

1. **直方圖**：顯示每篇論文的作者數量，讓我們了解AI研究的典型團隊規模。

2. **折線圖**：顯示每年論文發表的總數，以此展示AI研究的趨勢。

3. **論文標題的單字雲**：顯示AI研究標題中最常出現的單字。

  **直方圖**

  **折線圖**

  **論文標題的單字雲**

此時講者發現折線圖中 2023 年的數據有問題，AI 應該指數成長卻被腰斬；所以向 ChatGPT 說明2023年尚未過完，可以重新做一個公平的預測嗎？

最後 ChatGPT 也不負眾望的向大家展示了新的繪製成果，並且在圖片上標明2023 年的論文發表總數是預測的。

如果你有興趣，也可以看 ChatGPT 是如何用 Python 將這些數據分析、繪製出來的。

想搶先體驗新功能的朋友可以加入等候清單喔：[https://openai.com/waitlist/plugins](https://l.facebook.com/l.php?u=https%3A%2F%2Fopenai.com%2Fwaitlist%2Fplugins%3Ffbclid%3DIwAR3gPvnT_gF2d2yZSE5q1TPvncoQBLAo1E4Lun0B5fqxZMGC8cvwj7CzPKA&h=AT1IewtX1_-ZtWx-qgby-OuRwajSY2TOh8i-ovuMXld9ZN0NKjxlpH3RBUPfldFjZ95-BHas7YT-g4eUE4VH5DoPKtJrxJm0Uyk6spCI-ILgx5N6dqC4SufUFuCJQemK47ef_QxePA&__tn__=R]-R&c[0]=AT2CJjwCgax8N6srJJ3zsNkCY2ElNuahARS_wV5Z4K_q5YgL2LCjenQ4_gLu4WTMuTYC0BF6NDlesfnBhxZkwtImnqQU0T89zDAX99lZ4WS0Q583TfxjxSpnPItJ2sa1te0FqaEzDd0R0DtelOn4QCzko6wbKYt3lmxNohSnPV43TJT85k06fUbcOGPTvi42bLYMllbnt-UTvQkj1Ffolgg)

## 結語：如果想用 AI 做產品請三思

從 Demo 中可以看到這次 ChatGPT 的改版是非常有感的，跟過去只能文字輸入/輸出的型態相比，他多了非常多應用的可能性。

現在市面上有許多新興的產品是依附在 OpenAI 的 API 上面的，但如果哪天OpenAI 突然跑出來自己做，那對產品無疑是一個毀滅性打擊。

如果是針對 OpenAI 缺點做的產品就更危險了，你都能補足這個缺點了，OpenAI 官方會沒想到嗎？只是看他想不想去優化而已。

門檻越低的產品越容易被淘汰，在 AI 工具急速發展的年代更是如此！

延伸閱讀：[讓我們一起掌握 AI 帶來的紅利](https://dean-lin.medium.com/list/ddcc9c53a4ac)

**想將 AI 導入自己的職場與生活嗎？歡迎[購買筆者出版的新書](https://www.books.com.tw/products/0011004216)**，幫助您**在 AI 的浪潮中站穩腳步，並在未來的職場競爭中佔據優勢！**

  [https://www.books.com.tw/products/0011004216](https://www.books.com.tw/products/0011004216)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202304/chatgpt-plugins-preview/
