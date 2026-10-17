# 模擬 K8s Auto Scaling（可在 Local 端測試）

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202201/kubernetes-autoscaling-local/
- 發布：2022-01-21（更新：2023-05-05）
- 主題：技術教學
- 標籤：Kubernetes、Autoscaling、高併發、HPA
- 摘要：K8s 最吸引人的其中一個功能應該就是用 Auto Scaling 來應付高併發(High concurrency)情境，今天就用一個水平自動擴展(HPA)的範例來讓大家了解他的實際運作模式。

K8s 最吸引人的其中一個功能應該就是用 Auto Scaling 來應付高併發(High concurrency)情境，今天就用一個水平自動擴展(HPA)的範例來讓大家了解他的實際運作模式。

```
文章大綱
一、撰寫 Auto Scaling 的 yaml 檔
  ➤ HorizontalPodAutoscaler（水平自動擴展）參數說明
  ➤ 建立「nodetest-k8s-auto.yaml」來做 Auto Scaling 測試
二、調整 Service、Deployment 參數並加入 yaml 檔
  ➤ 調整 Service 參數
  ➤ 調整 Deployment 參數
  ➤ 將 Service、Deployment 設定合併到 nodetest-k8s-auto.yaml 中
三、部署＆測試 pod 是否會依據流量的增加自動擴展
  ➤ 開啟流量監控、部署 K8s
  ➤ 取得 Deployment 的外部 ip，然後瘋狂存取來測試 Auto Scaling
  ➤ 瘋狂存取後後，確認 maxReplicas 設定是否生效
  ➤ 關閉存取後，確認 minReplicas 設定是否生效
  ➤ 測試完成後將建立的 K8s 服務刪除、關閉 minikube

```

如果讀者尚未在本機安裝 K8s 環境，可以先閱讀：「[帶你將 Node.js Web App 給 Docker 化，並用 Kubernetes 部署的超簡單範例](https://deanlin.net/blog/202201/nodejs-docker-kubernetes-deploy/)」來設定基礎環境喔！

## 一、撰寫 Auto Scaling 的 yaml 檔

你也可以直接去[筆者的 github](https://github.com/dean9703111/nodejs-docker-k8s-auto-scaling-example)，直接將專案 Clone 到本機來做測試。

### HorizontalPodAutoscaler（水平自動擴展）參數說明

- **scaleTargetRef**：填寫你想要針對哪個 ReplicaSet 或 Deployment 或 Replication Controller 來做 Scaling。
- **minReplicas、maxReplicas**：Pod 擴展範圍。
- **targetCPUUtilizationPercentage**：Auto Scaling 是根據 CPU 用量來做擴展，判定的依據為 Deployment resources 的 requests 設定值（如果沒填寫基本上就不會動 XD）。

### 建立「nodetest-k8s-auto.yaml」來做 Auto Scaling 測試

```
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: node-web-app
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: node-web-app
  minReplicas: 1
  maxReplicas: 5
  targetCPUUtilizationPercentage: 10

```

原始碼：[nodetest-k8s-auto.yaml](https://gist.github.com/dean9703111/3626e36b2122a62142d94e740157f792)（GitHub Gist）

## 二、調整 Service、Deployment 參數並加入 yaml 檔

這裡的 Service、Deployment 都是從[先前這篇文章](https://deanlin.net/blog/202201/nodejs-docker-kubernetes-deploy/)改良過來的喔。

Docker image 可以 pull 筆者的範例，或是使用自己的都可以。

### 調整 Service 參數

- **nodePort**：這裡建議指定一個 port，如果沒有設定的話，最後對外 ip 的 port 為隨機產生。

### 調整 Deployment 參數

- **resources 的 requests**：如果 cpu 設定為 `100m`，而 targetCPUUtilizationPercentage 設定為 `10`；當 Pod 的 CPU 使用率為 `1m`（100m x 10%） 的時候就要進行 Auto Scaling。
- **replicas**：特別設定為 1 方便觀察，原則上他的值要跟 minReplicas 相同。

### 將 Service、Deployment 設定合併到 nodetest-k8s-auto.yaml 中

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
      nodePort: 30390
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
  replicas: 1
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
          resources:
            requests:
              cpu: 100m
              memory: 50Mi
            limits:
              cpu: 200m
              memory: 100Mi

```

原始碼：[nodetest-k8s-auto-extend.yaml](https://gist.github.com/dean9703111/18162c15146ad0bd116b07fa51bfa784)（GitHub Gist）

## 三、部署＆測試 pod 是否會依據流量的增加自動擴展

### 開啟流量監控、部署 K8s

```
# 將 minikube 監控流量的 addons 打開
minikube addons enable metrics-server

# 部署剛剛建立的 nodetest-k8s-auto.yaml
kubectl apply -f nodetest-k8s-auto.yaml

# 查看 Auto Scaler 狀態
kubectl get hpa

```

一開始 TARGETS 會顯示 `<unknown>`，這是因為還在判斷流量，過一陣子就會顯示百分比了

### 取得 Deployment 的外部 ip，然後瘋狂存取來測試 Auto Scaling

```
# 取得 Deployment 的外部 ip（此時會看到 port 與設定的 nodePort 相同）
minikube service node-app-service --url

# 開始瘋狂存取剛剛輸出的 url 來確認 pod 是否會自動成長
while true
do curl <Deployment 的外部 ip>
done

```

### 瘋狂存取後後，確認 maxReplicas 設定是否生效

看 REPLICAS 這個欄位是否會依據流量的增加自動擴展到設定的 maxReplicas。

### 關閉存取後，確認 minReplicas 設定是否生效

將上面的存取 Deployment 外部 ip 的 while 迴圈關掉後，pod 會自動減少到設定的 minReplicas。

### 測試完成後將建立的 K8s 服務刪除、關閉 minikube

```
kubectl delete -f nodetest-k8s-auto.yaml
# 如果沒關閉的話，你會發現電腦燙燙的，CPU 居高不下
minikube delete

```

以上就是模擬 K8s Auto Scaling 的測試方法啦！希望讀者可以透過文章了解這個技術的應用～

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202201/kubernetes-autoscaling-local/
