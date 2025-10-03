# Set & Map

### Map

`Map` 物件是簡單的 key-value 配對，`物件（Object）`和 `Map` 很相似，但是有以下幾點差別：

1. **Map 裡面的 key 是唯一的**，如果 `set` 到重複的 key，則舊的 value 會被覆蓋。
2. `Map` 中的 `keys` 會根據**被添加資料的時間而有順序性**，但 `Object` 沒有順序性。
3. Object 的鍵（key）只能是 `字串（Strings）`或 `Symbols`，而 `Map` 的鍵可以是任何值，包含物件、函式或原始型別（primitive type）。
4. 若要取得 `Map` 的大小非常容易，只需要取得 size 屬性即可；而 Object 的大小必須手動決定。
5. **當需要經常增添刪減屬性時，使用 `Map` 的效能會比 `Object` 來得好。**

> ES6 中**如果希望「陣列（Array）」的元素不會重複，可以使用 `Set`；如果是希望物件（Object）的鍵不會重複，則可以使用 `Map`**。

使用語法

```javascript
// 建立 Map
let myMap = new Map(); // 建立空的 Map
let myMap = new Map([
  [1, 'one'],
  [2, 'two'],
]); // 建立 Map 時直接代入內容

let keyString = 'a string',
	keyObj = {},
	keyFunc = function () {};

// 透過 .set(key, value) 來在 Map 中添加屬性
myMap.set(keyString, 'value associated with string');
myMap.set(keyObj, 'value associated with object');
myMap.set(keyFunc, 'value associated with function');

// 方法
myMap.has(keyString); // true，透過 .has 判斷該 Map 中是否有某一屬性
myMap.size; //  3，透過 .size 來取得 Map 內的屬性數目
myMap.get(keyString); // 使用 .get(key) 可取得屬性的內容
myMap.delete(keyString); // 刪除 Map 中的某個屬性，成功刪除回傳 true，否則 false
myMap.clear(); // 清空整個 Map
```

#### Map 的疊代

```javascript
myMap.keys(); // 取得 Map 的所有 keys，回傳 Iterable 的物件
myMap.values(); // 取得 Map 的所有 values，回傳 Iterable 的物件
myMap.entires(); // 取得 Map 的所有內容，回傳 Iterable 的物件
```

```javascript
// 透過 for...of 可以列舉所有 Map 中的內容
for (let [key, value] of myMap) {
  console.log(`The value of ${key} in Map is ${value}`);
}

for (let [key, value] of myMap.entries()) {
  console.log(key + ' = ' + value);
}

myMap.forEach(function (value, key) {
  console.log(key + ' = ' + value);
});

// 取得 Map 的 key
for (let key of myMap.keys()) {
  console.log(key);
}

// 取得 Map 的所有 values
for (let value of myMap.values()) {
  console.log(value);
}
```

#### Map 的複製（Clone）

```javascript
let students = {
  Aaron: { age: '29', country: 'Taiwan' },
  Jack: { age: '26', country: 'USA' },
  Johnson: { age: '32', country: 'Korea' },
};

const studentMap = new Map(Object.entries(students));
const cloneMap = new Map(studentMap);

cloneMap.get('Aaron'); // { age: '29', country: 'Taiwan' }
studentMap === cloneMap; // false. Useful for shallow comparison
```

#### Map 的合併（Merge）

```javascript
var first = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

var second = new Map([
  [1, 'uno'],
  [2, 'dos'],
]);

// 合併兩個 Map 時，後面的 Key 會覆蓋調前面的
// 透過 Spread operator 可以將 Map 轉換成陣列
var merged = new Map([...first, ...second]);

console.log(merged.get(1)); // uno
console.log(merged.get(2)); // dos
console.log(merged.get(3)); // three
```

**Map 也可以和陣列相合併**

```javascript
// Merge maps with an array. The last repeated key wins.
var merged = new Map([...first, ...second, [1, 'foo']]);

console.log(merged.get(1)); // foo
console.log(merged.get(2)); // dos
console.log(merged.get(3)); // three
```

#### 使用範例

使用 `Object.entries` 來將資料變成 iterable 代入 new `Map` 的參數中

```javascript
let students = {
  Aaron: { age: '29', country: 'Taiwan' },
  Jack: { age: '26', country: 'USA' },
  Johnson: { age: '32', country: 'Korea' },
};

const studentMap = new Map(Object.entries(students));

// 透過 for...of 可以列舉所有 Map 中的內容
for (let [key, value] of studentMap) {
  const { age, country } = value;
  console.log(`${key} is ${age} year's old, and lives in ${country}`);
}

//  檢驗某個鍵是否存在
studentMap.has('Aaron');

console.log(studentMap.get('Jack')); // 取得 Map 的屬性內容，Object { age: '26', country: 'USA' }
studentMap.delete('Johnson'); // 刪除 Map 中的某個屬性
studentMap.clear(); // 清空整個 Map

console.log(studentMap.size); // 看看 Map 中有多少內容
```

**補充**

#### 將 Map 轉成 Array

```javascript
const theMap = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

const theArray = [...theMap.entries()]; // [[1, 'one'], [2, 'two'], [3, 'three']]
const arrayOfMapKeys = [...theMap.keys()]; // [1, 2, 3]
const arrayOfMapValues = [...theMap.values()]; // ["one", "two", "three"]
```

#### 使用 Map 製作 HashTable

```javascript
// Credits: Engine Lin (https://medium.com/@linengine)

// object 也可以使用
const arr = ['apple', 'apple', 'banana', 'banana', 'cat', 'dog', 'fat', 'fat', 'fat', 'fat'];

const hashTable = new Map();

arr.forEach((item) => {
  if (hashTable.has(item)) {
    hashTable.set(item, hashTable.get(item) + 1);
  } else {
    hashTable.set(item, 1);
  }
});

// Map { 'apple' => 2, 'banana' => 2, 'cat' => 1, 'dog' => 1, 'fat' => 4 }
console.log(hashTable);
```

[參考文章](https://pjchender.dev/npm/npm-rx-js/#interval)



### Set

ES6 中**如果希望「陣列（Array）」的元素不會重複，可以使用 `Set`；如果是希望物件（Object）的鍵不會重複，則可以使用 `Map`**。

> [Set](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set) @ MDN

Set 的特色是有 `has()` 這個方法，可以**快速判斷該 Set 中是否包含某個元素**，重點不是讓我們把 Set 中的元素取出來用。

```
const obj = {  foo: 'bar',};const mySet = new Set();mySet.add(1); // [1]mySet.add(5); // [1, 5]mySet.add(5); // [1, 5]，重複的元素不會被加進去，依然是mySet.add(obj); // [ 1, 5, { foo: 'bar' } ]​mySet.has(5); // truemySet.has(obj); // truemySet.has({  foo: 'bar',}); // false​mySet.delete(5); // true，刪除成功mySet.delete(2); // false，刪除失敗mySet.size; // 2​// for ... ofmySet.entries(); // [key, value] 內容相同mySet.values(); // 和 mySet.keys 得到相同的內容mySet.keys(); // 和 mySet.values 得到相同的內容​mySet.forEach((item) => console.log('item', item));mySet.clear();
```

#### 基本使用

set 裡面的鍵不會重複，是 unique 的。

```
// new Set Typelet classroom = new Set(); //  建立教室這個 setlet Aaron = { name: 'Aaron', country: 'Taiwan' };let Jack = { name: 'Jack', country: 'USA' };let Johnson = { name: 'Johnson', country: 'Korea' };​// 把物件放入 set 中classroom.add(Aaron);classroom.add(Jack);classroom.add(Johnson);​// 檢驗 set 中是否包含某物件if (classroom.has(Aaron)) console.log('Aaron is in the classroom');​//  把物件移除 set 中classroom.delete(Jack);console.log(classroom.size); //    看看 set 中有多少元素console.log(classroom);
```

#### 陣列與集合間轉換

```
// Set 轉成 Arraylet SetToArray = [...classroom]; // Array.from(classroom);​// Array 轉成 Setlet ArrayToSet = new Set(SetToArray);
```

#### 過濾陣列中重複的元素

**keywords: `filter array`, `unique array element`**

**利用 Set 中元素不會重複的特性，可以用來過濾掉陣列中重複的元素，留下唯一**：

```
const arr = [1, 1, 3, 5, 5, 7];const arrToSet = new Set(arr); // Set { 1, 3, 5, 7 }const uniqueArray = [...arrToSet]; // [ 1, 3, 5, 7 ]
```

#### 例子

```
var mySet = new Set();​mySet.add(1); // Set { 1 }mySet.add(5); // Set { 1, 5 }mySet.add('some text'); // Set { 1, 5, 'some text' }var o = { a: 1, b: 2 };mySet.add(o); // Set { 1, 5, 'some text', { a: 1, b: 2 } }​// // o is referencing a different object so this is okaymySet.add({ a: 1, b: 2 }); // Set { 1, 5, 'some text', { a: 1, b: 2 }, { a: 1, b: 2 } }
```

\
