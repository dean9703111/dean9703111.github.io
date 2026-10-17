# 在 Mac 將 Nuxt.js 專案 Build 成 Docker Image 發佈到 dockerhub

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202104/nuxtjs-docker-image-dockerhub/
- 發布：2021-04-08
- 主題：技術教學
- 標籤：Nuxt.js、Docker、macOS、Docker Hub、Docker Image
- 摘要：每次聚會朋友都在聊用 Docker 來 deploy 有多潮，感覺再不研究就要落伍了；這篇文章會分享我將過去的 Nuxt 專案成功 Build 成 Docker Image 並 push 到 dockerhub 的步驟。

每次聚會朋友都在聊用 Docker 來 deploy 有多潮，感覺再不研究就要落伍了；這篇文章會分享我將過去的 Nuxt 專案成功 Build 成 Docker Image 並 push 到 dockerhub 的步驟；整體操作流程個人感覺非常簡單，大家不要被新的技術嚇到了，通常新技術的門檻比想像的更低一點。

```
一、事先準備
 ➤ 創建 Nuxt.js 專案
 ➤ 下載 & 安裝 Docker
二、在 local 把 Nuxt.js 專案 Build 成 Docker Image
 ➤ 撰寫 dockerfile & .dockerignore
 ➤ 把 Nuxt.js 專案 build 成 Docker Image
三、將 Docker Image 發佈到 dockerhub 上面

```

## 一、事先準備

### 創建 Nuxt.js 專案

> 如果你已經有 Nuxt.js 的專案可以跳過下面的操作流程

**STEP 1：**創建一個 Nuxt.js 專案

```
$ yarn create nuxt-app nuxtjs-tutorial

```

在創建過程中會問你一些問題，你可以參考筆者的選擇：

**STEP 2：**讓 Nuxt.js 專案跑起來

```
$ cd nuxtjs-tutorial
$ yarn dev

```

  看到這樣的狀況就是跑起來成功嚕

  開啟 [http://localhost:3000/](http://localhost:3000/) 可以順利看到畫面了喔！

### 下載 & 安裝 Docker

**STEP 1：**下載 Docker

**前往官網[下載 Docker 安裝檔](https://docs.docker.com/docker-for-mac/install/)**

> 安裝過程就是狂按下一步，就不在多做介紹，下載後要把 App 打開依照步驟執行才算是完成安裝喔！

**STEP 2：**註冊 Docker

在官網[註冊 docker 帳號](https://hub.docker.com/signup)

**STEP 3：**在 local 端登入 docker

```
$ docker login

```

輸入剛剛註冊的帳號密碼

## 二、在 local 把 Nuxt.js 專案 Build 成 docker image

### 撰寫 dockerfile & .dockerignore

**STEP 1：先用指令建立這兩個檔案**

```
$ touch dockerfile
$ touch .dockerignore

```

**STEP 2：撰寫 **dockerfile

```
# node 運行環境
FROM node:12.19.0

# 存放image的目錄
RUN mkdir -p /app
COPY . /app
WORKDIR /app

# 如果需要相關的環境變數請自行調整路徑
#COPY ./env/.prod-env /app/env/.prod-env

# 對外的port
ENV HOST 0.0.0.0
EXPOSE 3000

# 安裝套件並build專案
RUN yarn
RUN yarn build

# 執行專案
CMD [ "yarn", "start" ]

```

原始碼：[dockerfile](https://gist.github.com/dean9703111/647663b391da8292d80089d445f3e7e6)（GitHub Gist）

**STEP 2：撰寫 **.dockerignore

```
npm-debug.log
node_modules

```

原始碼：[.dockerignore](https://gist.github.com/dean9703111/3d2509de3aba92d14d42b530c2e654ed)（GitHub Gist）

### 把 Nuxt.js 專案 build 成 Docker Image

**STEP 1**：Build Docker Image

在專案目錄下輸入指令後耐心等待打包，**docker_example** 是這個 image 的名字

```
docker build -t docker_example .

```

**STEP 2**：Run Docker Image

輸入指令確認 image 是否成功

```
docker run -it — rm -p 80:3000 docker_example

```

> 特別注意『80 是對外 port，3000 是 container 的 port』，**docker_example **就是剛剛建立的 image 名稱

上圖 172.xx.xx.x:3000 的網址點開來是沒有畫面的，因為我們已經透過指令改變對外 port 到 80 了，在網址列上輸入 [http://localhost/](http://localhost/) 就能成功看到供果嚕～

## 三、將 Docker Image 發佈到 dockerhub 上面

**STEP 1**：給剛剛建立的 Image 一個 tag，username 是你 Doacker 的帳戶，xxximage是你希望這個 Image 在 dockerhub 上呈現的名字

```
docker tag docker_example username/xxximage

```

**STEP 2**：列出目前存在的 Images

```
docker images

```

**STEP 3**：把這個 Image 丟到 dockerhub 的 repositories

> 這個 push 的過程會根據你產生 Image 的大小決定，通常要等一段時間

```
docker push username/xxximage

```

**STEP 4**：在自己的 Docker 帳戶上看到剛剛上傳的 Docker Image

之後要部署這個 Docker Image 到其他 Server 就只需要輸入下面指令就能獲得嚕～

```
docker pull username/xxximage

```

希望這篇文章有幫助到使用 Mac 開發 Nuxt.js 專案的朋友們～

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202104/nuxtjs-docker-image-dockerhub/
