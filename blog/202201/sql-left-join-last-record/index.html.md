# SQL LEFT JOIN only last record of right table — 4 種解決方案

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202201/sql-left-join-last-record/
- 發布：2022-01-12（更新：2023-05-05）
- 主題：技術教學
- 標籤：SQL、LEFT JOIN、Last Record、Right Table、Solutions
- 摘要：最近工作時剛好遇到了一個需求：「需要取得每位使用者最後的工作記錄。」使用者是一張 table，工作記錄又是ㄧ張 table；原本以為這個簡單的需求用個 leftJoin 就能瞬間搞定，結果卻搞了自己快一個晚上…因此決定把解決方案放上來，給有遇到相同問題朋友們。

最近工作時遇到了一個需求：「需要取得每位使用者最後的工作記錄。」使用者是一張 table，工作記錄又是ㄧ張 table；原本以為這個簡單的需求用個 leftJoin 就能瞬間搞定，結果卻搞了自己快一個晚上…因此決定把解決方案放上來，給有遇到相同問題朋友們。

```
文章大綱
一、使用線上 SQL 語法測試網站來理解＆學習這個知識
二、生成 users、user_logs 兩張 table
三、撰寫可以找出 right table「所有欄位」的 SQL
  ➤ 用 table alias name 來內部比對取得最大值
  ➤ 用 max 函式來找出最大值
四、撰寫找出 right table「最後 updated_at」的 SQL
  ➤ 用 GROUP BY 的概念搭配 MAX 函式篩選出最大值
  ➤ 用 Laravel 現成函式 subquery-joins 找出最大值

```

## 一、使用線上 SQL 語法測試網站來理解＆學習這個知識

考慮到讀者未必手上都剛好有能夠運行的資料庫環境，所以這邊推薦兩個實用的線上 SQL 語法測試網站([SQL fiddle](http://sqlfiddle.com/)、[MySQL Online](https://paiza.io/en/projects/new?language=mysql))

如果不知道如何使用，可以參考我之前的文章：「[線上 SQL 語法測試網站(SQL fiddle、MySQL Online)](https://deanlin.net/blog/202102/online-sql-playgrounds/)」

## 二、生成 users、user_logs 兩張 table

```
CREATE TABLE users (
  id INT,
  name varchar(50)
);
CREATE TABLE user_logs (
  id INT,
  user_id INT,
  log varchar(50),
  updated_at timestamp
);

INSERT INTO users VALUES (1, 'John');
INSERT INTO users VALUES (2, 'Yannis');
INSERT INTO users VALUES (3, 'Victor');
INSERT INTO users VALUES (4, 'Jimmy');
INSERT INTO users VALUES (5, 'Richard');

INSERT INTO user_logs VALUES (1, 1 ,"John 工作記錄 1","2022-01-01 00:00:00");
INSERT INTO user_logs VALUES (2, 1 ,"John 工作記錄 big","2022-01-09 00:00:00");
INSERT INTO user_logs VALUES (3, 1 ,"John 工作記錄 2","2022-01-05 00:00:00");
INSERT INTO user_logs VALUES (4, 1 ,"John 工作記錄 3","2022-01-06 00:00:00");
INSERT INTO user_logs VALUES (5, 4 ,"Jimmy 工作記錄 1","2022-01-01 00:00:00");
INSERT INTO user_logs VALUES (6, 4 ,"Jimmy 工作記錄 2","2022-01-03 00:00:00");
INSERT INTO user_logs VALUES (7, 4 ,"Jimmy 工作記錄 big","2022-01-05 00:00:00");
INSERT INTO user_logs VALUES (8, 5 ,"Richard 工作記錄 big","2022-01-06 00:00:00");
INSERT INTO user_logs VALUES (9, 5 ,"Richard 工作記錄 1","2022-01-05 00:00:00");

```

原始碼：[init_users_user_logs.sql](https://gist.github.com/dean9703111/3fa4de5f05238eda29b1d2caa820870a)（GitHub Gist）

## 三：撰寫可以找出 right table「所有欄位」的 SQL

### ➤ 用 table alias name 來內部比對取得最大值

```
SELECT t1.name,
       t2.log,
       t2.updated_at
FROM `users` t1
LEFT JOIN
  (SELECT t.*
   FROM user_logs t
   WHERE NOT EXISTS
       (SELECT 1
        FROM user_logs
        WHERE user_id = t.user_id
          AND updated_at > t.updated_at ) ) t2 ON t2.user_id = t1.id
ORDER BY t1.id

```

原始碼：[last_record_method1.sql](https://gist.github.com/dean9703111/aab92233de2baf5232477ad86ec6fcb2)（GitHub Gist）

  使用[SQL fiddle](http://sqlfiddle.com/) 線上編譯的結果

### ➤ 用 max 函式來找出最大值

```
SELECT users.name,
       user_logs.log,
       user_logs.updated_at
FROM users
LEFT JOIN user_logs ON `users`.`id` = `user_logs`.`user_id`
AND `user_logs`.`updated_at` =
  (SELECT max(updated_at)
   FROM user_logs
   WHERE user_logs.user_id = users.id)
ORDER BY users.id

```

原始碼：[last_record_method2.sql](https://gist.github.com/dean9703111/3f4ad3b36da6c68921c002d04cf89734)（GitHub Gist）

  使用[SQL fiddle](http://sqlfiddle.com/) 線上編譯的結果

**特別提醒**：上述方案在同個 user 的 user_logs 有[年月日時分秒]完全相同的「updated_at」時，會跳出複數筆資料；但此情境在正常環境中不會發生，若想避免此狀況，可以使用 distinct 函式來將 user 過濾。

## 四、撰寫找出 right table「最後 updated_at」的 SQL

有時我們只是要找出使用者最後更新的日期（updated_at），就不必大費周章抓出所有欄位，下面也提供兩個方案。

### ➤ 用 GROUP BY 的概念搭配 MAX 函式篩選出最大值

```
SELECT name,
       updated_at
FROM `users`
LEFT JOIN
  (SELECT user_id,
          MAX(updated_at) AS updated_at
   FROM `user_logs`
   GROUP BY `user_id`) AS `user_logs` ON `users`.`id` = `user_logs`.`user_id`

```

原始碼：[last_record_method3.sql](https://gist.github.com/dean9703111/b18a73be311b276ea0f104258c88d573)（GitHub Gist）

  使用[SQL fiddle](http://sqlfiddle.com/) 線上編譯的結果

### ➤ 用 Laravel 現成函式 [subquery-joins](https://laravel.com/docs/8.x/queries#subquery-joins) 找出最大值

```
$latestLogs = DB::table('user_logs')
                ->select('user_id', DB::raw('MAX(updated_at) as last_log_created_at'))
                ->groupBy('user_id');

$users = DB::table('users')
        ->joinSub($latestLogs, 'latest_logs', function ($join) {
            $join->on('users.id', '=', 'latest_logs.user_id');
        })->get();

```

原始碼：[last_record_method4.php](https://gist.github.com/dean9703111/c3f98ae4141dd457ae25e9c2feeaf0d3)（GitHub Gist）

希望今天這篇文章有幫助讀者解決工作或是專案上的需求喔～

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202201/sql-left-join-last-record/
