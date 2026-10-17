# 工程師的開發環境大補帖：Node.js、NVM、git、yarn、VSCode

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202101/developer-environment-setup/
- 發布：2021-01-15（更新：2023-05-05）
- 主題：技術教學
- 標籤：Node.js、nvm、Git、Yarn、VS Code
- 摘要：工欲善其事，必先利其器

工欲善其事，必先利其器

## 為什麼寫這篇文章？

因為我自己在`剛接觸程式時最痛恨的就是環境設定`
不知道是我跟電腦不合還是怎樣，每次我學新東西的時候總是在環境設定這個環節卡的不要不要的
為了避免讀者在之後實戰的文章中一邊看一邊心理罵:「**為什麼我們程式一模一樣但我的就是會錯?**」這樣的憾事發生，所以我決定詳細的說明這個專案所需安裝的一切環境，並附上我本機安裝的工具版本，讓大家在日後的文章中可以順利操作與學習

你們實際操作時通常都有新的版本了，如果最新版本有問題可以切換到筆者的版本來操作

## 需要安裝哪些工具

`說明包含 windows/mac 兩個系統的設定`

- [Node.js](https://deanlin.net/blog/202101/developer-environment-setup/#e634)
- [NVM](https://deanlin.net/blog/202101/developer-environment-setup/#8082)
- [git](https://deanlin.net/blog/202101/developer-environment-setup/#6dcd)
- [yarn](https://deanlin.net/blog/202101/developer-environment-setup/#bf69)
- [VSCode](https://deanlin.net/blog/202101/developer-environment-setup/#d582)

## 開發環境介紹 & 設定

## 1. Node.js [官網](https://nodejs.org/en/about/)

能夠在 server 端運行 JavaScript 的開放原始碼、跨平台 JavaScript 的執行環境。前端工程師的福音

筆者安裝的版本：v16.19.1 (使用nvm安裝，後面會介紹)

## 2. nvm [官網](https://github.com/nvm-sh/nvm)

由於 Node.js 更新速度是非常快的，而許多套件有 Node.js 版本的相依性，透過NVM你可以輕鬆切換 Node.js 版本來符合不同專案的使用環境

### ➤ windows 安裝步驟

- 先進入[windows下載頁面](https://github.com/coreybutler/nvm-windows/releases)
- 選擇最新版本的下載 **nvm-setup.zip**(筆者版本為1.1.7，你看到這篇文章時可能有提供更新的版本)

3. 下載後一路瘋狂下一步即可

4. 安裝後開啟命令提示字元(CMD)

5. 在 CMD 輸入指令確認 nvm 是否安裝成功

### ➤ mac 安裝步驟

- 打開終端機(Terminal)，以下步驟皆在終端機操作
- mac 建議先下載 Homebrew

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"

```

3. 下載後安裝 nvm

```
brew install nvm

```

4. 在終端機輸入指令確認 nvm 是否安裝成功

### ➤ 用 NVM 安裝 node.js

- `指令：nvm install [version]`
- 請切記要安裝一個 Node.js 版本(請選用[LTS版本](https://nodejs.org/en/))，不然你寫的程式會沒有可以運行的環境

```
nvm install 16.19.1

```

筆者nvm安裝版本：0.39.3

## 3. git [官網](https://git-scm.com/)

這是對於程式版本控制的工具，透過他你可以輕鬆回到過去，並且在多人協作時也能有效管控
[windows下載頁面](https://git-scm.com/download/win)、[mac下載頁面](https://git-scm.com/download/mac)

我有朋友使用 windows 怎麼樣都無法正常安裝 git，如果你也遇到了這個不幸，你還是有[Git桌面板](https://desktop.github.com/)可以選擇的

### 常用指令

- 抓取遠端數據庫(僅確認有什麼更新，不合併)

```
git fetch

```

- 抓取遠端數據庫更新

```
git pull

```

- 更新遠端數據庫

```
git push

```

筆者windows git安裝版本：2.27.0.windows.1

## 4. yarn

這是專門安裝套件的軟體，站在巨人的肩膀上你才能看得更遠，妥善運用別人提供的套件，程式的路能走得更遠
[下載頁面](https://classic.yarnpkg.com/lang/en/)

### 常用指令

- 依據 package.json 安裝所有套件

```
yarn

```

- 安裝到 dependencies

```
yarn add [package]

```

- 安裝到 devDependencies

```
yarn add [package] –save-dev

```

- 全域安裝套件(使該套件可在電腦任何位置被呼叫)

```
yarn global add [package]

```

筆者yarn安裝版本：1.19.2

## 5. VScode [官網](https://code.visualstudio.com/)

現在已經過了用記事本在打程式的年代了，透過 IDE(整合開發環境)可以大幅增加你撰寫程式的速度
[VScode下載頁面](https://code.visualstudio.com/)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202101/developer-environment-setup/
