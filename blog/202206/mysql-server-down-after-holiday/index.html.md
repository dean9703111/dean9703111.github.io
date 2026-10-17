# 放完假後 MySQL Server 就掛了，不是每個錯誤都能靠 Google 找到解答

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202206/mysql-server-down-after-holiday/
- 發布：2022-06-21（更新：2023-05-05）
- 主題：技術教學
- 標籤：MySQL Server、Ubuntu Server、APT、Version Control、MySQL
- 摘要：如果你用上圖的錯誤訊息去 Google 搜尋，相信會得到很多解答；但這篇文章遇到的問題，如果用關鍵字搜尋是找不到真正解答的；也因為這次的經驗讓我對 Ubuntu Server、MySQL、apt-mark 有更深一層的認識。

如果你用上圖的錯誤訊息去 Google 搜尋，相信會得到很多解答；但這篇文章遇到的問題，如果用關鍵字搜尋是找不到真正解答的；也因為這次的經驗讓我對 Ubuntu Server、MySQL、apt-mark 有更深一層的認識。

```
大綱
ㄧ、問題描述
二、關鍵字搜尋到的方案
三、從 Error Log 猜想問題
四、問題的真實原因
五、解決方案
六、後記

```

## ㄧ、問題描述

上週建立的中介資料庫在經歷一個假日後，在禮拜一被同事回報登入失敗。

一開始我以為是帳號密碼錯誤，又或是公司網路問題；但在成功登入 Server 後，我發現連 root 的權限都無法登入 MySQL DB，並出現如下錯誤：

```
ERROR 2002 (HY000): Can’t connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' (2)

```

## 二、關鍵字搜尋到的方案

遇事不知求 Google，我馬上搜尋過去前輩們是怎麼解決這個問題的。

解決的方法五花八門，我這邊統整一下大致的方向：

- 重啟 MySQL 服務
- 調整 MySQL 資料夾權限，補齊遺漏的檔案
- 重啟整台 Server
- 把 MySQL 砍掉重裝

但很遺憾的，1~3 我都嘗試過了，MySQL 依舊頑強的吐一樣的錯誤，看來不能依靠網路的的解法了。

## 三、從 Error Log 猜想問題

因為網路上的方案都失效，所以決定換一個問題處理的方向，去找 MySQL 的 Error Log（其實一開始應該就要這樣做）。

**STEP 1**：前往 MySQL 的設定檔。

```
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf

```

**STEP 2**：查看 Error Log 存放路徑。

**STEP 3**：查看 Error Log 內容。

```
sudo vim /var/log/mysql/error.log

```

從時間軸發現服務被完整的關閉後就無法重啟，但假日有誰會把服務給關閉呢？

於是我便詢問公司的 MIS 部門，在假日是否有對 Server 進行維護作業。

果然如我所料，對方表示在假日有對各 Server 系統做例行性維護升級，而這台 Ubuntu Server 的 MySQL 正是在這次升級後出問題。

## 四、問題的真實原因

既然問題是由系統升級引起的，那就比較有解決的方向了；在與 MIS 同仁討論後，我們猜想問題應該是 Ubuntu Server 升級後內建的 MySQL 版本為 8.0，但原本安裝的版本為 5.7，因此在升級後發生版本對應不到的問題。

於是我們便執行下面的指令來查看目前 apt 安裝的套件有哪些，以及對應的版本：

```
apt list --installed

```

結果如我們所想，系統更新後，新版本的 MySQL 真的把舊版本給覆蓋住了。

## 五、解決方案

但因為這台是給其他合作單位使用的資料庫，而對方因為總總歷史包袱只能選用 MySQL 5.7，所以我們是不能對它做升級的。

但幸好 MIS 在維護前有先將 Server 備份，因此我們如下步驟進行處理：

**STEP 1**：用備份將原本的 Ubuntu Server 還原。

**STEP 2：**下指令將 MySQL Server 版本鎖定。

```
apt-mark hold mysql-server

```

**STEP 3**：查看是否有鎖定成功。

```
apt-mark showhold

```

**STEP 4：**鎖定後再將 Ubuntu Server 做升級維護。

**STEP 5：**升級完成後確認 MySQL 版本有鎖定住。

```
mysql --version

```

透過上面的步驟後，MySQL 就成功復活了～

## 六、後記

這個問題花了不少時間才解決，主要是因為我在一開始判斷問題時就走偏了；過去我都是從錯誤訊息找答案，而這次的錯誤訊息卻把我引導到另一個錯誤的方向。

這次的經驗也讓我明白，除了發生問題的本體外，還要多觀察其他有「相依性」的資訊，否則會陷入死胡同，還期待用錯誤的方法得到正確的結果。

但同時也提醒大家，千萬不要看完這篇文章後，回去就把所有安裝的套件版本都鎖死；除非有不得已的理由，不然系統、套件都是需要定期更新的，這樣穩定性、安全性才足夠。

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202206/mysql-server-down-after-holiday/
