# 用 Node.js 享受 Cloud MongoDB 吧

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202108/nodejs-mongodb-atlas/
- 發布：2021-08-26
- 主題：技術教學
- 標籤：MongoDB、MongoDB Tutorial、Node.js
- 摘要：你知道有免費的 Cloud MongoDB 資源可以使用嗎？雖然跟著官方教學走就能順利使用，但有些小細節我還是補充一下避免新手擔心，希望大家可以無障礙的使用這些資源～

你知道有免費的 Cloud MongoDB 資源可以使用嗎？雖然跟著官方教學走就能順利使用，但有些小細節我還是補充一下避免新手擔心，希望大家可以無障礙的使用這些資源～

```
大綱
一、建立雲端 MongoDB
  SETP 1：註冊帳號
  SETP 2：建立組織(Organization)
  SETP 3：建立專案(Project)
  SETP 4：建立叢集（Build your first cluster）
  SETP 5：建立 DB 使用者（Create your first database user）
  SETP 6：加入可以存取的 IP（Add IP Address to your Access List）
  SETP 7：載入範例資料（Load Sample Data）
  SETP 8：連線到這個叢集（Connect to your cluster）
二、用 Node.js 取得雲端 MongoDB 的資料

```

## 一、建立雲端 MongoDB

**SETP 1**：註冊帳號

建議直接用 Google 帳號登入就好了，這個是真的有「免費」資源讓大家使用的。

基本上你只打勾才能下一步啦

**SETP 2**：建立組織(Organization)

> 左下角的「Get Started」是官方教學，但如果在這個階段「Build your first cluster」會失敗 QQ，所以建議按照文章操作順序比較穩定

點擊「Create an Organization」

輸入組織名稱後點擊「Next」

點擊「Create Organization」建立組織，你之後還是可以邀請其他人進來這個組織，不急於現在。

**SETP 3**：建立專案(Project)

點擊「New Project」

輸入專案名稱

點擊「Create Project」建立專案

**SETP 4**：建立叢集（Build your first cluster）

選最右邊「FREE」的

這邊我是選擇「Google Cloud」在「Taiwan」的機器，選好後按「Create Cluster」

**SETP 5**：建立 DB 使用者（Create your first database user）

點擊「Add New Database User」建立使用者

他有提供三種驗證方式，如果是初學者我建議選擇「Password」就好，而密碼我建議用「Autogenerate Secure Password」產生的比較安全；其他權限都先使用默認就好，按下「Add User」便建立成功～

**SETP 6**：加入可以存取的 IP（Add IP Address to your Access List）

點擊「Add IP Address」加入 IP

如果你電腦的 IP 是固定的就選「ADD CURRENT IP ADDRESS」，如果是使用手機分享或是其他浮動 IP，你可以選「ALLOW ACCESS FROM ANYWHERE」，但如果選了這個就真的要小心保護你的帳號密碼不要外流

**SETP 7**：載入範例資料（Load Sample Data）

為了方便讓大家測試與學習，官方提供範例資料讓大家使用

點擊「Load Sample Dataset」即可，大概等個 5~8 分鐘左右載入

**SETP 8**：連線到這個叢集（Connect to your cluster）

點擊「Connect」來看連線的方式

這裡我推薦使用「MongoDB Compass」，有 GUI 介面就是友善～

下載及安裝的方式請依照自己的作業系統處理，這裏我就不贅述了；但下面連線字串真的要複製起來等等連線要用

打開安裝好的「MongoDB Compass」後，在框框內貼上剛剛複製的連線字串，記得要把 `<password>` 換成在 「STEP 5 建立 DB 使用者」的密碼

連進去後就能看到範例資料嚕～

## 二、用 Node.js 取得雲端 MongoDB 的資料

**STEP 1**：建立一個資料夾，輸入 `npm init`

**SETP 2**：安裝 MongoDB 套件 `npm install mongodb`

**STEP 3**：建立 `index.js` 並貼上官方範例程式

**SETP 4**：將 `uri` 的內容要改成自己的（你怎麼連線到 MongoDB Compass 這裡就是怎麼設定）

```
const { MongoClient } = require("mongodb");

// Replace the uri string with your MongoDB deployment's connection string.
const uri =
  "mongodb+srv://<user>:<password>@<cluster-url>?retryWrites=true&writeConcern=majority";

const client = new MongoClient(uri);

async function run() {
  try {
    await client.connect();

    const database = client.db('sample_mflix');
    const movies = database.collection('movies');

    // Query for a movie that has the title 'Back to the Future'
    const query = { title: 'Back to the Future' };
    const movie = await movies.findOne(query);

    console.log(movie);
  } finally {
    // Ensures that the client will close when you finish/error
    await client.close();
  }
}
run().catch(console.dir);

```

原始碼：[Mongo_example.js](https://gist.github.com/dean9703111/f24587554ce195dc3249ff68f7bdd3ff)（GitHub Gist）

**STEP 5**：在終端機輸入 `node index.js`

如果順利印出下面的資訊就代表你成功踏入 MongoDB 學習的第一步嚕～

**STEP 6**：如果你有驗證的心，可以回到 MongoDB Compass

選好 DB 以及 Collection 後，在 filter 輸入 `{ title: ‘Back to Future’ }` ，確認回傳結果是否與 Node.js 查到的相同

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202108/nodejs-mongodb-atlas/
