# 帶你將 Node.js Web App 給 Docker 化，並用 Kubernetes 部署的超簡單範例

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202201/nodejs-docker-kubernetes-deploy/
- 發布：2022-01-21
- 主題：技術教學
- 標籤：Node.js、Docker、Kubernetes、教學範例
- 摘要：公司很早就導入了 Kubernetes（K8s），主要是為了自動化部署以及管理、監測多台機器上的 Container，在遇到高併發情境時可以自動擴展。過去我都是以輔助者的角色跟 DevOps 說我希望有哪些功能，所以便藉著寫這篇文章的機會來初探這個已經成為業界潮流的技術。

公司很早就導入了 Kubernetes（K8s），主要是為了自動化部署以及管理、監測多台機器上的 Container，在遇到高併發情境時可以自動擴展。過去我都是以輔助者的角色跟 DevOps 說我希望有哪些功能，所以便藉著寫這篇文章的機會來初探這個已經成為業界潮流的技術。

```
文章大綱
一、安裝 Docker、kubectl、minikube 等工具
  ➤ 安裝 Docker
  ➤ 安裝 kubectl
  ➤ 安裝 minikube
二、建立 Node.js Web App
  ➤ 建立「package.json」
  ➤ 建立「server.js」
  ➤ 運行 Node.js Web App
三、將 Node.js Web App 給 Docker 化
  ➤ 建立「Dockerfile」
  ➤ 建立「.dockerignore」
  ➤ 製作 Docker Image
  ➤ 印出建立的 Images
  ➤ 將建立好的 Docker Image 跑起來
  ➤ 確認 Docker Image 運行狀態，並進入網址確認畫面如預期
四、撰寫 K8s 使用的 yaml 檔＆部署 K8s
  ➤ K8s 的 yaml 檔基礎說明
  ➤ K8s 部署
  ➤ 查看部署後 pods 狀態
  ➤ 取得 deployment 的外部 ip，確認是否會顯示網頁
  ➤ 刪除執行中的 K8s
  ➤ 關閉 minikube

```

> 本篇文章是在 Mac 的作業系統下操作的

## 一、安裝 Docker、kubectl、minikube 等工具

### 安裝 Docker

前往[官網下載 Docker 安裝檔](https://docs.docker.com/desktop/mac/install/)，安裝過程就是狂按下一步，這裡就不再多做介紹，下載後要把 App 打開依照步驟執行才算是完成安裝喔！

  這邊建議順便註冊一組 Docker 帳號，方便你日後的操作。

### 安裝 kubectl

安裝他才能操作 K8s 喔！

```
# 安裝 kubectl
brew install kubectl
# 確認是否安裝成功
kubectl version --client
# 查看目前 cluster 運行狀態
kubectl get all

```

### 安裝 minikube

這個工具可以讓你在本機(Local)架設 K8s 叢集(Cluster)喔！

```
# 安裝 minikube
brew install minikube
# 啟動 minikube，啟動 k8s cluster
minikube start
# 查看 minikube 狀態
minikube status
# 取得目前 minikube 的 IP address
minikube ip

```

## 二、建立 Node.js Web App

> 你也可以直接去[筆者的 github](https://github.com/dean9703111/nodejs-docker-k8s-auto-scaling-example)，直接將專案 Clone 到本機來做測試。

### 建立「package.json」

```
{
  "name": "docker_web_app",
  "version": "1.0.0",
  "description": "Node.js on Docker",
  "author": "Dean Lin",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.17.2"
  }
}

```

原始碼：[node_web_app.json](https://gist.github.com/dean9703111/dba0f27584d5c0d7f778aa7cacb1d4b3)（GitHub Gist）

### 建立「server.js」

```
"use strict";

const express = require("express");

// Constants
const PORT = 8080;
const HOST = "0.0.0.0";

// App
const app = express();
app.get("/", (req, res) => {
  res.send("Hello World");
});

app.listen(PORT, HOST);
console.log(`Running on http://${HOST}:${PORT}`);

```

原始碼：[node_web_app.js](https://gist.github.com/dean9703111/0e82ccc37a3ae3cb68e78e047464c1ec)（GitHub Gist）

### 運行 Node.js Web App

在終端機先輸入`yarn`將套件安裝，再輸入`yarn start`確認專案可以運行。

  如果你是 Clone 筆者專案，記得要 cd 進入 docker_web_app 裡面再安裝＆執行喔！

  看到 Hello World 就恭喜完成第一步嚕

## 三、將 Node.js Web App 給 Docker 化

### 建立「Dockerfile」

填寫製作 Docker Image 時需要參考的環境＆指令。

```
FROM node:14

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install
# If you are building your code for production
# RUN npm ci --only=production

# Bundle app source
COPY . .

EXPOSE 8080
CMD [ "node", "server.js" ]

```

原始碼：[Dockerfile](https://gist.github.com/dean9703111/b4ed238746412ea272addb3e7667c1ee)（GitHub Gist）

### 建立「.dockerignore」

填寫製作 Docker Image 時不想要複製的檔案。

```
node_modules
npm-debug.log

```

原始碼：[.dockerignore](https://gist.github.com/dean9703111/346e2e3876af9dbd6e8ee568b1b72bc5)（GitHub Gist）

### 製作 Docker Image

```
# 登入 Docker
docker login
# ！如果 Docker app沒有開啟就執行下面步驟會報錯！
# 建立 Docker Image
docker build . -t <your username>/node-web-app
# 筆者範例指令
docker build . -t babydragon9703111/node-web-app

```

### 印出建立的 Images

```
docker images

```

### 將建立好的 Docker Image 跑起來

```
# 裡面填寫的 port 是本地端要跑的（對外 port）
docker run -p [port]:8080 -d <your username>/node-web-app
# 筆者範例指令
docker run -p 8080:8080 -d babydragon9703111/node-web-app

```

### 確認 Docker Image 運行狀態，並進入網址確認畫面如預期

```
# Get container ID
docker ps

# Print app output
docker logs <container id>

```

  看到 Hello World 就距離成功非常接近嚕！

## 四、撰寫 K8s 使用的 yaml 檔＆部署 K8s

### K8s 的 yaml 檔基礎說明

- **apiVersion**：API 版本，需要依照 kind 的物件來決定 version。
- **kind**：建立的物件。
- **metadata**：描述物件及標籤。
- **spec**：物件要的功能。

建立`nodetest-k8s.yaml`來做測試範例，裡面的 image 等相關資訊記得算成自己的

```
apiVersion: v1
kind: Service
metadata:
  name: node-app-service
spec:
  selector:
    app: node-web-app
  ports:
    - protocol: "TCP"
      port: 8080
      targetPort: 8080
  type: LoadBalancer

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-web-app
spec:
  selector:
    matchLabels:
      app: node-web-app
  replicas: 3
  template:
    metadata:
      labels:
        app: node-web-app
    spec:
      containers:
        - name: node-web-app
          image: babydragon9703111/node-web-app:latest
          imagePullPolicy: Always
          ports:
            - containerPort: 8080

```

原始碼：[nodetest-k8s.yaml](https://gist.github.com/dean9703111/fbdb562caf70b78ee7801d377c546e12)（GitHub Gist）

### K8s 部署

```
# 啟動 k8s cluster
minikube start

# 執行部署
kubectl apply -f nodetest-k8s.yaml

```

### 查看部署後 pods 狀態

```
# 此指令會查 default namespace 的資源
kubectl get pods

```

因為我們在 spec 的 replicas 的值設為 3，所以會產生 3 個 pods。

### 取得 deployment 的外部 ip，確認是否會顯示網頁

```
minikube service node-app-service --url

```

  如果網頁有正確顯示就大功告成啦～

### 刪除執行中的 K8s

```
# 指定刪除剛剛建立的 K8s
kubectl delete -f nodetest-k8s.yaml

```

剛下達指令刪除時，在 STATUS 的部分會顯示 Terminating，過一段時間就會消失嚕～

### 關閉 minikube

```
# 如果沒關閉的話，你會發現電腦燙燙的，CPU 居高不下
minikube delete

```

> 以上就是 Node.js X Docker X Kubernetes 超簡單範例啦！希望讀者可以透過這篇文章對這些技術有基礎認識～

> 如果想更近一步了解 K8s Auto Scaling 的技術，歡迎閱讀筆者寫的這篇文章：「[模擬 K8s Auto Scaling（可在 Local 端測試）](https://deanlin.net/blog/202201/kubernetes-autoscaling-local/)」

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202201/nodejs-docker-kubernetes-deploy/
