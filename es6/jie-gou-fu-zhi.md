---
description: >-
  js在設定變數的時候，都是一個值，但如果這個變數可以變成有結構的，那會不會變的很好用呢？沒錯那就是destructuring
  assignment，讓你的變數有結構，這實在是有夠酷的拉~~~
---

# Destructuring assignment--(解構賦值)

## 起手式

#### 解構賦值可以追尋幾個重點來寫，會比較好上手：

1. `物件解構` `陣列解構`變數寫成結構的樣子，變數放在值(value)的位子
2. `物件解構`不想要設定變數，把變數放在(key)的位子
3. …rest表示結構中沒有指定的值(需要放到最後面)
4. 預計值(default value)的寫法，是在變數的結構中直接賦予變數數值

```javascript
//array
let arr = [10, 20];
let [a, b] = arr;
console.log(a);// expected output: 10
console.log(b);// expected output: 20

//-----...rest把後面的剩下的資料都賦予到rest裡面 ----
let [a, b, ...rest] = [10, 20, 30, 40, 50];
console.log(rest);// expected output: Array [30,40,50]

//obj
let obj = { a: 10, b: 20 }
let { a:a, b:b } = obj;
console.log(a); // 10
console.log(b); // 20

//-----...rest把後面的剩下的資料都賦予到rest裡面 ----
// Stage 4(finished) proposal
{a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40};
console.log(a); // 10
console.log(b); // 20
console.log(rest); // {c: 30, d: 40}
```

## 陣列解構

### 設定default值

```javascript
let a, b;
[a=5, b=7] = [1];   //原本只要寫 [ a , b ] 但如果沒有值給它的話就給它一個預設值
console.log(a); // 1
console.log(b); // 7   //沒有給值，所以為預設的的 7 .
```

### 變數交換

```javascript
let a = 1;
let b = 3;

[a, b] = [b, a];
console.log(a); // 3
console.log(b); // 1

//----------------------以前的話必需要這樣子寫----------------------//
const arr = [1,2,3];
[arr[2], arr[1]] = [arr[1], arr[2]];
console.log(arr); // [1,3,2]
```

### 解析自函式回傳的陣列

```javascript
function f() {
  return [1, 2];
}

let a, b; 
[a, b] = f(); 
console.log(a); // 1
console.log(b); // 2
```

### 忽略某些回傳值

```javascript
let arr = [1,2,3];

const [a, , b] = arr;  //忽略中間的這個值
console.log(a); // 1
console.log(b); // 3
[,,] = arr; //你也是可以全部忽略
```

### 把矩陣剩餘部分解構到一個變數

```javascript
const [a, ...b] = [1, 2, 3];
console.log(a); // 1
console.log(b); // [2, 3]
```

{% hint style="danger" %}
&#x20;要注意的是，當左邊函式裡使用其餘解構，同時使用結尾逗號，這樣會拋出例外 `SyntaxError`

```javascript
const [a, ...b,] = [1, 2, 3];    //b後面多了一個逗號
// SyntaxError 語法錯誤: 其餘元素不可以跟隨結尾逗號
// 需要把其餘運算子放在最後的元素
```
{% endhint %}

### 正規運算式的比對結果取值

(這個要在想一下這個，比較難一點)

&#x20;當正則運算式的方法 `exec()` 比對到一個值，其回傳陣列中的第一個值是相符的完整字串，後績的則是比對到正則運算式每組括號內的部分。當你沒需要利用第一個完整比對結果時，解構指派式讓你更簡單的取出後績元素。

```javascript
function parseProtocol(url) { 
  const parsedURL = /^(\w+)\:\/\/([^\/]+)\/(.*)$/.exec(url);
  if (!parsedURL) {
    return false;
  }
  console.log(parsedURL); // ["https://developer.mozilla.org/en-US/Web/JavaScript", "https", "developer.mozilla.org", "en-US/Web/JavaScript"]

  const [, protocol, fullhost, fullpath] = parsedURL;
  return protocol;
}

console.log(parseProtocol('https://developer.mozilla.org/en-US/Web/JavaScript')); // "https"
```

## 物件解構

### 無宣告指派

變數可以在宣告式後，再透過解構進行指派。

```javascript
let a, b;
({a, b} = {a:1, b:2}); //括弧一定要加

const {a,b} = {a:1, b:2} //和上面兩行完全一樣意思
```

{% hint style="info" %}
當針對物件進行解構，而該句式沒有進行宣告時，指派式外必須加上括號 `( ... )`

`{a, b} = {a: 1, b: 2}` 不是有效的獨立語法，因為左邊的 `{a, b}` 被視為程式碼區塊&#x800C;_**非物件。**_

_**`({a, b} = {a: 1, b: 2})`**_ 是有效的，如同 _**`const {a, b} = {a: 1, b: 2}`**_

`( ... )` 表達式前句需要以分號結束，否則可能把上一句視為函式隨即執行。
{% endhint %}

### 指派到新的變數名稱

```javascript
const o = {p: 42, q: true};
const {p: foo, q: bar} = o;
 
console.log(foo); // 42 
console.log(bar); // true
```

### 在物件解構時使用其餘變數

```javascript
let {a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40}
console.log(a); // 10 
console.log(b); // 20 
console.log(rest); // { c: 30, d: 40 }
```

### 預設值&指定新的變數名稱及預設值

```javascript
const {a = 10, b = 5} = {a: 3};
console.log(a); // 3
console.log(b); // 5


const {a:aa = 10, b:bb = 5} = {a: 3};
console.log(aa); // 3
console.log(bb); // 5
```

###

### 放在Function的參數裡

```javascript
const user = {
  id: 42,
  displayName: 'jdoe',
  fullName: {
    firstName: 'John',
    lastName: 'Doe'
  }
};

function userId({id}) {   //變數寫成 { id } 直接取到變數物件中的 id 值
  return id;
}

function whois({displayName, fullName: {firstName: name}}) {
  return `${displayName} is ${name}`;
}

console.log(userId(user)); // 42
console.log(whois(user));  // "jdoe is John"

//當然你也可以預設值在裡面
function drawChart({
      size = "big",
      coords = { x: 0, y: 0 },
      radius = 25
    } = {}) {
      console.log(size, coords, radius);
      // do some chart drawing
    }

drawChart({
  coords: {x: 18, y: 30},
  radius: 30
});
```

{% hint style="info" %}
&#x20;在上述函式 **`drawChart`** 中，左方之解構式被指派到一個空物件: `{size = 'big', coords = {x: 0, y: 0}, radius = 25} = {}` 。你也可以略過填寫右方的指派式。不過，當你沒有使用右方指派式時，函式在呼叫時會找出最少一個參數。透過上述形式，你可以直接不使用參數的呼叫 **`drawChart()`** 。當你希望在呼叫這個函式時不傳送參數，這個設計會帶來方便。而另一個設計則能讓你確保函式必須傳上一個物件作為參數。
{% endhint %}

## 進階(混合使用)

### 陣列及物件--混合使用

```javascript
const props = [
  { id: 1, name: 'Fizz'},
  { id: 2, name: 'Buzz'},
  { id: 3, name: 'FizzBuzz'}
];

const [,, { name }] = props;

console.log(name); // "FizzBuzz"
```

### 巢狀物件或陣列的解構

```javascript
const metadata = {
  title: 'Scratchpad',
  translations: [
    {
      locale: 'de',
      localization_tags: [],
      last_edit: '2014-04-14T08:43:37',
      url: '/de/docs/Tools/Scratchpad',
      title: 'JavaScript-Umgebung'
    }
  ],
  url: '/en-US/docs/Tools/Scratchpad'
};

let {
  title: englishTitle, // rename
  translations: [
    {
       title: localeTitle, // rename
    },
  ],
} = metadata;

console.log(englishTitle); // "Scratchpad"
console.log(localeTitle);  // "JavaScript-Umgebung"
```

### 循環取出的解構

```javascript
const people = [
  {
    name: 'Mike Smith',
    family: {
      mother: 'Jane Smith',
      father: 'Harry Smith',
      sister: 'Samantha Smith'
    },
    age: 35
  },
  {
    name: 'Tom Jones',
    family: {
      mother: 'Norah Jones',
      father: 'Richard Jones',
      brother: 'Howard Jones'
    },
    age: 25
  }
];

for (const {name: n, family: {father: f}} of people) {
  console.log('Name: ' + n + ', Father: ' + f);
}

// "Name: Mike Smith, Father: Harry Smith"
// "Name: Tom Jones, Father: Richard Jones"
```

### 以物件演算屬性名稱解構

```javascript
let key = 'z';
let {[key]: foo} = {z: 'bar'};

console.log(foo); // "bar"
```

### 不符合 JavaScript 識別字的屬性名稱

```javascript
const foo = { 'fizz-buzz': true };
const { 'fizz-buzz': fizzBuzz } = foo;

console.log(fizzBuzz); // "true"   
```

### 物件解構時的原型鏈追溯

```javascript
let obj = {self: '123'};
obj.__proto__.prot = '456';
const {self, prot} = obj;
// self "123"
// prot "456"（Access to the prototype chain）
```
