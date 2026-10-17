# 必讀！了解 ChatGPT 有哪些問題與限制

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202301/chatgpt-limitations/
- 發布：2023-01-26
- 主題：AI 實戰
- 標籤：ChatGPT、ChatGPT Chatbot、Bugs、Issues、Limits
- 摘要：筆者最近高頻率的使用 ChatGPT，在享受便利的同時也碰到了不少坑（bugs & issues & limits）；因此將這些問題彙整起來與大家分享。

筆者最近高頻率的使用 ChatGPT，在享受便利的同時也碰到了不少坑（bugs & issues & limits）；因此將這些問題彙整起來與大家分享。

```
大綱

 回答未必是正確的
 回答的資料有限
 拒絕回答敏感問題（ex：股票、虛擬貨幣漲跌、暴力、犯罪...）
 回答到一半斷掉
 提問次數限制
 穩定性問題
 流量大時狀況多

```

## 回答未必是正確的

> May occasionally generate incorrect information（可能偶爾會產生不正確的信息）

ChatGPT 知道很多知識，但有時會給出一些看似正確，但實際上是錯誤的回應。

以下是筆者碰過的錯誤回答：

- 詢問他某個地方的旅遊景點 ➝ 景點不存在，或是根本不在哪個地方。
- 詢問他當地知名餐廳 ➝ 名字跟簡介煞有其事，結果一查根本不存在。
- 請他幫忙寫程式 Side Project ➝ 選用過時套件，使用套件方法錯誤。
- 請他協助重構程式 ➝ 重構後的程式可以運作，但部分解釋不完全正確。

有興趣的讀者可以去以下文章回顧：

- [如何寫出有效的 Prompt，獲得更好的 ChatGPT 回覆](https://deanlin.net/blog/202301/effective-chatgpt-prompts/)
- [靠問 ChatGPT 寫程式，能完成 OpenAI & Linebot 的 Side Project 嗎？](https://deanlin.net/blog/202301/chatgpt-line-bot-side-project-part-2/)
- [ChatGPT Code Review、Refactoring、Comments 的能力如何？](https://deanlin.net/blog/202301/chatgpt-code-review-refactoring/)

雖然前幾篇文章已經提過了，但我還是再次提醒讀者：「無論未來人工智慧發展的多完善，我們都要保持獨立思考的能力，以及對答案的懷疑態度，請勿將 AI 的回答作為唯一的決策依據。」

## 回答的資料有限

> Limited knowledge of world and events after 2021.（對 2021 年後的世界和事件了解有限）

因為他的資料截止於 2021 年，所以較新的資訊都無法提供，這也進階導致了部分回答的準確率；不過未來如果與搜尋引擎結合，那能力肯定大幅提升。

## 拒絕回答敏感問題（ex：股票、虛擬貨幣漲跌、暴力、犯罪…）

> Trained to decline inappropriate requests.（受過拒絕不當請求的訓練）

如果詢問他股票漲跌、虛擬貨幣未來走勢，或是些暴力犯罪的問題，他會拒絕回答。

> 筆者在網路上看過有人用「寫劇本」的方式來繞過這個限制，但個人並不推薦這種操作。

## 回答到一半斷掉

相信讀者使用 ChatGPT 時，一定都遇過回答到一半就斷掉的問題（如下圖）。

此時用「繼續」這類關鍵字就能讓他繼續完成回答。

## 提問次數限制

如果在短時間詢問太多問題，你可能會看到如下圖示：

目前暫時無解，需要等半小時或是更多時間才能使用；未來 ChatGPT 會推出付費版本，相信到時會提高使用者的詢問次數、增加回覆速度。

## 穩定性問題

有時回答到一半就無法回答了：

又或是把一些很正常的內容判定成違反內容政策：

通常這類狀況只要刷新頁面（Reload）或開新的對話（New chat）就可以解決（也可以透過連結表單回覆你的建議，讓 AI 未來變得更完善）。

## 流量大時狀況多

如果你還沒登入，會被直接卡在外面：

如果你已經登入了，那網頁會三不五時就卡住，刷新頁面後會有如下彈窗：

以上是筆者目前遇過的狀況，不知道讀者還有碰到其他錯誤的情境嗎？歡迎留言分享。

目前 ChatGPT 還在測試階段，相信日後推出的版本（ex：付費版）會提供更穩定的品質＆準確的答案，就讓我們拭目以待吧！

延伸閱讀：[AI 實戰](https://deanlin.net/categories/ai/)

**想將 AI 導入自己的職場與生活嗎？歡迎[購買筆者出版的新書](https://www.books.com.tw/products/0011004216)**，幫助您**在 AI 的浪潮中站穩腳步，並在未來的職場競爭中佔據優勢！**

  [https://www.books.com.tw/products/0011004216](https://www.books.com.tw/products/0011004216)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202301/chatgpt-limitations/
