# PHP 陣列操作：那些讓你意外的小陷阱(array_unique、array_filter)

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202310/php-array-pitfalls/
- 發布：2023-10-18
- 主題：技術教學
- 標籤：PHP、JavaScript、Array Methods、Exception、Code
- 摘要：我發現許多從其他程式語言轉來 PHP 的開發者，經常會在操作陣列時碰到意料之外的狀況（我就是受害者）。今天我要來談談 PHP 中的 array_unique 和 array_filter 函數，以及它們的行為與你想像中可能存在的差異。

我發現許多從其他程式語言轉來 PHP 的開發者，經常會在操作陣列時碰到意料之外的狀況（我就是受害者）。今天我要來談談 PHP 中的 `array_unique` 和 `array_filter` 函數，以及它們的行為與你想像中可能存在的差異。

## 🎭 Array_filter 的索引行為

在 JavaScript 的世界中，我們用 `filter` 來過濾陣列，結果非常直觀。

```
let arr = [1, 2, 3, 4];
let filteredArr = arr.filter(value => value > 2);
console.log(filteredArr); // 看吧！直接返回 [3, 4]

```

但換到 PHP，`array_filter` 卻有點不一樣。使用它之後，你會發現原始的索引還在：

```
$arr = array(1, 2, 3, 4);
$filteredArr = array_filter($arr, function($value) {
    return $value > 2;
});
print_r($filteredArr); // 喔喔！結果是 Array ( [2] => 3 [3] => 4 )

```

**這意味著如果你嘗試在過濾後的陣列上取得 `$filteredArr[0]`，會得到一個未定義的索引錯誤。**

## 🎩 Array_unique 的特殊之處

`array_unique` 函數在 PHP 中用於移除陣列中的重複值。

但小心！它也不會重新排列索引：

```
$arr = array(1, 2, 2, 3, 3, 4);
$uniqueArr = array_unique($arr);
print_r($uniqueArr); // 哇！這是什麼魔法？Array ( [0] => 1 [1] => 2 [3] => 3 [5] => 4 )

```

同樣的，如果你直接取得 `$uniqueArr[2]`會讓你再次掉入那恐怖的陷阱中！

## 🌈 如何應對？

為了避免這些問題，建議在使用這些函數後，用 `array_values` 函數重設索引：

```
$arr = array(1, 2, 3, 4);
$filteredArr = array_filter($arr, function($value) {
    return $value > 2;
});
$reIndexedArr = array_values($filteredArr);
print_r($reIndexedArr); // Array ( [0] => 3 [1] => 4 )

```

這樣，你就可以放心地存取 `$reIndexedArr[0]` 而不必擔心會遭遇未定義的索引。

## 🎉 總結

雖然 PHP 的陣列操作函數在某些方面與其他語言有所不同，但只要你了解這些差異並採取適當的預防措施，就可以順利避免可能的錯誤。

希望這篇文章能夠幫助你更深入地了解 PHP 的陣列操作 🚀

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202310/php-array-pitfalls/
