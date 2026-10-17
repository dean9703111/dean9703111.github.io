# 線上 SQL 語法測試網站(SQL fiddle、MySQL Online)

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202102/online-sql-playgrounds/
- 發布：2021-02-26（更新：2023-05-05）
- 主題：技術教學
- 標籤：MySQL、SQL Fiddle、MySQL Online、線上 SQL 語法測試網站、線上資料庫語法測試
- 摘要：當朋友問你 SQL 語法問題時如果沒有現成的資料庫真的很難幫上忙，並且也不確定自己的回答是不是真的能解決問題；今天就來介紹兩個我常用的 SQL 語法測試網站，並透過解說朋友遇到的題目讓大家快速了解網站的使用方式。

當朋友問你 SQL 語法問題時如果沒有現成的資料庫真的很難幫上忙，並且也不確定自己的回答是不是真的能解決問題；今天就來介紹兩個我常用的 SQL 語法測試網站，並透過解說朋友遇到的題目讓大家快速了解網站的使用方式。

```
一、SQL Fiddle 簡介
二、MySQL Online 簡介
三、題目：找出每個 department 中 score 最高的兩個人並以 score 進行排序
  1. 用 SQL Fiddle 解題
  2. 用 MySQL Online 解題

```

## 一、SQL Fiddle 簡介

**網址：[http://sqlfiddle.com/](http://sqlfiddle.com/)**

### 支援資料庫版本：

延伸閱讀：[Table](https://sheetsu.com/tables/507fea70b1)

### 介面說明：

- **選擇資料庫及版本**
- **DDL 區域**：CREATE TABLE, INSERT 都在這個地方輸入，按下『Build Schema』即可生成 Schema；`**要先生成Schema才能執行SQL**`
- **DML 區域**：資料庫操作語言區域
- **Result**：顯示執行結果

## 二、MySQL Online 簡介

**網址：[https://paiza.io/en/projects/new?language=mysql](https://paiza.io/en/projects/new?language=mysql)**

### 介面說明：

- **選擇測試語言**：除了 MySQL 外，他還能測試 C、Java、R、php…
- **程式碼區域**：他的DDL與DML是混合在一起的，所以要測試就全部貼上去
- **Result**：顯示執行結果

## 三、題目：找出每個 department 中 score 最高的兩個人，並符合輸出結果的排序

### ⚒️ 生成題目資料表的 SQL :

```
CREATE TABLE employee (
  id INT,
  name varchar(50),
  score INT,
  department_id  INT
);
CREATE TABLE department (
  id INT,
  name varchar(50)
);
INSERT INTO employee VALUES (1, 'John', 83,1);
INSERT INTO employee VALUES (2, 'Yannis', 88,2);
INSERT INTO employee VALUES (3, 'Victor', 83,1);
INSERT INTO employee VALUES (4, 'Jimmy', 83,3);
INSERT INTO employee VALUES (5, 'Richard', 88,1);
INSERT INTO employee VALUES (6, 'Hens', 82,2);
INSERT INTO employee VALUES (7, 'Peter', 89,2);
INSERT INTO employee VALUES (8, 'Brian', 77,1);
INSERT INTO employee VALUES (9, 'Kelvin', 90,3);
INSERT INTO employee VALUES (10, 'Ben', 80,3);

INSERT INTO department VALUES (1, 'Sales');
INSERT INTO department VALUES (2, 'Operator');
INSERT INTO department VALUES (3, 'Development');

```

原始碼：[employee_score_question.sql](https://gist.github.com/dean9703111/1a1e64627d699759e01e8f15fdf50518)（GitHub Gist）

### 目標輸出結果：

延伸閱讀：[Table](https://sheetsu.com/tables/339e86faf8)

這個題目表面上看起來很簡單，實際做起來會遇到一些沒想到的問題，有興趣的小夥伴建議自己先玩玩不要直接看答案，文章後面也會說明這兩種編輯器的最大差異

### 1. 用 SQL Fiddle 解題

你的 **SQL 版本會被限制在 5.6**，你需要在許多好用函數都無法使用的狀態下完成這個題目

**解題思路**：因為 5.6 並沒有 row_number() 這個函數，所以我們要自己製作出來；另外多利用 subquery 的概念讓解答符合題目的輸出結果。

  **解答的[原始碼在這裡喔！](https://gist.github.com/dean9703111/1cd40fc06a6d78bb592cb83b1b2422ce)**

### 2. 用 MySQL Online 解題

你的 **SQL 版本為 8.0**，你解題的重點便在於對新的函數是否熟悉

**解題思路**：MySQL 8.0 是一個重大更新，ROW_NUMBER 這個函數就能讓你以輕鬆的姿態搞定這個題目，如果你不熟悉可以[參考這份文檔](https://www.begtut.com/mysql/mysql-row-number-function.html)。

  **解答的[原始碼在這裡喔！](https://gist.github.com/dean9703111/b4bbcc0ff66d2544aeee18c89f9f63ad)**

我的解答僅供參考(尚未優化)，有興趣的朋友可以在底下留言分享你的做法喔～希望這篇文章能幫助臨時需要寫 SQL 的朋友

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202102/online-sql-playgrounds/
