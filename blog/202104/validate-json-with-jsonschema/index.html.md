# 拒當前端黑鍋俠！用「jsonschema」來驗證後端回傳的 json 資料結構

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202104/validate-json-with-jsonschema/
- 發布：2021-04-22（更新：2023-05-05）
- 主題：技術教學
- 標籤：JSON Schema、JSON Schema Validator、後端資料驗證、JSON 格式驗證、前端密技
- 摘要：開發網頁前端時，會需要用到後端回傳的 api 數據來渲染頁面，如果雙方依照需求規格書好好實作會天下太平；但在現實生活中總是會有例外發生，如果後端傳給前端時的資料格式不夠穩定，前端在渲染的時候就可能會出錯。遇到這種狀況時，「jsonschema」套件就是你的救星！

開發網頁前端時，會需要用到後端回傳的 api 數據來渲染頁面，如果雙方依照需求規格書好好實作會天下太平；但在現實生活中總是會有例外發生，如果後端傳給前端時的資料格式不夠穩定，前端在渲染的時候就可能會出錯。遇到這種狀況時，「jsonschema」套件就是你的救星！

  圖片來源：寶寶不說貼圖

```
一、「jsonschema」是什麼？
二、「jsonschema」有哪些優點？
三、「jsonschema」驗證範例
  ➤ 簡單範例
  ➤ 複雜點的範例
  ➤ 常見的陣列範例
四、「jsonschema」其他功能

```

## 一、「jsonschema」是什麼？

如果你常常被後端回傳的 json 資料整，那這個套件你一定要學習；他是一個專們用來驗證 json 格式是否符合預期的套件。

### 可透過下面指令安裝：

```
npm i jsonschema

```

## 二、「jsonschema」有哪些優點？

- 可以詳細定義要驗證的 json 格式，包含內部所有參數的型態（string/number/array/object）
- 驗證結果會用 true/false 回傳
- 發生 error 時，會很明確的指出是在哪個位置出錯
- 可以客製化回傳錯誤的訊息
- 學習及使用非常直覺

## 三、「jsonschema」驗證範例

### 簡單範例：

我們要驗證 「4」 這個 instance 的 type 是否為 number。

```
var Validator = require('jsonschema').Validator;
var v = new Validator();
var instance = 4;
var schema = {"type": "number"};
console.log(v.validate(instance, schema));

```

原始碼：[jsonschema_simple.js](https://gist.github.com/dean9703111/a737c451c2ea22850643d6d04feaa5c0)（GitHub Gist）

程式執行結果為：

```
ValidatorResult {
  instance: 4,
  schema: { type: 'number' },
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

原始碼：[jsonschema_simple_result.js](https://gist.github.com/dean9703111/54d8b767461ed7e5ff5e7d1e6a90efa7)（GitHub Gist）

**解說**：因為 「4」的確是「number」的type，所以在「errors」參數的結果為 empty array。

### 複雜點的範例：

- 建立「驗證地址格式」的「addressSchema」
- 建立「驗證個人格式」的「schema」，並且讓個人格式的「address」關聯到「addressSchema」
- 建立一個人的物件（p），填入個人訊息
- 使用「jsonschem」提供的「Validator」 function 來驗證物件(p)是否符合個人格式(addressSchema)

```
var Validator = require('jsonschema').Validator;
var v = new Validator();

// Address, to be embedded on Person
var addressSchema = {
  "id": "/SimpleAddress",
  "type": "object",
  "properties": {
    "lines": {
      "type": "array",
      "items": {"type": "string"}
    },
    "zip": {"type": "string"},
    "city": {"type": "string"},
    "country": {"type": "string"}
  },
  "required": ["country"]
};

// Person
var schema = {
  "id": "/SimplePerson",
  "type": "object",
  "properties": {
    "name": {"type": "string"},
    "address": {"$ref": "/SimpleAddress"},
    "votes": {"type": "integer", "minimum": 1}
  }
};

var p = {
  "name": "Barack Obama",
  "address": {
    "lines": [ "1600 Pennsylvania Avenue Northwest" ],
    "zip": "DC 20500",
    "city": "Washington",
    "country": "USA"
  },
  "votes": "lots"
};

v.addSchema(addressSchema, '/SimpleAddress');
console.log(v.validate(p, schema));

```

原始碼：[jsonschema_complex.js](https://gist.github.com/dean9703111/4bfc92a7558f690594a6545a565aed62)（GitHub Gist）

程式執行結果為：

```
ValidatorResult {
  instance: {
    name: 'Barack Obama',
    address: {
      lines: [Array],
      zip: 'DC 20500',
      city: 'Washington',
      country: 'USA'
    },
    votes: 'lots'
  },
  schema: {
    id: '/SimplePerson',
    type: 'object',
    properties: { name: [Object], address: [Object], votes: [Object] }
  },
  options: {},
  path: [],
  propertyPath: 'instance',
  errors: [
    ValidationError {
      path: [Array],
      property: 'instance.votes',
      message: 'is not of a type(s) integer',
      schema: [Object],
      instance: 'lots',
      name: 'type',
      argument: [Array],
      stack: 'instance.votes is not of a type(s) integer'
    }
  ],
  throwError: undefined,
  throwFirst: undefined,
  throwAll: undefined,
  disableFormat: false
}

```

原始碼：[jsonschema_complex_result.js](https://gist.github.com/dean9703111/e0e385f07219d2e57b52cc69b47e3bce)（GitHub Gist）

**解說**：因為我們在「shemca」這個變數下設定「vote」需要為「integer」的type；但是我們傳入的「vote」內容為「lots」的字串，所以「errors」會回傳一個「is not of a type(s) integer」的錯誤訊息。

### 常見的陣列範例：

後端 api 其實很常回傳這類陣列的資訊，但因為各種歷史業障；在版本更新時，舊資料在新欄位的資料往往是空的，這時驗證回傳資料的動作就很重要了！

下面是模擬後端回傳「名字」的陣列，用「arraySchema」做格式驗證：

```
var Validator = require('jsonschema').Validator;
var v = new Validator();

var arraySchema = {
    "type": "array",
    "items": {
        "properties": {
            "name": { "type": "string" },
            "lastname": { "type": "string" }
        },
        "required": ["name", "lastname"]
    }
}
var arrayName = [
    {
        "name": "寶寶",
        "lastname": "不說"
    },
    {
        "name": "鹿尼",
        "lastname": 123
    },
    {
        "name": null,
        "lastname": "好想兔"
    },
    {
        "name": "鹿尼"
    },
]
console.log(v.validate(arrayName, arraySchema));

```

原始碼：[jsonschema_array.js](https://gist.github.com/dean9703111/b3e35d49abd28fc3032695abfb36aa7d)（GitHub Gist）

程式執行結果為：

```
ValidatorResult {
  instance: [
    { name: '寶寶', lastname: '不說' },
    { name: '鹿尼', lastname: 123 },
    { name: null, lastname: '好想兔' },
    { name: '鹿尼' }
  ],
  schema: { type: 'array', items: { properties: [Object], required: [Array] } },
  options: {},
  path: [],
  propertyPath: 'instance',
  errors: [
    ValidationError {
      path: [Array],
      property: 'instance[1].lastname',
      message: 'is not of a type(s) string',
      schema: [Object],
      instance: 123,
      name: 'type',
      argument: [Array],
      stack: 'instance[1].lastname is not of a type(s) string'
    },
    ValidationError {
      path: [Array],
      property: 'instance[2].name',
      message: 'is not of a type(s) string',
      schema: [Object],
      instance: null,
      name: 'type',
      argument: [Array],
      stack: 'instance[2].name is not of a type(s) string'
    },
    ValidationError {
      path: [Array],
      property: 'instance[3]',
      message: 'requires property "lastname"',
      schema: [Object],
      instance: [Object],
      name: 'required',
      argument: 'lastname',
      stack: 'instance[3] requires property "lastname"'
    }
  ],
  throwError: undefined,
  throwFirst: undefined,
  throwAll: undefined,
  disableFormat: false
}

```

原始碼：[jsonschema_array_result.js](https://gist.github.com/dean9703111/a3483a7f6a847be55cba539975461c89)（GitHub Gist）

**解說**：除了第一個「寶寶不說」外，我們每個 instance 其實都故意給他錯誤的資訊，來確認這個格式驗證是否生效，錯誤結果在「errors」裡面呈現的很完美。

## 四、「jsonschema」其他功能

請參考[官方網站](https://www.npmjs.com/package/jsonschema)，上面提供更多詳實的範例。

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202104/validate-json-with-jsonschema/
