# Apache Bench — 壓力測試好夥伴

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202110/apache-bench-load-testing/
- 發布：2021-10-27（更新：2023-05-05）
- 主題：技術教學
- 標籤：Apache Bench、壓力測試、macOS、實用指令、重要參數
- 摘要：最近在測試 K8s 在大流量時 pod 的 auto scaling 機制，使用的工具是 Mac 本身內建 Apache Bench，這篇文章就來介紹它的基礎使用方式。

最近在測試 K8s 在大流量時 pod 的 auto scaling 機制，使用的工具是 Mac 本身內建 Apache Bench，這篇文章就來介紹它的基礎使用方式。

```
文章大綱
一、基礎操作
二、理解重要回傳資訊
三、使用 POST request
四、測試注意事項

```

## 一、基礎操作

下面是以 2 個連線數（c）建立 10 個 requests（n）來存取 Google 網址的範例。

```
ab -n 10 -c 2 https://www.google.com/

```

需要特別注意的是 request 數量不得低於連線數，否則會直接噴如下的錯誤：

通常來說，requests 數量會是連線數量的 100 倍。

## 二、理解重要回傳資訊

順利執行後會印出如下資訊，下面挑幾個重要資訊來做說明：

- **Concurrency Level**：我們設定的連線量。
- **Failed requests/Complete requests**：我們需要知道在壓力測試時失敗的百分比。
- **Requests per second**：平均每秒可回應的 Request 數量。
- **Time per request**：平均每個 Request 的時間。
- **Connection Times (ms)**：可以從這裡觀察連線的狀況。

## 三、使用 POST request

有時會需要用 POST 傳送參數以及在 Header 帶入 Token 資訊，下面提供一個簡單範例：

```
ab -n 10000 -c 100 -p post.txt -T application/json -H 'Authorization: Token your_token' https://your_test_ip/

```

- **-p**：這個是要傳送 data 的文檔路徑。
- **-T**：傳送資料時的 Header 的 Content-type。
- **-H**：放在 Header 的資訊。

post.txt 裡面只需要以 json 格式呈現即可：

```
{
  "username":"baobao",
  "password":"baobaoverycute",
  "action":"login"
}

```

### 四、測試注意事項

- 連線量請不要一開始就 100 個，請由小而大遞增；否則最後的測試數據可能不準確。
- 測試時請將 Server 與 Client 放在相同的網域；若不同網域來做測試就可能會受到頻寬影響。
- 測試主要是為了確認 Server 可以承受的負載，盡可能避免其他干擾因素。

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202110/apache-bench-load-testing/
