# 用 tree 套件生成專案樹狀的資料夾（目錄）結構

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202407/project-tree-structure/
- 發布：2024-07-11
- 主題：技術教學
- 標籤：Tree、npm、Folder Structure、Project Structure、README
- 摘要：隨著專案迭代，除了程式碼越來越龐大外，資料夾結構也越來越複雜。這邊推薦一個直接產生資料夾結構的套件「tree」，透過簡單的指令就能生成樹狀的目錄結構。

## 用 tree 套件生成專案樹狀資料夾（目錄）結構

隨著專案迭代，除了程式碼越來越龐大外，資料夾結構也越來越複雜。

到後來，可能連開發人員自己都搞不清楚每個資料夾在做什麼事。

為了避免交接、教育訓練時要花很多時間回憶、解釋，我們應該把這些資訊記錄到文件中。

這邊推薦一個直接產生資料夾結構的套件「[tree](https://www.npmjs.com/package/tree-cli)」，透過簡單的指令就能生成樹狀的目錄結構，以此加速文件撰寫。

## 安裝 tree 套件

在 Windows & Mac 有安裝 npm 的電腦上，只要輸入如下指令便可安裝。

```
npm install -g tree-cli

```

安裝好後，可透過 `tree -h` 查看有哪些指令可以使用。

官方文件中有提醒，Windows 要用「treee」來當指令（多一個 e），筆者的環境為 Mac。

## 使用 tree 生成樹狀的目錄結構

下面分享幾個高頻率使用的指令：

- `tree` : 顯示「資料夾」與「目錄」

- `tree -d` : 僅顯示「資料夾」

- `tree -l n` : 顯示到第 n 層目錄（tree -l 2 就是顯示兩層目錄）

- `tree –ignore pattern` : 排除不要顯示的檔案/資料夾（tree –ignore node_modules 就能排除 node_modules 資料夾）

- `tree -o filename` : 將結果寫入 filename 這個檔案

這邊展示一個組合指令： `tree –ignore node_modules -l 3 -d > README.md`

這個指令能將 3 層目錄資料夾（排除 node_modules）結果寫入 README.md 這個檔案中。

一開始生成好後，README.md 的預覽會因為沒有排版導致難以閱讀。

但只要在開頭和結尾加上「```」就會排版嚕！

以上就是這篇文章的分享，希望這個套件能幫助大家更輕鬆、清晰的撰寫與表達文件。

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202407/project-tree-structure/
