# [面試][後端]設計 API 時會考慮哪些點？

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202110/fullstack-career-notes-day-16/
- 發布：2021-10-01
- 主題：職涯成長
- 系列：全端工程師生存筆記
- 標籤：iT 邦幫忙鐵人賽、後端驗證、單元測試、Restful API、Graphql
- 摘要：既然 Junoir 跟 Senior 的後端工程師都能寫出可以運作的 API；那公司為什麼要多花錢請一個比較貴的工程師呢？

你設計的 API 除了跑的動以外，它安全穩定嗎？

既然 Junoir 跟 Senior 的後端工程師都能寫出可以運作的 API；那公司為什麼要多花錢請一個比較貴的工程師呢？

不僅面試官會問這個問題，你也要時常反問自己：「`我憑什麼可以開出更高的薪水？我跟其他的工程師有哪裡不一樣？`」

## 大綱

- **設計 API 時會考慮哪些點？**

- 1.1 面試官為什麼會問？
- 1.2 面試官想從答案確認什麼？
- 1.3 筆者提供的簡答

- **回答問題所需具備的知識**

- 2.1 驗證 Client 端身份
- 2.2 了解後端驗證的重要性，並認識常見的 Validation Rule
- 2.3 優化後端的常見方法
- 2.4 淺談 Unit Test（單元測試）

- **衍伸問題**

- 3.1 你知道 PUT & PATCH 的差異嗎？
- 3.2 請舉例 RESTful api 的命名原則
- 3.3 GraphQL 跟 RESTful API 的差異在哪裡？

## 1. 設計 API 時會考慮哪些點？

### 1.1 面試官為什麼會問？

能依照需求規格書寫好 API 只是對後端工程師的基礎要求；但這個 API 考慮的是否周全就看每個工程師的經驗了。

這算是`常見面試題`，面試官想從回答中了解你後端的實作經驗與基本功，並判斷你在進入團隊後是否需要花更多時間訓練才能成為有效戰力。

### 1.2 面試官想從答案確認什麼？

- 你設計的 API 是否有防護的機制
- 當 Client 端傳入不符規範的參數及型態時，你的 API 是否有成為防線
- 你有用什麼方法保證 API 的穩定性
- 是否有思考過 API 的優化

### 1.3 筆者提供的簡答

以過去設計的內部管理系統來說，Client 端在登入後會取得 JWT，之後在存取 API 前會用這個 `JWT 來驗證是否有存取的權限`；為了避免 Client 端使用不符規範的參數，我會先`驗證每個傳入參數的格式`，並撰寫 `Unit Test` 確保這隻 API 在不同情境下的 Response 都符合期待；再確認 API 的穩定性後，我會去思考還有什麼地方可以優化，讓 Client 端有更好的體驗。

## 2. 回答問題所需具備的知識

### 2.1 驗證 Client 端身份

這邊帶讀者瞭解兩種主流驗證機制，大部分的框架都有提供對應的方案供開發人員使用。

- **Session 與 Cookie**
當 Client 端登入後，Server 會產生一筆 Session 並記錄（可能存在資料庫或記憶體），同時將可以識別身份的 SessionID 送回 Client 端儲存於 Cookie；此後 Client 端的請求，Server 只需要根據 Cookie 中的 SessionID 尋找 Session 來驗證使用者即可。

- **使用 Session 要考量的問題**

- 因為 Session 的資料存在 Server 中，需考慮同時有`很多使用者上線時要面對的效能或是記憶體問題`。

- **解決方案**：定期清理過期 Session。

- 如果採取 Cluster 部署，就要考慮`不同 Server 的 Session 共享問題`。

- **解決方案**：Server 需設計 Session 同步機制（Replication）。

- **JSON Web Token（JWT）**
當 Client 端登入成功後，Server 會回傳一個 JWT，Client 端通常會將 JWT 儲存於 localstorage；當 Client 端打算訪問受保護的資源時，需要在 Header 的 Authorization 使用 Bearer 模式添加 JWT，如果驗證通過即可存取相關資源。

- **使用 JWT 要考量的問題**

- 在 Client 端取得 JWT 後，在 JWT 有效期內他都能夠使用；即使我們直接把使用者帳號刪除，這個 JWT 依舊可以訪問受保護的資源。

- **解決方案**：設計黑名單機制，使用記憶體資料庫維護一份黑名單，如果想讓某個 JWT 失效，把它加入黑名單就好了。

- `不要為了方便把 JWT 的有效期限設定太長`，不然這個 JWT 洩露出去後，任何人都可以拿它來幹大事。

- **解決方案**：JWT 有效期限不要太長，同時使用 HTTPS 來減少盜用。

### 2.2 了解後端驗證的重要性，並認識常見的 Validation Rule

`無論前端驗證做得多完美，後端驗證都是必須存在的`；因為想攻擊系統的人不一定會從前端執行，直接 Call API 嘗試所有可能性的效率快多了；`前端驗證可以大幅減少不必要的 Request 產生，後端驗證則是保護資料的最終防線`。

- **為什麼要了解 Validation Rule？**

- 你可以節省非常多親手撰寫 Function 的時間（自己寫的效能跟穩定度未必好，還很佔版面）。
- 這些不符規範的資料並不會進入後端運算的邏輯層面。
- 驗證發現錯誤就 Response Error，減少 Client 端等待時間。

接下來用情境來介紹幾個常見的 Validation Rule，不同框架的名字可能略有不同，我這邊以 [Laravel](https://laravel.tw/docs/5.0/validation#available-validation-rules) 的為主。

- **驗證使用者註冊系統時填寫的參數**

- 此為 Laravel 驗證傳入參數的程式，下方依據參數進行解說。

```
$validator = Validator::make($request->all(), [
  'email' =>  ['required', 'email', 'string', 'max:255', 'unique:users'],
  'name' =>  ['required', 'string', 'max:50'],
  'password' => ['required', 'string', 'max:255', 'min:8', 'regex:/^.*(?=.{8,})(?=.*?[a-zA-Z])(?=.*?[0-9]).*$/', 'confirmed']
]);

```

- **email**
為必填 `required` 欄位，同時檢查傳入的字串是否符合 `email` 格式以及長度 `max` 是否超過限制，並確認這個 email 並沒有被註冊過 `unique`。
- **name**
為必填 `required` 欄位，同時檢查傳入的字串長度 `max` 是否超過限制。
- **password**
為必填 `required` 欄位，除了檢查傳入的字串長度 `max` 是否超過限制外，通常會限制最小長度 `min`；再嚴謹一點會要求 password 複雜度需包含英數字 `regex`，且內容與 password_confirmation 欄位相同 `confirmed`。

### 2.3 優化後端的常見方法

這邊先不討論程式語言和資料庫選擇帶來的影響，專注在共同可行性方案上。

- **操作資料庫時加入限制條件**
就算前端 Request 的參數沒有限制條件（ex：分頁、筆數、種類…），後端也要有意識的`設定預設值`，不然當資料量大的時候除了會跑很久浪費效能外，前端也不適合接收/呈現這麼多資訊。
- **回傳前端的資料盡量是已經計算過的**
Client 端使用的機器效能未知，如果把太多的邏輯運算交給前端可能導致不好的使用者體驗；我這邊建議後端最好`回傳計算後的資料`，前端負責顯示為主，不過這塊也要看實際的業務量以及情境來 trade-off 。
- **加入 CDN Cache**
如果遇到國外客戶抱怨連線速度很慢，就可以考慮採用 CDN 來解決這個問題；CDN 服務商的做法是將 Server 建置在世界各地，將網站上的靜態內容（圖片、影音、檔案）與動態內容（資料庫查詢）Cache 到 Server，`根據訪客的地理位置，從最近的 Server 提供資料`；好處如下：

- 提升網站的穩定度（如果某個 CDN Server 掛了也有替身）。
- 節省原本 Server 的頻寬（因為流量被分散了）。
- 改善使用者體驗（過去會卡住的都是大檔案，有了 CDN Cache 後瀏覽會更流暢）。

- **提升硬體水平**
江湖傳言：「效能不夠，機器來湊！」雖然聽起來好像是在開玩笑；但實際業務上如果真的遇到效能瓶頸，許多公司的做法就是`先靠硬體讓服務穩定`；服務穩定了，你才有心力去 Refactor 程式。

### 2.4 淺談 Unit Test（單元測試）

- **為什麼 Unit Test 很重要？**

- 有它保證`程式碼的穩定性`，發生錯誤時可以快速找到源頭，因此開發人員可以更大膽的開發。
- 減少手動測試的時間，大幅`降低相關維運成本`。
- API 版本更新時（ex：v2 升級到 v3），`確保舊版 API 可以正常運行`。

- **Unit Test 使用時機**

- 開發人員要先在本機 Run 過一遍，確認所有 Unit Test 都通過。
- 如果有使用 Git 做版本控制，在特定 branch 合併時 CI/CD 應該要有某個 Stage 來做測試，`確保合併分支的程式正確性`。

- **撰寫 Unit Test 基礎概念**

- **測試時使用（或自動建立）Mock data，不要動到 Server 資料**
Unit Test 著重在`測試程式邏輯`，建議測試時使用 Mock data，不然可能導致正式環境發生意外。
- **測試要涵蓋你程式中的所有邏輯**

- 假設一個 API 有 12 種回傳邏輯，你的 Unit Test 就要有 12 個；根據莫非定律，`少寫的測試往往上線後就會爆掉`。
- 如果 API 有做 JWT 防護、Request 參數驗證，我建議要`把所有錯誤情境都做一次模擬`；甚至要模擬 JWT 還在有效期，但實際權限已被移除的`邊緣測試`。

筆者聽過有些公司在專案正式 Release 時沒有撰寫任何的 Unit Test，直接讓客戶人肉測試，等客戶回報錯誤時再去修復 Bug；嗯…這種公司的離職率通常很高，因為有一堆歷史業障需要修但沒人敢碰。

## 3. 衍伸問題

### 3.1 你知道 PUT & PATCH 的差異嗎？

`考點：對於 API 基礎操作的掌握程度`

「PUT」是`替換`資源，而「PATCH」則是`更新部份`資源內容；以使用者舉例，假設他有「姓名、簡介、年齡」這幾個欄位，如果想更新「姓名」的資訊：

- 用 PUT 方法送出更新，除了「姓名」外還要填寫所有的欄位，如果沒填寫會導致其他的欄位更新成預設值。
- 用 PATCH 方法送出更新，只需填寫要更新的「姓名」欄位即可。

### 3.2 請舉例 RESTful API 的命名原則

`考點：確認你寫 API 的習慣是否符合標準`

假設今天有一個 users 的 Table。

- **取得 user list**

- **方法**：GET
- **API 路徑**：`/api/users`

- **取得 user detail**

- **方法**：GET
- **API 路徑**：`/api/users/{user_id}`

- **新增 user**

- **方法**：POST
- **API 路徑**：`/api/users`

- **更新 user 資訊**

- **方法**：PUT
- **API 路徑**：`/api/users/{user_id}`

- **刪除 user**

- **方法**：DELETE
- **API 路徑**：`/api/users/{user_id}`

### 3.3 GraphQL 跟 RESTful API 的差異在哪裡？

`考點：確認你對市場技術的認知程度`

過去 `RESTful API` 需要根據需求規格書給`每個 API 設計回傳的資料結構`，如果這個 API 在很多頁面被共用就有可能要回傳更多的資訊，但回傳的資訊中有些是只有特定頁面才有需求的，假使拆成多隻 API 也會導致日後維護困難；而 `GraphQL` 的出現正好解決了這個問題，他能夠`由 Client 端來定義 Server 端要回傳的資料結構`，不再回傳冗贅資訊。

- **補充：用範例了解兩者差異**
要查詢圖書館中特定書籍，回傳內容須包含：「作者、書名、出版年」。

- **GraphQL**

- **Client 端傳送的 query**

```
query {
  books (id:12) {
    authors {
        firstName
        lastName
    }
    title
    yearPublished
  }
}

```

- **Server 端 Response 的 result**

```
{
  "data": {
    "books": {
      "authors": [
        {
          "firstName": "Lin",
          "lastName": "Dean"
        }
      ],
      "title": "全端工程師生存筆記",
      "yearPublished": "2022"
    }
  }
}

```

- **RESTful API**

- **Client 端的 Request**
需要拆成`GET api/books/12`、`GET api/authors/12`來分別取得書籍、作者資訊。

- **Server 端 Response 的 result**
`GET api/books/12`

```
{
  "title" : "全端工程師生存筆記",
  "authorID": 12,
  "yearPublished" : 2022,
  "page": 168,
  "tags": ["履歷"、"面試"、"職場"]
}

```

`GET api/authors/12`

```
{
  "firstName": "Lin",
  "lastName": "Dean"
}

```

在上面的範例中，RESTful API 需要兩個 Request 才能取得完整資料，如果想用一個 Request 取得完整資料就要重新建立一支 API；比較起來`GraphQL 對前端工程師來說是更好使用的工具`；不過轉換與建立的成本較 RESTful API 高，如果想嘗試 GraphQL 帶來的效益，可以考慮漸進式的遷移。

感謝大家的閱讀，如果喜歡我的文章可以`訂閱`接收通知；如果有幫助到你，按`Like`可以讓我更有寫文的動力，我們明天見～

### 參考資源

- [進階 RESTful API 討論](https://ithelp.ithome.com.tw/articles/10224134)
- [透過 JWT 實作驗證機制](https://medium.com/%E9%BA%A5%E5%85%8B%E7%9A%84%E5%8D%8A%E8%B7%AF%E5%87%BA%E5%AE%B6%E7%AD%86%E8%A8%98/2e64d72594f8)
- [認識 Cookie、Session、Token 與 JWT](https://blog.yyisyou.tw/5d272c64/)
- [簡單聊一聊 Cookie、Session、Token、JWT 的區別和作用](https://segmentfault.com/a/1190000021810849)

在許多人的幫助下，本系列文章已成功出版，除了添加新的篇章，更完善了每個案例的應對進退；如果對現在的職涯感到迷茫，也許這本書能帶給你不一樣的觀點～

**天瓏書局：** [https://www.tenlong.com.tw/products/9786263334571](https://www.tenlong.com.tw/products/9786263334571)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202110/fullstack-career-notes-day-16/
