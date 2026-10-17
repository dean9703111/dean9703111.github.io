# NUXTJS GCP serverless實戰

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202006/nuxtjs-gcp-serverless/
- 發布：2020-06-24
- 主題：技術教學
- 標籤：Nuxt.js、GCP、Serverless、App Engine、Front End Development
- 摘要：因為跟著官方文件做，讓我被雷了整整一天，於是我決定紀錄操作流程，希望看到的人可以節省時間

### 事前準備

1. 你有GCP的帳戶(沒有就直接去開一個)
2. 你有信用卡(因為GCP serverless服務一定要綁信用卡，就算你一塊錢都沒花，你還是得綁定)
3. 你會用git(如果不會你要抄襲前人的成果都很難)
4. 如果有mac電腦會是最棒的，因為作者的操作都是在mac的環境

### 文章目標

1. 根據官方文件建立Nuxt專案
2. 開啟GCP App Engine
3. 將Nuxt專案deploy到GCP

SETP 1/5 : 建立Nuxt專案

[這裡是官方安裝說明文件](https://nuxtjs.org/guide/installation)，根據我的個人經驗，你每個禮拜都會得到不一樣的錯誤訊息(感覺他們工程師根本沒有qa就直接上正式版本了…)，以下為我操作的及遇到的錯誤及相關解決方案。

創建nuxt專案，project-name請自由發揮

```
$ yarn create nuxt-app <project-name>

```

接下來會問你一堆問題，根據我之後會撰寫的文章類型，你可以跟我做一樣的設定

只要你運氣夠差，你就會遇到這個錯誤：

```
throw new SAOError(`Failed to install ${packageName} in ${cwd}`)

```

別怕，仔細看錯誤訊息，你會發現是因為local端的node版本過低，最新的nuxt需要你的[node版本](https://nodejs.org/en/)保持在13.7以上，建議你可以使用[nvm這個套件](https://github.com/nvm-sh/nvm#install--update-script)來做local端node的版本控制

調整完node版本後再下一樣建立nuxt的指令

```
$ yarn create nuxt-app <project-name>

```

這次就會通過嚕～接著下指令確認使否正常跑

```
cd nuxt_baobao
yarn dev

```

如果成功出現下面的畫面就恭喜你嚕～

  [http://localhost:3000/](http://localhost:3000/)

SETP 2/5 : 將nuxt專案build起來

因為實際運行你總不可能把整個專案丟到GCP，所以要下指令build這個專案，那他變小一點

```
yarn build

```

如果沒發生意外應該會build失敗(我真的懷疑他們到底有沒有做過上線測試…)

因為很多人遇到這個問題，有興趣可以參考[這篇文章](https://github.com/postcss/autoprefixer/issues/1328#issuecomment-647279513)，不然就複製下面指令執行

```
nvm use 14.4
yarn add autoprefixer@9.8.1

```

安裝完套件再下一次yarn build指令就能順利完成

SETP 3/5 : 開啟GCP App Engine

我建議你先建立一個[GCP新的專案](https://console.cloud.google.com/cloud-resource-manager)來做這次的操作，建立好專案去App Engine建立應用程式

  地區請選擇離自己近的，價格會便宜一點

SETP 4/5 : 下載Google Cloud SDK

如果你可以根據[官方文件](https://cloud.google.com/sdk/docs/quickstarts)操作，裡面有提供各式各樣的安裝方法，一開始我是直接下載壓縮檔來安裝，但遇到各種悲劇…

```
Welcome to the Google Cloud SDK!
ERROR:root:code for hash md5 was not found.

```

然後網路上你查到[大部分的答案](https://stackoverflow.com/questions/25372911/python-pip-error-on-osx)都是唬爛你的

所以我個人建議你使用[homebrew](https://brew.sh/zh-tw/)安裝吧

```
brew cask install google-cloud-sdk

```

安裝後你可以嘗試看看

```
gcloud init

```

如果不發生意外應該又是一大串的錯誤…這個時候的問題通常是python版本的問題，請指定你執行gcp的python版本

```
export CLOUDSDK_PYTHON=python3

```

之後再執行gcloud init應該就是一路通暢了，裡面會有一些讓你登入以及選擇專案的選項就按造個人需求調整即可

SETP 5/5 : deploy Nuxt專案到GCP App Engine

如果你上面的操作都很順利，你接下來就可以嘗試把本地的nuxt專案發布到GCP上面了，首先你要在nuxt專案做以下調整，這裡是[官方文件](https://nuxtjs.org/faq/appengine-deployment/)(跟著照做會錯誤)：

新增app.yaml檔案，這個檔案是在deploy的時候使用的

PS.在這裡我要特別提醒一件事情，一但你的環境(env)設定為flex，你醒來的時候帳單就有可能爆掉，所以這個部分請謹慎，這是我端午連假完打開GCP帳單發現的慘痛經驗，使用標準環境才會有免費配額，但還是會扣一點費用(ex: Google Storage、Could Build…)。

```
runtime: nodejs12

instance_class: F1

automatic_scaling:
  max_instances: 1

handlers:
  - url: /_nuxt
    static_dir: .nuxt/dist/client
    secure: always

  - url: /(.*\.(gif|png|jpg|ico|txt))$
    static_files: static/\1
    upload: static/.*\.(gif|png|jpg|ico|txt)$
    secure: always

  - url: /.*
    script: auto
    secure: always

env_variables:
  HOST: '0.0.0.0'
  NODE_ENV: 'production'

```

原始碼：[nuxt_baobao_app.yaml](https://gist.github.com/dean9703111/6b739e200a284d085377d96555e2d171)（GitHub Gist）

調整package.json檔案，要在dependencies加上“@nuxtjs/vuetify”，並加上這個專案nodejs的版本(本範例為14.4)

```
{
  "name": "nuxt_baobao",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "nuxt",
    "build": "nuxt build",
    "start": "nuxt start",
    "export": "nuxt export",
    "serve": "nuxt serve",
    "lint:js": "eslint --ext .js,.vue --ignore-path .gitignore .",
    "lint": "yarn lint:js"
  },
  "dependencies": {
    "@nuxtjs/axios": "^5.11.0",
    "@nuxtjs/vuetify": "^1.11.2",
    "autoprefixer": "9.8.1",
    "chart.js": "^2.9.3",
    "hchs-vue-charts": "^1.2.8",
    "nuxt": "^2.13.0",
    "vue-chartjs": "^3.5.0"
  },
  "devDependencies": {
    "@nuxtjs/eslint-config": "^3.0.0",
    "@nuxtjs/eslint-module": "^2.0.0",
    "@nuxtjs/vuetify": "^1.11.2",
    "babel-eslint": "^10.1.0",
    "eslint": "^7.2.0",
    "eslint-config-prettier": "^6.11.0",
    "eslint-plugin-nuxt": "^1.0.0",
    "eslint-plugin-prettier": "^3.1.4",
    "prettier": "^2.0.5"
  },
  "engines": {
    "node": "^12"
  }
}

```

原始碼：[nuxt_baobao_package.json](https://gist.github.com/dean9703111/e4436bcdb26e49a6bb1be8afc98d59a9)（GitHub Gist）

完成以上設定後在專案目錄下指令

```
gcloud init

```

這裡會讓你再次確認自己的設定檔(是否初始化、新增)，登入的google帳號，選擇雲端專案(就是你剛剛新增的那個專案)

在完成上述設定後就可以輸入以下指令來deploy啦(這裡會讓你在確認一下deploy發布的專案位置)

```
gcloud app deploy

```

如果你看到以下畫面就要恭喜你了

輸入下面指令可以檢視你的網頁是否發布成功嚕～

```
gcloud app browse

```

  [https://nuxt-demo-281302.dt.r.appspot.com](https://nuxt-demo-281302.dt.r.appspot.com)

如果有問題歡迎在下方回覆，如果這篇文章有讓你少走彎路請用力拍手喔～

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202006/nuxtjs-gcp-serverless/
