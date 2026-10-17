# 正在為大數據所苦嗎？帶你了解 Kafka 這個分布式消息串流平台，以及它能解決什麼問題！

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202211/what-is-kafka/
- 發布：2022-11-20（更新：2023-05-05）
- 主題：技術教學
- 系列：Kafka 與分散式追蹤
- 標籤：Kafka、Big Data、Kafka Streams、Throughput、Pub Sub
- 摘要：如果推出的 Web 系統受到市場親睞，那隨著使用者成長，勢必會更高頻率的呼叫 api 與存取資料庫；假使我們沒有設計任何的緩衝機制，那資料庫不堪重負也只是時間問題。 而且隨著公司規模的擴張，接下來可能會有專門的團隊來分析使用者行為，公司高層與投資人會想看具體的業務狀況…

如果推出的 Web 系統受到市場親睞，那隨著使用者成長，勢必會更高頻率的呼叫 api 與存取資料庫；假使我們沒有設計任何的緩衝機制，那資料庫不堪重負也只是時間問題。

而且隨著公司規模的擴張，接下來可能會有專門的團隊來分析使用者行為，公司高層與投資人會想看具體的業務狀況，系統需要建立即時監控系統，甚至要整合外部資源…如果資料串流沒設計好，系統架構就會變成下面這張圖。

  image from [link](https://engineering.linkedin.com/distributed-systems/log-what-every-software-engineer-should-know-about-real-time-datas-unifying)

如果你覺得這個畫面很可怕，那不妨花點時間了解 Kafka 這門技術；本篇文章會帶讀者了解它的精神，以及選擇它的理由。

```
大綱

一、Kafka 具體能解決哪些問題，為什麼能解決？
二、Kafka 的基礎架構與階層關係
三、Kafka 為什麼吞吐量大？
四、Redis 也能夠處理 Pub/Sub，為什麼要用 Kafka？

```

本篇文章是筆者揉合許多大神的經驗後，再用自己理解的話語表達出來，相關的資源放在最下方，有興趣的讀者可以參考看看。

## 一、Kafka 具體能解決哪些問題，為什麼能解決？

**Kafka 可以讓資料以合適的時機出現在合適的地方，你可以把它想像成消息隊列（Message queue），生產者（Producer）會往隊列結尾塞東西，消費者（Consumer）從隊列依次拿東西。**

如果看完上面的解釋還是一頭霧水，相信下面「送包裹」的案例能幫你理解。

### 困境描述：

如果住在沒有管理員的公寓，包裹在上班時間送到你家門口，你就只能跟貨運司機說聲抱歉，因為沒人在家只能請他下次再送。

### 解決方案：

因為不是每個人都住在有管理員幫忙收包裹的大樓，為了解決這個問題，市場提出一個**解決方案：「超商取貨」**；從此以後，貨運司機不用跟你聯繫，把包裹送到超商即可，而你也不用在家裡等司機送貨，自己有時間的時候再去取貨就好。

「Kafka」就是扮演上述解法中「超商」的角色，這個方案的優勢在於：

- **降低耦合性**：如果收貨人不在家，那司機就算送到門口也沒人取貨，因此這趟運送的實際效益為「0」；改成超商取貨後，司機只管把包裹送到指定超商即可，不再依賴收貨人是否在現場。
- **非同步機制**：就算收貨人在家，司機到樓下後也要等人下來領包裹（有些人還慢吞吞，或提出各種奧客需求），這個等待期間司機也沒辦法繼續送貨，整個流程都被卡住了；但改成超商取貨後，司機把包裹送到超商就可以直接跑下一家，完全不用等待。
- **避免高峰期**：假使有人趁著雙 11 在不同平台訂貨，會造成送包裹的單位不同；如果他們剛好在同個時間到你家門口，收貨人在面對一堆等待簽收的司機時容易手忙腳亂。而超商取貨時，儘管包裹的數量是一樣的，但你可以舒服的領取。

## 二、Kafka 的基礎架構與階層關係

透過上面的故事，大家應該對 Kafka 消息轉發者（Message broker）的定位更為理解，下面再說明一下它的基礎架構與階層關係：

```
[Producer]  [Kafka cluster]                         [Consumer]
              ├── Broker1
Producer0->   │     ├── Topic A
              │     │     ├── Partition0(Leader)  -> Consumer Group A
              │     └── Topic B                         ├── Consumer0
              │           └── Partition0(Follower)      └── Consumer1
              └── Broker2
                    ├── Topic A
                    │     ├── Partition0(Follower)
                    └── Topic B
Producer1->               └── Partition0(Leader)  -> Consumer Group B
                                                        ├── Consumer0
                                                        └── Consumer1

```

- **Producer**：生產者，為消息的入口。
- **Kafka cluster**：Kafka 的集群，通常會有多個 Broker。
- **Broker**：Kafka 的實體，可以把它想像成每個 Broker 對應一台 Server。
- **Topic**：消息的主題，有點像是資料庫的 Table。
- **Partition**：Topic 的分區，一個 Topic 可以有多個分區。
- **Replication**：你在架構上會發現有些 Partition 是 Leader，有些是 Follower，當 Leader 故障時 Follower 就會上位；且 Leader 跟 Follower 會在不同的機器上，避免故障後一起陣亡。
- **Consumer**：消費者，為消息的出口。
- **Consumer Group**：可以將多個消費者組成一個消費群組，在 Kafka 的設計中，一個 Partition 的數據只允許消費群組中的某個消費者消費，消費群組中的消費者可以消費同一個 Topic 不同 Partition 的數據。

通常狀態下，**一個 Consumer Group 下的 Consumer 數量，建議與 Partition 的數量相同**；下面我們把 Partition 當成一碗飯，Consumer 比喻為一個人，這樣會更好理解為何要這樣設計：

- 可以出現一個人吃多碗飯的情況（但會吃太飽）。
- 不能出現多個人吃一碗飯的情況（禁止搶飯吃）。
- 如果人比飯碗多，那就得有人餓著。
- **一人一碗，剛剛好（理想設計）。**

## 三、Kafka 為什麼速度快、吞吐量大？

- **順序讀寫**：Kafka 將資料寫到硬碟上，通常我們都會覺得硬碟的讀寫效能不理想；但效能是否理想，是取決於`順序讀寫 or 隨機讀寫`，無論硬碟還是記憶體都是如此，有時硬碟順序讀寫的效能甚至高於記憶體隨機讀寫。

  image from [link](https://deliveryimages.acm.org/10.1145/1570000/1563874/jacobs3.jpg)

- **零拷貝**：從 Producer 到 Broker，Kafka 採用 mmap 的方式把資料持久化到硬碟（從 2 次 CPU 拷貝減為 1 次）；從 Broker 到 Consumer，Kafka 採用 sendFile 的方式發送硬碟資料（零拷貝）。
- **批量發送**：Producer 在發送消息時，可以等消息到固定數量後再一次發送（假設網路頻寬為 10MB/s，一次傳送 10MB 的消息會比 1KB 的消息分 10,000 次傳送快很多）。
- **批量壓縮**：有時系統的瓶頸不在 CPU 或是硬碟，而是在網路 IO；針對這個問題，我們可以在批量發送的基礎上，一次對多筆消息進行批量壓縮，以此降低 IO 負擔。

Kafka 的訊息傳送是基於 TCP 協定，擁有比 HTTP 更好的效能，並且比 UDP 更加安全。

## 四、Redis 也能處理 Pub/Sub，為什麼要用 Kafka？

筆者在網路上看到一個很有趣的[比喻](https://zhuanlan.zhihu.com/p/68052232)，在這裡做個分享：

老闆有個好消息要告訴大家，有兩個辦法:

- **到每個座位上挨個兒告訴每個人**。什麼？張三去上廁所了？那張三就只能錯過好消息了！
- **老闆把消息寫到黑板上，誰想知道就來看一下**。什麼？張三請假了？沒關係，我一周之後才擦掉，總會看見的！什麼？張三請假兩週？那就算了，我反正只保留一周，不然其他好消息沒地方寫了。

Redis 用第一種辦法，Kafka 用第二種辦法。

- Redis 是記憶體資料庫，它提供的 Pub/Sub 是將消息儲存於記憶體中，如果消息不用長久保存，並且消費速度快，那 Redis 很好用。
- Kafka 將資料儲存於硬碟上，為消息提供了持久化的服務，這樣的設定對消費者來說很好，他可以按造順序消費，也可以重新消費過去的消息（不過硬碟容量有限，還是會使用「空間 or 時間」作為淘汰策略）。

Redis 也能將資料持久化，但並不是那麼的可靠。

希望這篇文章的內容有讓讀者初步了解 Kafka 的應用，[下一篇文章](https://deanlin.net/blog/202211/nodejs-kafka-hands-on/)筆者會帶大家建立 Node.js 專案 ＆ Kafka 環境，透過[具體的實踐](https://deanlin.net/blog/202211/nodejs-kafka-hands-on/)來了解 Kafka 的運行邏輯。

參考文件：

- [再过半小时，你就能明白Kafka的工作原理了](https://zhuanlan.zhihu.com/p/68052232)
- [Kafka KRaft模式探索](https://www.cnblogs.com/smartloli/p/16430041.html)
- [Kafka解决了什么问题?(CSDN)](https://blog.csdn.net/liuzhenghui666666/article/details/106467057)
- [Kafka解决了什么问题?(知乎)](https://www.zhihu.com/question/53331259)
- [KafkaJS](https://kafka.js.org/)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202211/what-is-kafka/
