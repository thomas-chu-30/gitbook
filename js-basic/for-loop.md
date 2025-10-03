# for loop & switch

### 起手式

```javascript
for (statement 1; statement 2; statement 3) {
  // code block to be executed
}
```

&#x20;**Statement 1**在執行代碼塊之前執行（一次）。

&#x20;**Statement 2** 定義執行代碼塊的條件。

**Statement 3** 在代碼塊執行之後（每次）執行一次。

```javascript
//常用的for loop
let arr = [1,2,3,4,5,6,7];
for(let i = 0; i < arr.length; i++) {
      console.log(i);//0,1,2,3,4,5,6
} //依據arr的長度跑出幾次的迴圈
```

### for...in

如果for...in對arr跑的話，就會跑出idx的值。另外對obj跑的話，就會跑出keys，所以你的把obj\[keys]就可以得到裡面的value的值

```javascript
let arr = [1,2,3,4,5,6,7];
for(let i in arr){
    console.log(i) // 0,1,2,3,4,5,6
}

let todoList = {item1: "clear my car", item2: "hiking everyday"}
for(var item in todoList){
    console.log(item) //item1 item2
}

let todoList = {item1: "clear my car", item2: "hiking everyday"}
for(var item in todoList){
    console.log(todoList[item]) //clear my car //hiking everyday
}
```

### for...of

for...of 只可以對arr跑，沒有辦法對obj跑，那它跑出來的就是arr裡面的值。

```javascript
let arr = [1,2,3,4,5,6,7];
for(let i of arr){
    console.log(i) // 1,2,3,4,5,6,7
}
let arr = [{key:'val'},{key:'val2'}]
for(let i of arr){
    console.log(i) //{key: "val"} //{key: "val2"}
}
```

[for of for in 差別在那裡](https://kanboo.github.io/2018/01/30/JS-for-of-forin/)

### switch

下列為 swtich 的基本寫法

```javascript
switch (expr) {
  case 'Oranges':
    console.log('Oranges are $0.59 a pound.');
    break;
  case 'Mangoes':
  case 'Papayas':
    console.log('Mangoes and papayas are $2.79 a pound.');
    // expected output: "Mangoes and papayas are $2.79 a pound."
    break;
  default:
    console.log(`Sorry, we are out of ${expr}.`);
}
```

如果你有同時要判斷好多的 case 的話，可以參考下列的這個寫法

{% embed url="https://stackoverflow.com/questions/13207927/switch-statement-for-multiple-cases-in-javascript" %}
同時判斷好幾個 case&#x20;
{% endembed %}

