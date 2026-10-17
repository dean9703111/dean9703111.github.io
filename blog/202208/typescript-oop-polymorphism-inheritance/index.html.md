# 多型(Polymorphism)＆繼承(Inheritance)能解決什麼問題？ — 用 TypeScript 學物件導向

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202208/typescript-oop-polymorphism-inheritance/
- 發布：2022-08-25（更新：2023-05-05）
- 主題：技術教學
- 系列：TypeScript 物件導向
- 標籤：Polymorphism、Inheritance、多型、繼承、物件導向
- 摘要：市面上很多文章、影片分享了物件導向要如何應用。 卻很少人分享這些概念最初是為了解決什麼問題，所以很多人就算學完了物件導向的概念，卻依舊沒有把它應用在手上的專案。 因此這個系列文中，筆者會先分享「直線思考下」我們會設計出怎樣糟糕的程式…

市面上很多文章、影片分享了物件導向要如何應用。

卻很少人分享**這些概念最初是為了解決什麼問題**，所以很多人就算學完了物件導向的概念，卻依舊沒有把它應用在手上的專案。

因此這個系列文中，筆者會先分享「直線思考下」我們會設計出怎樣糟糕的程式；接著再來討論如何應用物件導向的概念對程式做 Refactoring，提升程式未來的擴充性、維護性。

```
大綱
一、建立一個讓不同職業交流的平台
二、老闆想要再多加幾個職業
三、[Refactoring]將「介紹技能」程式移轉到 Class 變成 Function
四、平台經營的很成功，要擴增「技能交換、讀書會」的功能
五、[Refactoring]老闆…我改不動了，打算用繼承進行重構
六、應用多型讓不同職業的工作者進行交流
七、所以「多型(Polymorphism)＆繼承(Inheritance)」的概念幫我們解決了什麼問題？

```

## 一、建立一個讓不同職業交流的平台

既然要建立不同職業交流的平台，那工作者就是必不可少的元素；我們先建立一個「工作者（Worker）」的 Class，並設計「name（姓名）、jobType（職業類型）」作為屬性。

因為這個系統只是試水溫的階段，所以在一開始只開放「工程師（ENGINEER）、設計師（DESIGNER）、律師（LAWYER）」這幾個職業（Job），我們用列舉（enum）的方式來實現。

```
export enum Job { // 職業對應的常數
  ENGINEER = 0,
  DESIGNER = 1,
  LAWYER = 2,
}
export class Worker {
  private _name: string; // 姓名
  private _jobType: number; // 職業類型

  constructor(theName: string, theJobType: number) {
    this._name = theName;
    this._jobType = theJobType;
  }

  public get name() {
    return this._name;
  }

  public set name(theName: string) {
    this._name = theName;
  }

  public get jobType() {
    return this._jobType;
  }

  public set jobType(theJobType: number) {
    this._jobType = theJobType;
  }
}

```

原始碼：[Worker-init.ts](https://gist.github.com/dean9703111/be165f0a7735971ae8a62346ff2bf668)（GitHub Gist）

在 Worker 的 Class 建立完後，我們便可以將對應的「職業」實體化；既然我們要讓不同的職業進行交流，就模擬一小段讓他們「介紹技能」的程式。

```
let workers = [
  new Worker("Dean", Job.ENGINEER), // 產生工作者
  new Worker("Amy", Job.DESIGNER),
  new Worker("Peter", Job.LAWYER),
];

workers.forEach((worker) => {
  console.log(worker.name);
  switch (worker.jobType) {
    case Job.ENGINEER: {
      console.log("我超會寫程式！");
      break;
    }
    case Job.DESIGNER: {
      console.log("我超會UI設計！");
      break;
    }
    case Job.LAWYER: {
      console.log("我超會打官司！");
      break;
    }
  }
});

```

原始碼：[introSkill-init.ts](https://gist.github.com/dean9703111/c1611b4265fa117c34e724cba80cbae6)（GitHub Gist）

## 二、老闆想要再多加幾個職業

系統上線後反應熱烈，老闆希望再新增「魔術師（MAGICIAN）、畫家（PAINTER）」這兩個職業，於是我們在 Job 裡面新增這兩個職業的常數。

```
export enum Job { // 職業對應的常數
  ENGINEER = 0,
  DESIGNER = 1,
  LAWYER = 2,
  MAGICIAN = 3,
  PAINTER = 4
}

```

原始碼：[job-add.ts](https://gist.github.com/dean9703111/ff56305a0ac0798f4668290d8de420f4)（GitHub Gist）

在 Worker 的 Class 調整完畢後，我們也要同步調整讓工作者「介紹技能」的程式。

```
let workers = [
  new Worker("Dean", Job.ENGINEER), // 產生工作者
  new Worker("Amy", Job.DESIGNER),
  new Worker("Peter", Job.LAWYER),
  new Worker("Davy", Job.MAGICIAN),
  new Worker("Angela", Job.PAINTER),
];

workers.forEach((worker) => {
  console.log(worker.name);
  switch (worker.jobType) {
    case Job.ENGINEER: {
      console.log("我超會寫程式！");
      break;
    }
    case Job.DESIGNER: {
      console.log("我超會UI設計！");
      break;
    }
    case Job.LAWYER: {
      console.log("我超會打官司！");
      break;
    }
    case Job.MAGICIAN: {
      console.log("我會變魔術！");
      break;
    }
    case Job.PAINTER: {
      console.log("我會畫畫！");
      break;
    }
  }
});

```

原始碼：[introSkill-add.ts](https://gist.github.com/dean9703111/aac5820b25bd16363c29d8b65251edc0)（GitHub Gist）

我想程式寫到這裡，應該有很多人發覺不對勁了，如果每新增一個職業，我們就要在另一個檔案中調整程式，這樣伴隨系統的成長，我們會越來越難維護它。

## 三、[Refactoring]將「介紹技能」程式移轉到 Class 變成 Function

為了避免日後我們要在多個地方維護程式，我們把判斷職業的 switch 轉移到 Worker 的 Class 中，並新增一個「introSkill」的 Function 讓外部使用。

```
export class Worker {
  private _name: string; // 姓名
  private _jobType: number; // 職業類型

  constructor(theName: string, theJobType: number) {
    this._name = theName;
    this._jobType = theJobType;
  }

  public get name() {
    return this._name;
  }

  public set name(theName: string) {
    this._name = theName;
  }

  public get jobType() {
    return this._jobType;
  }

  public set jobType(theJobType: number) {
    this._jobType = theJobType;
  }

  public introSkill() {
    switch (this.jobType) {
      case Job.ENGINEER: {
        console.log("我超會寫程式！");
        break;
      }
      case Job.DESIGNER: {
        console.log("我超會UI設計！");
        break;
      }
      case Job.LAWYER: {
        console.log("我是傳奇律師！");
        break;
      }
      case Job.MAGICIAN: {
        console.log("我是魔術師！");
        break;
      }
      case Job.PAINTER: {
        console.log("我是畫家！");
        break;
      }
    }
  }
}

```

原始碼：[worker-addFun.ts](https://gist.github.com/dean9703111/5face493257f1308f643c9a83c1350c1)（GitHub Gist）

經過上面的調整後，不同職業的工作者如果想「介紹技能」，呼叫 introSkill 的 Function 即可。

```
let workers = [
  new Worker("Dean", Job.ENGINEER), // 產生工作者
  new Worker("Amy", Job.DESIGNER),
  new Worker("Peter", Job.LAWYER),
  new Worker("Davy", Job.MAGICIAN),
  new Worker("Angela", Job.PAINTER),
];

workers.forEach((worker) => {
  console.log(worker.name);
  worker.introSkill() // 改成函式後就不用這樣改來改去了
});

```

原始碼：[worker-refactoring.ts](https://gist.github.com/dean9703111/95eab0cb648c88bfc94690d9acf43ce1)（GitHub Gist）

## 四、平台經營的很成功，要擴增「技能交換、讀書會」的功能

為了讓各行各業交流的更加熱烈、深化自身技能，老闆請你增加「技能交換（skillExchange）、讀書會（studyGroup）」的功能，如果你依照原本的邏輯繼續設計程式會長這樣。

```
export class Worker {
  private _name: string; // 姓名
  private _jobType: number; // 職業類型

  constructor(theName: string, theJobType: number) {
    this._name = theName;
    this._jobType = theJobType;
  }

  public get name() {
    return this._name;
  }

  public set name(theName: string) {
    this._name = theName;
  }

  public get jobType() {
    return this._jobType;
  }

  public set jobType(theJobType: number) {
    this._jobType = theJobType;
  }

  public introSkill() {
    switch (this.jobType) {
      case Job.ENGINEER: {
        console.log("我超會寫程式！");
        break;
      }
      case Job.DESIGNER: {
        console.log("我超會UI設計！");
        break;
      }
      case Job.LAWYER: {
        console.log("我是傳奇律師！");
        break;
      }
      case Job.MAGICIAN: {
        console.log("我是魔術師！");
        break;
      }
      case Job.PAINTER: {
        console.log("我是畫家！");
        break;
      }
    }
  }
  public skillExchange(jobType: number) {
    switch (jobType) {
      case Job.ENGINEER: {
        console.log("我想學程式！");
        break;
      }
      case Job.DESIGNER: {
        console.log("我想學UI設計！");
        break;
      }
      case Job.LAWYER: {
        console.log("我想學法律！");
        break;
      }
      case Job.MAGICIAN: {
        console.log("我想學魔術！");
        break;
      }
      case Job.PAINTER: {
        console.log("我想學畫畫！");
        break;
      }
    }
  }

  public studyGroup() {
    switch (this.jobType) {
      case Job.ENGINEER: {
        console.log("程式讀書會");
        break;
      }
      case Job.DESIGNER: {
        console.log("UI設計讀書會！");
        break;
      }
      case Job.LAWYER: {
        console.log("法律讀書會");
        break;
      }
      case Job.MAGICIAN: {
        console.log("魔術讀書會");
        break;
      }
      case Job.PAINTER: {
        console.log("畫畫讀書會");
        break;
      }
    }
  }
}

```

原始碼：[worker-ugly-code.ts](https://gist.github.com/dean9703111/44adfd5e4a16fe329eba6558af647b9d)（GitHub Gist）

我們現在才 5 個職業、3 個功能就把 Worker 這個 Class 搞的這麼冗長了，現在先來分析這個程式日後會遭遇的問題：

- **維護性**：日後增加新的職業，需要在每個 switch 設計對應的 case；如果職業繁多，那產生 Bug 是完全可以預期的事情。
- **可讀性**：如果這個平台由多位工程師共同開發，那其他工程師使用到 Worker 這個 Class 時可能會很抖，因為無論閱讀理解還是改寫都很困難。
- **擴充性**：如果未來需要根據特定職業設計獨有的 Function（比如只允許工程師使用 Programing），那可能就要動到 Class 的結構了。

## 五、[Refactoring]老闆…我改不動了，打算用繼承進行重構

既然我們已經體會到「新增職業」帶來的痛苦，不如先用繼承的概念重構，將「工作者（Worker）」設計為基礎類別（父類別）。

因為職位有「多樣性」，所以我們將 Worker 定義為「抽象類別（abstract class）」，並將「skill、skillExchange、studyGroup」這些功能抽象化，讓繼承的衍生類別（子類別）去實現。

```
export abstract class Worker {
  private _name: string; // 姓名

  constructor(theName: string) {
    this._name = theName;
  }

  public get name() {
    return this._name;
  }

  public set name(theName: string) {
    this._name = theName;
  }
  abstract introSkill(): void;

  abstract skillExchange(): void;

  abstract studyGroup(): void;
}

```

原始碼：[woker-inheritance.ts](https://gist.github.com/dean9703111/3daa4538e6869e1ef1a8fb0a1a960803)（GitHub Gist）

日後我們想新增一個職業時，只要讓他繼承「工作者（Worker）」的 Class，並實作抽象的功能即可；下面舉一個新增「工程師（Engineer）」這個職業的例子。

```
class Engineer extends Worker {
  introSkill() {
    console.log("我超會寫程式！");
  }

  skillExchange() {
    console.log("我想學程式！");
  }

  studyGroup() {
    console.log("程式讀書會");
  }
}

let engineer = new Engineer("Dean");
engineer.studyGroup();

```

原始碼：[engineer.ts](https://gist.github.com/dean9703111/a7d4b0cec3577037451b155d83f21128)（GitHub Gist）

程式重構後，我們新增職業不但變得輕鬆，還大幅降低產生 Bug 的風險；因爲繼承 Worker 的 Class 後，會明確告訴你需要實作的功能有哪些（如果沒有實作就會報錯😅）。

## 六、應用多型讓不同職業的工作者進行交流

接著我們回到讓不同職業的工作者「介紹技能」的程式。

這次我們在陣列中塞了兩種職業（Engineer、Designer）；不過因為這些職業都是 Worker 的衍生類別，所以在 forEach 中我們可以用「Worker」進行解析，進而存取 Worker 所提供的方法（introSkill）。

```
let workers = [new Engineer("Dean"), new Designer("Amy")];
workers.forEach((worker: Worker) => {
  console.log(worker.name);
  worker.introSkill();
});

```

原始碼：[worker-polymorphism.ts](https://gist.github.com/dean9703111/17b1efe18be6402d4d88f70a299acf3a)（GitHub Gist）

**備註 1**：如果今天我們在工程師（Engineer）的 Class 新增 programming 的方法，那這個方法在 forEach 裡面是無法使用的（因為裡面僅允許 Worker 的方法）。
**備註 2**：如果我們今天在陣列中增加一個並非 Worker 的衍生類別（ex：new Animal()），那 forEach 會因為偵測到你放了非 Worker 的衍生類別而報錯。

## 七、所以「多型(Polymorphism)＆繼承(Inheritance)」的概念幫我們解決了什麼問題？

這邊筆者就分享自己使用下來最直觀的感受：

- 繼承的概念讓我們的程式可以被重複使用。
- 多形的概念幫我們把功能抽象化，根據實際的需求去執行。

**參考資源**：[Java物件導向 重觀念迅速教學 3 Polymorphism 什麼是繼承? 什麼是多型？為何多型以及如何設計多型？](https://www.youtube.com/watch?v=FkgLc5gFPAU&list=PLicQRHHL75d7RG_kfnEFAEnBouLncvwQp&ab_channel=%E6%B0%B4%E7%90%83%E6%BD%98)

```
▶︎ 用 TypeScript 學物件導向
 封裝(Encapsulation)能解決什麼問題？
 多型(Polymorphism)＆繼承(Inheritance)能解決什麼問題？(本篇)

```

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202208/typescript-oop-polymorphism-inheritance/
