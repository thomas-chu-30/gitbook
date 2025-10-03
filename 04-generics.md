# 04 generics

> 泛型（Generics）是指在定義函式、介面或類別的時候，**不預先指定具體的型別**，而在使用的時候再指定型別的一種特性。

## 簡單的例子

首先，我們來實現一個函式 `createArray`，它可以建立一個指定長度的陣列，同時將每一項都填充一個預設值：

{% code title="createArray (any)" %}
```typescript
function createArray(length: number, value: any): Array<any> {
  let result = [];
  for (let i = 0; i < length; i++) {
    result[i] = value;
  }
  return result;
}
createArray(3, "x"); // ['x', 'x', 'x']
```
{% endcode %}

上例中，我們使用了[之前提到過的陣列泛型](https://willh.gitbook.io/typescript-tutorial/basics/type-of-array#%E9%99%A3%E5%88%97%E6%B3%9B%E5%9E%8B)來定義返回值的型別。

這段程式碼編譯不會報錯，但是一個顯而易見的缺陷是，它並沒有準確的定義返回值的型別：

`Array<any>` 允許陣列的每一項都為任意型別。但是我們預期的是，陣列中每一項都應該是輸入的 `value` 的型別。

這時候，泛型就派上用場了：

{% code title="createArray (generic)" %}
```typescript
function createArray<T>(length: number, value: T): Array<T> {
  let result: T[] = [];
  for (let i = 0; i < length; i++) {
    result[i] = value;
  }
  return result;
}
createArray<string>(3, "x"); // ['x', 'x', 'x']
```
{% endcode %}

上例中，我們在函式名後添加了 `<T>`，其中 `T` 用來指代任意輸入的型別，在後面的輸入 `value: T` 和輸出 `Array<T>` 中即可使用了。

接著在呼叫的時候，可以指定它具體的型別為 `string`。當然，也可以不手動指定，而讓型別推論自動推算出來：

{% code title="createArray (inferred)" %}
```typescript
function createArray<T>(length: number, value: T): Array<T> {
  let result: T[] = [];
  for (let i = 0; i < length; i++) {
    result[i] = value;
  }
  return result;
}
createArray(3, "x"); // ['x', 'x', 'x']
```
{% endcode %}

## 多個型別引數

定義泛型的時候，可以一次定義多個型別引數：

{% code title="" %}
```typescript
function swap<T, U>(tuple: [T, U]): [U, T] {
  return [tuple[1], tuple[0]];
}
swap([7, "seven"]); // ['seven', 7]
```
{% endcode %}

上例中，我們定義了一個 `swap` 函式，用來交換輸入的元組。

## 泛型約束

在函式內部使用泛型變數的時候，由於事先不知道它是哪種型別，所以不能隨意的操作它的屬性或方法：

{% code title="" %}
```typescript
function loggingIdentity<T>(arg: T): T {
  console.log(arg.length);
  return arg;
}

// index.ts(2,19): error TS2339: Property 'length' does not exist on type 'T'.
```
{% endcode %}

上例中，泛型 `T` 不一定包含屬性 `length`，所以編譯的時候報錯了。

這時，我們可以對泛型進行約束，只允許這個函式傳入那些包含 `length` 屬性的變數。這就是泛型約束：

{% code title="loggingIdentity (constrained)" %}
```typescript
interface Lengthwise {
  length: number;
}
function loggingIdentity<T extends Lengthwise>(arg: T): T {
  console.log(arg.length);
  return arg;
}
```
{% endcode %}

上例中，我們使用了 `extends` 約束了泛型 `T` 必須符合介面 `Lengthwise` 的形狀，也就是必須包含 `length` 屬性。

此時如果呼叫 `loggingIdentity` 的時候，傳入的 `arg` 不包含 `length`，那麼在編譯階段就會報錯了：

{% code title="loggingIdentity (call error)" %}
```typescript
interface Lengthwise {
  length: number;
}
function loggingIdentity<T extends Lengthwise>(arg: T): T {
  console.log(arg.length);
  return arg;
}

loggingIdentity(7);
// 傳入的參數，必需要有 length 的屬性，不然就會有 error 出現
// index.ts(10,17): error TS2345: Argument of type '7' is not assignable to parameter of type 'Lengthwise'.
```
{% endcode %}

{% hint style="info" %}
多個型別引數之間也可以互相約束：
{% endhint %}

{% code title="copyFields" %}
```typescript
function copyFields<T extends U, U>(target: T, source: U): T {
  for (let id in source) {
    target[id] = (<T>source)[id];
  }
  return target;
}
let x = { a: 1, b: 2, c: 3, d: 4 };
copyFields(x, { b: 10, d: 20 });
```
{% endcode %}

上例中，我們使用了兩個型別引數，其中要求 `T` 繼承 `U`，這樣就保證了 `U` 上不會出現 `T` 中不存在的欄位。

## 泛型介面

[之前學習過](https://willh.gitbook.io/typescript-tutorial/basics/type-of-function#%E4%BB%8B%E9%9D%A2%E4%B8%AD%E5%87%BD%E5%BC%8F%E7%9A%84%E5%AE%9A%E7%BE%A9)，可以使用介面的方式來定義一個函式需要符合的形狀：

{% code title="SearchFunc" %}
```typescript
interface SearchFunc {
  (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function (source: string, subString: string) {
  return source.search(subString) !== -1;
};
```
{% endcode %}

當然也可以使用含有泛型的介面來定義函式的形狀：

{% code title="CreateArrayFunc (generic signature)" %}
```typescript
interface CreateArrayFunc {
  <T>(length: number, value: T): Array<T>;
}

let createArray: CreateArrayFunc;
createArray = function <T>(length: number, value: T): Array<T> {
  let result: T[] = [];
  for (let i = 0; i < length; i++) {
    result[i] = value;
  }
  return result;
};
createArray(3, "x"); // ['x', 'x', 'x']
```
{% endcode %}

進一步，我們可以把泛型引數提前到介面名上：

{% code title="CreateArrayFunc<T>" %}
```typescript
interface CreateArrayFunc<T> {
  (length: number, value: T): Array<T>;
}

let createArray: CreateArrayFunc<any>;

createArray = function <T>(length: number, value: T): Array<T> {
  let result: T[] = [];
  for (let i = 0; i < length; i++) {
    result[i] = value;
  }
  return result;
};
createArray(3, "x"); // ['x', 'x', 'x']
```
{% endcode %}

注意，此時在使用泛型介面的時候，需要定義泛型的型別。

## 泛型類別

與泛型介面類似，泛型也可以用於類別的型別定義中：

{% code title="GenericNumber" %}
```typescript
class GenericNumber<T> {
  zeroValue: T;
  add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function (x, y) {
  return x + y;
};
```
{% endcode %}

## 泛型引數的預設型別

在 TypeScript 2.3 以後，我們可以為泛型中的型別引數指定預設型別。當使用泛型時沒有在程式碼中直接指定型別引數，從實際值引數中也無法推測出時，這個預設型別就會起作用。

{% code title="createArray (default generic)" %}
```typescript
function createArray<T = string>(length: number, value: T): Array<T> {
  let result: T[] = [];
  for (let i = 0; i < length; i++) {
    result[i] = value;
  }
  return result;
}
```
{% endcode %}
