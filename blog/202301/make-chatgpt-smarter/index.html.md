# 3 個小技巧讓 ChatGPT 變聰明！

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202301/make-chatgpt-smarter/
- 發布：2023-01-26
- 主題：AI 實戰
- 標籤：ChatGPT、Tips、ChatGPT Prompt、Skills、ChatGPT Review
- 摘要：先前在「如何寫出有效的 Prompt，獲得更好的 ChatGPT 回覆」這篇文章中，筆者介紹了 prompt 的基礎使用原則，而這篇文章則是再補充一些小技巧（簡寫、符號、語言設定），讓你能更靈活的操作 ChatGPT。

先前在「[如何寫出有效的 Prompt，獲得更好的 ChatGPT 回覆](https://deanlin.net/blog/202301/effective-chatgpt-prompts/)」這篇文章中，筆者介紹了 prompt 的基礎使用原則，而這篇文章則是再補充一些小技巧（簡寫、符號、語言設定），讓你能更靈活的操作 ChatGPT。

```
大綱

 TIP 1：設定 ChatGPT 快捷鍵（簡寫）
 TIP 2：用符號讓 ChatGPT 理解要處理的任務
 TIP 3：設定 ChatGPT 回應的語言

```

## TIP 1：設定 ChatGPT 快捷鍵（簡寫）

```
接下來的對話，如果我只傳送「c」這個字母，就是「繼續完成回答」的意思。

```

因為 ChatGPT 的回答長度有限制，常常說到一半就斷了，要輸入「繼續」才能看到後續的文字；如果你跟筆者一樣覺得煩，不妨給他設計個快捷鍵。

> **小提醒**：理論上設定的「快捷鍵」在同個聊天室內都有效，但筆者使用下來發現偶爾會跑掉，因此建議讀者把常用的設定複製一份。

## TIP 2：用符號讓 ChatGPT 理解要處理的任務

其實 ChatGPT 的理解能力已經很強了，但有時就是回答不到重點，此時我們可以加上`「」`這類的符號讓他理解。

```
優化文案「ERP 系統，量身打造，系統廠首選」

```

你也可以增加一些附加的描述、限制。

```
優化「」內的文案，擴充到 1000 字，分段描述。

「ERP 系統，為客戶量身打造，系統廠首選」

```

發現話還沒說完，剛好可以試試看剛剛設定的快捷鍵「c」。

> 我只給他短短的描述，他就能從幾個關鍵字擴展到這個地步，這個自由發揮真的厲害。

## TIP 3：設定 ChatGPT 回應的語言

```
不管我用什麼語言詢問，接下來的對話都使用「繁體中文」回答我。

```

「xxx」可以自由填入其他語言（ex：英文、日文…），[在之前的文章中](https://deanlin.net/blog/202301/chatgpt-chinese-vs-english/)，筆者實驗過 ChatGPT 的英文能力比較好，尤其涉及專業知識的部分（ex：程式語言）。

所以我們可以取個巧，用「英文」提問，然後要求他用「繁體中文」回答。

> **小提醒**：如果你只要求用「中文」回答，有高機率收到簡體字；另外即使已經告訴他要用「繁體中文」，偶爾還是會跳掉。

```
不管我使用什麼語言詢問，接下來的對話都使用「繁體中文」回答我。
Explain decorator pattern, use exmple with node.js.

```

這邊筆者是用英文詢問 ChatGPT 關於 Decorator 這個 Design Pattern 的解釋，並要求他提供「Node.js」的範例程式。

從回答你可以看到 ChatGPT 的確有使用「繁體中文」來回應，在最後也提供了一個用 Node.js 撰寫的範例：

```
class Request {
  constructor(user) {
    this.user = user;
  }

  get content() {
    return 'This is the base request content';
  }
}

class MemberDecorator {
  constructor(request) {
    this.request = request;
  }

  get content() {
    return `[Member] ${this.request.content}`;
  }
}

class AdminDecorator {
  constructor(request) {
    this.request = request;
  }

  get content() {
    return `[Admin] ${this.request.content}`;
  }
}

const user = {
  role: 'admin',
};

let request = new Request(user);
if (user.role === 'member') {
  request = new MemberDecorator(request);
} else if (user.role === 'admin') {
  request = new AdminDecorator(request);
}
console.log(request.content);

```

我覺得他給的範例挺清晰易懂的，完整的說明我就不貼上來混字數了；有興趣的朋友可以複製筆者的指令，直接貼到 ChatGPT 來嘗試看看。

未來筆者會出一系列使用 ChatGPT、openAI 的文章，如果你對這個議題感興趣，可以持續關注我的文章！

延伸閱讀：[讓我們一起掌握 AI 帶來的紅利](https://dean-lin.medium.com/list/ddcc9c53a4ac)

**想將 AI 導入自己的職場與生活嗎？歡迎[購買筆者出版的新書](https://www.books.com.tw/products/0011004216)**，幫助您**在 AI 的浪潮中站穩腳步，並在未來的職場競爭中佔據優勢！**

  [https://www.books.com.tw/products/0011004216](https://www.books.com.tw/products/0011004216)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202301/make-chatgpt-smarter/
