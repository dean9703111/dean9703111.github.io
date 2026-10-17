# 封裝(Encapsulation)能解決什麼問題？ — 用 TypeScript 學物件導向

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202208/typescript-oop-encapsulation/
- 發布：2022-08-23
- 主題：技術教學
- 系列：TypeScript 物件導向
- 標籤：封裝、Encapsulation、物件導向、OOP、Object Oriented
- 摘要：市面上很多文章、影片分享了物件導向要如何應用。 卻很少人分享這些概念是為了解決什麼問題，所以很多人就算學完了物件導向的概念，卻依舊沒有把它應用在手上的專案。 因此這個系列文中，筆者會先分享「直線思考下」我們會設計出怎樣糟糕的程式；接著再來討論如何應用物件導向的概念對程式做…

市面上很多文章、影片分享了物件導向要如何應用。

卻很少人分享**這些概念是為了解決什麼問題**，所以很多人就算學完了物件導向的概念，卻依舊沒有把它應用在手上的專案。

因此這個系列文中，筆者會先分享「直線思考下」我們會設計出怎樣糟糕的程式；接著再來討論如何應用物件導向的概念對程式做 Refactoring，提升程式未來的擴充性、維護性。

```
大綱
一、設計考卷
二、如果考卷的 Class 也被其他工程師使用
三、我們希望在「設定答案」時通知其他老師
四、老闆…我改不動了，打算用封裝進行重構
五、所以「封裝(Encapsulation)」這個概念幫我們解決了什麼問題？

```

## 一、設計考卷

假使我們今天要製作一個線上考試的系統，相信「考卷」是考試的必要元素之一，今天我們就用它來舉例，說明「封裝」的重要性。

這邊我們先簡單定義「考卷（ExaminationPaper）」該有的屬性，像是「試卷名稱（name）、題目（questions）、答案（answers）」。

```
export class ExaminationPaper {
  public name : string;
  public questions : Array<string>;
  public answers : Array<string>;

  constructor() {
    this.name = "";
    this.questions = [];
    this.answers = [];
  }
}

```

原始碼：[examinationPaper.ts](https://gist.github.com/dean9703111/2eaaaace2654968f412a9f45f22ebff8)（GitHub Gist）

如果我們想要使用裡面的屬性，只要透過 new 將它實體化，接著用「.」就能夠存取使用，像是下面的範例中我們新增了一份「數學」考卷。

```
let examinationPaper = new ExaminationPaper();
examinationPaper.name = "數學";
examinationPaper.questions = [
  "1 + 1 = ?",
  "7 x 8 = ?",
  "123 - 321 = ?"
];
examinationPaper.answers = [
  "2",
  "56",
  "-198"
];

```

原始碼：[mathExaminationPaper.ts](https://gist.github.com/dean9703111/8a594ddcd3cfbf63de41783a13cd565e)（GitHub Gist）

## 二、如果考卷的 Class 也被其他工程師使用

上面直接給考卷「試卷名稱、題目、解答」的方式看起來是不是很清爽？

但如果今天這個系統除了你之外，還有其他的工程師也使用了這個考卷的 Class，在其他檔案去新增諸如「英文、國文、歷史…」的考卷，你覺得未來可能會發生什麼問題呢？

```
import { ExaminationPaper } from "./encapsulation";

// 檔案 A.ts
let englishExaminationPaper = new ExaminationPaper();
englishExaminationPaper.name = "英文";
englishExaminationPaper.questions = ['xxx','xxx','xxx'];
englishExaminationPaper.answers = ['yy','yy','yy'];

// 檔案 B.ts
let chineseExaminationPaper = new ExaminationPaper();
chineseExaminationPaper.name = "國文";
chineseExaminationPaper.questions = ['xxx','xxx','xxx'];
chineseExaminationPaper.answers = ['yy','yy','yy'];

// 檔案 C.ts
let historyExaminationPaper = new ExaminationPaper();
historyExaminationPaper.name = "歷史";
historyExaminationPaper.questions = ['xxx','xxx','xxx'];
historyExaminationPaper.answers = ['yy','yy','yy'];

```

原始碼：[mutiUseExaminationPaper.ts](https://gist.github.com/dean9703111/53bed35a586faf21034eb689787717a1)（GitHub Gist）

## 三、我們希望在「設定答案」時通知其他老師

今天設想一個情境，老師可能在出完考卷後發現提供的答案寫錯了，因此需要修改試卷答案；而校方為了確保試卷答案的正確性，希望系統可以在「設定答案」時發信通知其他老師，讓大家確認答案是否正確。

好的，此時身為工程師的你會怎麼做？

去搜尋所有「examinationPaper.answers」的程式，然後在後面加上「sendEmail」的函式嗎？

```
let examinationPaper = new ExaminationPaper();
examinationPaper.name = "歷史";
examinationPaper.questions = ['xxx','xxx','xxx'];
examinationPaper.answers = ['yy','yy','yy'];
sendEmail(); // 有多少個地方使用到 ExaminationPaper 的 Class，就有多少個地方需要增加這個函式

```

原始碼：[addSendEmail.ts](https://gist.github.com/dean9703111/a5433b355990769b298ce4570144e4c3)（GitHub Gist）

如果你真的採用上面的做法，假使這個試卷的 Class 被引用 100 次，那就需要在那 100 個地方新增「sendEmail」的函式；這個做法不但會增加日後程式維護的困難度，要是不久後校方要求「設定答案時，需要先經過審核」，那你就會改到想死吧？

> 系統在設計時，就需要考慮到未來的擴充性；否則在需求變更時會死得很慘。

## 四、老闆…我改不動了，打算用封裝進行重構

過去我們把「name、questions、answers」都設定為 public 可以直接存取，但就如同上面的範例，正因為可以「直接存取」而導致未來程式擴充性的問題。

因此在重構上，我們將「name、questions、answers」改為 private，如果想要存取則要透過 Getters、Setters 的函式。

```
export class ExaminationPaper {
  private _name: string;
  private _questions: Array<string>;
  private _answers: Array<string>;

  constructor() {
    this._name = "";
    this._questions = [];
    this._answers = [];
  }

  public get name() {
    return this._name;
  }

  public set name(theName: string) {
    this._name = theName;
  }

  public get questions() {
    return this._questions;
  }

  public set questions(theQuestions: Array<string>) {
    this._questions = theQuestions;
  }

  public get answers() {
    return this._answers;
  }

  public set answers(theAnswers: Array<string>) {
    this._answers = theAnswers;
    this.sendEmail(); // 如果改成使用封裝，我們就只需要在這個地方新增一行就搞定了！
  }

  private sendEmail() {
    console.log("Send email to teachers.");
  }
}

```

原始碼：[refactoringExaminationPaper.ts](https://gist.github.com/dean9703111/e5b9be948c30d74ebdf7b3bb9bcbb4e4)（GitHub Gist）

重構後，儘管我們 ExaminationPaper 的 Class 變得更長，但是日後有其他需求新增時，我們可以更靈活的變更，維護性、可讀性、擴充性都增加不少。

以今天重構後的 Class 舉例，如果日後使用到設定答案：

```
examinationPaper.answers = [...];

```

就會觸發到 `sendEmail` 的函式。

```
let examinationPaper = new ExaminationPaper();
examinationPaper.name = "歷史";
examinationPaper.questions = ["xxx", "xxx", "xxx"];
examinationPaper.answers = ["yy", "yy", "yy"]; // 會因為設定試卷答案而觸發 sendEmail 函式

```

原始碼：[triggerSendEmail.ts](https://gist.github.com/dean9703111/31bb0337836de4616252ef23b66bdbb6)（GitHub Gist）

## 五、所以「封裝(Encapsulation)」這個概念幫我們解決了什麼問題？

1. 透過封裝，我們可以保護 Class 屬性，不讓人隨意修改。
2. 資料讀取、變更，透過 Getters、Setters 統一處理。
3. 因為把方法封裝，所以在需求變更時，只要修改一個地方，不需要在每個用到的地方都修改。

```
▶︎ 用 TypeScript 學物件導向
 封裝(Encapsulation)能解決什麼問題？(本篇)
 多型(Polymorphism)＆繼承(Inheritance)能解決什麼問題？

```

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202208/typescript-oop-encapsulation/
