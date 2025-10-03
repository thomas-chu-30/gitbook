---
description: >-
  這個篇是要來說Array的操作，常常我們會到一些Array，裡面有包了一個Object，然後又要對這個Object作很多的事情，但是那一個操是最好的，那就要好好的來想一下，希望這篇對讀者有幫助。
---

# 8大陣列批次操作

這裡要介紹8個Array的操作方式

| Array 操作 |        |       |        |
| -------- | ------ | ----- | ------ |
| forEach  | filter | some  | sort   |
| map      | find   | every | reduce |

## forEach -- 初學好上手的forEach

* 將元素一個一個抓出來，帶入函數執行
* 不會產生新的Array

![由圖可知顯而易見，下面寫一個ppap的簡單範例](<../.gitbook/assets/截圖 2020-03-08 下午10.03.49.png>)

我們有一個`arr`，有四個產品，分別是`pen`,`apple`,`pen`,`pinapplen`用forEach把裡面的產品都變成one。

```javascript
let arr = [
 { p: "pen" }, { p: "apple" }, 
 { p: "pen" }, { p: "pinapple" }
];
arr.forEach(e => {
 console.log(e); //{ p:'pen' }
 e.p = "one";
});
//console.log(arr)
//[{ p:"one" },{ p:"one" },{ p:"one" }, { p:"one" }]
```

## map -- 和forEach兄弟長的好像

* 把每一個元素抓出來執行
* 將執行的結果存到另一個arr

![](<../.gitbook/assets/截圖 2020-03-08 下午10.05.29.png>)

![](<../.gitbook/assets/截圖 2020-03-08 下午10.06.35.png>)

## filter&#x20;

![](<../.gitbook/assets/截圖 2020-03-08 下午10.07.20.png>)

## find

![](<../.gitbook/assets/截圖 2020-03-08 下午10.08.05.png>)

some every

![](<../.gitbook/assets/截圖 2020-03-08 下午10.09.00.png>)

## sort

![](<../.gitbook/assets/截圖 2020-03-08 下午10.09.45.png>)

## reduce

![](<../.gitbook/assets/截圖 2020-03-08 下午10.10.36.png>)

### reduce如何運作

```
[0, 1, 2, 3, 4].reduce(
  (accumulator, currentValue, currentIndex, array) => {
    return accumulator + currentValue;
  },
  10
);
```

|    callback | accumulator | currentValue | currentIndex |     array    | return value |
| ----------: | :---------: | :----------: | :----------: | :----------: | :----------: |
|  first call |      10     |       0      |       0      | \[0,1,2,3,4] |      10      |
| second call |      10     |       1      |       1      | \[0,1,2,3,4] |      11      |
|  third call |      11     |       2      |       2      | \[0,1,2,3,4] |      13      |
| fourth call |      13     |       3      |       3      | \[0,1,2,3,4] |      16      |
|  fifth call |      16     |       4      |       4      | \[0,1,2,3,4] |      20      |

### 攤平一個多維陣列

```
var flattened = [[0, 1], [2, 3], [4, 5]].reduce((a, b) => a.concat(b)
    ,[]);
// flattened is [0, 1, 2, 3, 4, 5]
```

### 計算相同元素數量並以物件鍵值顯示

```
var names = ['Alice', 'Bob', 'Tiff', 'Bruce', 'Alice'];

var countedNames = names.reduce(function (allNames, name) { 
  if (name in allNames) {
    allNames[name]++;
  }
  else {
    allNames[name] = 1;
  }
  return allNames;
}, {});
// countedNames is:
// { 'Alice': 2, 'Bob': 1, 'Tiff': 1, 'Bruce': 1 }
```

### 移除陣列中的重複項目--sort()組和技

```
let arr = [1, 2, 1, 2, 3, 5, 4, 5, 3, 4, 4, 4, 4];
let result = arr.sort().reduce((init, current) => {
    if (init.length === 0 || init[init.length - 1] !== current) {
        init.push(current);
    }
    return init;
}, []);
console.log(result); //[1,2,3,4,5]
```

| Array 批次操作方法            | 描述            |
| ----------------------- | ------------- |
| Array.forEach(Function) | 對每一個元素執行動作    |
| Array.map(Function)     | 執行結果存到新的Array |
| Array.filter(Function)  | 留下符合條件的元素     |
| Array.find(Function)    | 找到第一個符合條件的元素  |
| Array.some(Function)    | 判斷有元素符合條件     |
| Array.every(Function)   | 判斷所有元素符合條件    |
| Array.sort(Function)    | 根據大小排序陣列      |
| Array.reduce(Function)  | 根據規則縮減陣列      |
