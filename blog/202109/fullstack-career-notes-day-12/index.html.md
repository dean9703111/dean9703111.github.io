# [面試][前端]在使用後端的資料前，你有先做驗證嗎？

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202109/fullstack-career-notes-day-12/
- 發布：2021-09-27
- 主題：職涯成長
- 系列：全端工程師生存筆記
- 標籤：iT 邦幫忙鐵人賽、前端面試、前端驗證、Jsonschema、前端設計
- 摘要：回想當年還是前端菜鳥時，我完全信任後端前輩回傳的資料；記得當年有個產品上線前的測試一切正常，但上線幾天後 PM 就接到客戶回報系統上有頁面顯示不全的 Bug；因為網頁前端是我負責的，所以第一個被抓去興師問罪。

## 筆者背鍋小故事

回想當年還是前端菜鳥時，我完全信任後端前輩回傳的資料；記得當年有個產品上線前的測試一切正常，但上線幾天後 PM 就接到客戶回報系統上有頁面顯示不全的 Bug；因為網頁前端是我負責的，所以第一個被抓去興師問罪。

經驗不足的我在收到 Bug 後，先用本機的環境模擬客戶遇到的問題，但弄了老半天始終查不出問題發生的原因；最後到客戶的正式機上查看顯示不全的頁面，才發現原來是後端忘記更新正式機的 API 版本；把狀況跟後端的前輩溝通後，才順利解決這個 Bug。

為了避免這種狀況再次發生；之後在寫前端時，都會先驗證後端回傳的資料是否符合規定的 JSON 資料結構。

## 大綱

1. **在使用後端的資料前，你有先做驗證嗎？**

2. 1.1 面試官為什麼會問？
3. 1.2 面試官想從答案確認什麼？
4. 1.3 筆者提供的簡答

5. **回答問題所需具備的知識**

6. 2.1 認識「JSON schema」
7. 2.2 安裝「jsonschema」並了解它的優點
8. 2.3 「jsonschema」驗證範例

9. **衍伸問題**

10. 3.1 前端開發時會考慮哪些問題

## 1. 在使用後端的資料前，你有先做驗證嗎？

### 1.1 面試官為什麼會問？

這算是常見面試題，主要確認你的前端技術力，這類題目範圍非常廣泛；有興趣的朋友可以看我放在參考資源的「[前端工程師面試問題集](https://h5bp.org/Front-end-Developer-Interview-Questions/translations/chinese-traditional/)」；因為問題太過廣泛，幾乎無從準備，所以面試時就看個人基本功夠不夠深，經手的專案有沒有用到了。

### 1.2 面試官想從答案確認什麼？

- 你過去的專案是否有做這塊的基礎驗證
- 你的基本功是否紮實

### 1.3 筆者提供的簡答

現在的專案採前後端分離的架構，在前端使用後端的資料前，我會先透過「jsonschema」這款套件驗證 JSON 資料結構，確認後端回傳的欄位是否齊全、格式是否正確；以此避免前端顯示有問題的資訊，在遇到問題時也可以快速釐清是前端還是後端造成的。

## 2. 回答問題所需具備的知識

### 2.1 認識「JSON schema」

在介紹套件前，先讓大家對「JSON schema」有一個基礎認知；它是一種基於 JSON 格式定義 JSON 資料結構的規範，下面放上一段範例讓大家快速了解：

```
const person_schema = {
  type: "object",
  properties: {
    name: { type: "string" },
    age: { type: "number", maximum: 150 },
  },
  required: ["name", "age"],
};

```

我相信上面這個「JSON schema」應該是非常簡單易懂，他要求 JSON 的格式需要符合以下要求：

- name 為 string
- age 為 number 且最大值不得超過 150
- name 與 age 都是必需的

### 2.2 安裝「jsonschema」並了解它的優點

現在有許多套件提供驗證 JSON 格式的功能，今天筆者用自己熟悉的套件：「`jsonschema`」來跟讀者們分享，了解使用的邏輯後，想換成其他套件都非常容易。

#### 安裝步驟

- **SETP 1**：建立一個專案資料夾。
- **SETP 2**：在終端機輸入 `npm init` 初始化專案。
- **SETP 3**：在終端機輸入 `npm i jsonschema` 安裝套件。

#### 套件優點

如果後端 API 版本變更、 DB 的 Table 欄位變更沒有通知前端，前端頁面就可能發生新資料沒有位置放、舊資料部分欄位為空的狀況；如果沒有這個套件守護前端，工程師可能就要背黑鍋了。

- 可以詳細定義要驗證的 JSON 資料結構，包含內部所有參數的型態（string/number/array/object）。
- 驗證到錯誤格式時，會明確說明錯誤的原因。
- 可客製化錯誤訊息，轉換成好理解的文字。
- 學習及使用非常直覺。

### 2.3 「jsonschema」驗證範例

- **SETP 1**：在專案資料夾下建立「simple.js」來了解如何驗證 JSON 資料結構。

- **STEP 2**：以剛剛的 `person_schema` 作為範例來跟大家做講解，程式如下：

```
var Validator = require("jsonschema").Validator;
var v = new Validator();

const personSchema = {
  type: "object",
  properties: {
    name: { type: "string" },
    age: { type: "number", maximum: 150 },
  },
  required: ["name", "age"],
};
var person = {
  name: "baobao",
  age: 14,
};
let result = v.validate(person, personSchema);
console.log("valid：" + result.valid);
console.log(result);

```

- **STEP 3**：在終端機輸入 `node simple.js`，會看到如下的驗證結果：

```
valid：true
ValidatorResult {
  instance: { name: 'baobao', age: 14 },
  schema: {
      type: 'object',
      properties: { name: [Object], age: [Object] },
      required: [ 'name', 'age' ]
  },
  options: {},
  path: [],
  propertyPath: 'instance',
  errors: [],
  throwError: undefined,
  throwFirst: undefined,
  throwAll: undefined,
  disableFormat: false
}

```

- **STEP 4**：了解輸出參數的意義

- **valid**：用布林（boolean）型態回傳驗證是否通過（true/false）。
- **instance**：這是我們想要驗證的資料，此範例要驗證的資料是`person`這個變數。
- **schema**：我們提供驗證的規則（JSON schema）。
- **errors**：會以陣列的形式顯示驗證發現的錯誤，你可以透過這個參數在前端顯示錯誤原因；因此次驗證的資料結構正確，故回傳空陣列。

- **STEP 5**：將`person`改為錯誤的資料結構：

- 將必填參數`name`移除
- 把 age 的數字填寫超過最大值

```
var person = {
  age: 200,
};

```

- **STEP 6**：在終端機輸入 `node simple.js`，確認驗證結果的 errors 是否捕捉到錯誤。

```
errors: [
    ValidationError {
      path: [Array],
      property: 'instance.age',
      message: 'must be less than or equal to 150',
      schema: [Object],
      instance: 200,
      name: 'maximum',
      argument: 150,
      stack: 'instance.age must be less than or equal to 150'
    },
    ValidationError {
      path: [],
      property: 'instance',
      message: 'requires property "name"',
      schema: [Object],
      instance: [Object],
      name: 'required',
      argument: 'name',
      stack: 'instance requires property "name"'
    }
],

```

透過以上範例，相信大家都明白「jsonschema」套件的基礎使用方式，有興趣深入研究的朋友請參考[官網](https://www.npmjs.com/package/jsonschema)文件；如果沒接觸過「JSON schema」的朋友建議自己手動實做看看，這算是非常重要的前端概念。

## 3. 衍伸問題

### 3.1 前端開發時會考慮哪些問題

> 這就是個大哉問，每個人都有自己考慮的點，我建議回答自己擅長且真的有導入專案的部分，因為通常後續會有更深入的問題。

考點：透過一個廣泛的問題，了解你開發時最在意的點

為了減少設計變更，在開始前會先請 UI/UX 設計好 `Wireframe`（[Figma](https://www.figma.com/)），並向使用者說明流程取得共識，直到雙方確認都沒問題後再進入開發階段；為了讓網頁風格一致，前端在設計時會遵循 `Material Design`。

為了提升開發效率，會先思考哪個 `Framework` 較適合這個專案；在依照設計提供的 Protype 完成設計後，會先用[Responsively](https://medium.com/dean-lin/de5f9efe9f4c)這款工具，確認網頁在不同解析度的裝置上是否會跑版。

完成前期開發後，會思考如何優化效能，像是用 lazy loading 提升頁面載入速度、減少頁面 Request 數量及 Response 時間，透過這些設計讓使用者有更好的體驗。

感謝大家的閱讀，如果喜歡我的文章可以`訂閱`接收通知；如果有幫助到你，按`Like`可以讓我更有寫文的動力，我們明天見～

### 參考資源

1. [拒當前端黑鍋俠！用「jsonschema」來驗證後端回傳的 json 資料結構(筆者部落格)](https://medium.com/dean-lin/a0940809a394#08eb)
2. [ajv-validator/ajv](https://github.com/ajv-validator/ajv)
3. [驗證資料的好幫手 - JSON Schema Validator](https://github.com/Hsueh-Jen/blog/issues/2)
4. [前端工程師面試問題集](https://h5bp.org/Front-end-Developer-Interview-Questions/translations/chinese-traditional/)

> 在許多人的幫助下，本系列文章已成功出版，除了添加新的篇章，更完善了每個案例的應對進退；如果對現在的職涯感到迷茫，也許這本書能帶給你不一樣的觀點～
> **天瓏書局：** [https://www.tenlong.com.tw/products/9786263334571](https://www.tenlong.com.tw/products/9786263334571)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202109/fullstack-career-notes-day-12/
