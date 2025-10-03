# 06 interface

### interface 基本款

**Interface 可以用來定義物件，還有由物件所延伸的型別（例如，陣列、函式）**

#### Object

```typescript
interface Person {
  name: string;
  age: number;
}
```

#### Object / Array

元素都是字串的陣列：

```typescript
interface StringArray {
  [index: number]: string;
}
```

{% hint style="info" %}
不過自已覺得這樣子比較實用一點：

```typescript
const arr: string[] = ["thomas", "tay", "leo"];
```
{% endhint %}

任何屬性值都可以接收的物件：

```typescript
interface Dictionary {
  [propName: string]: any;
}
```

#### Function 上用的到的 interface

```typescript
interface GreetFunc {
  (name: string, age: number): void;
}

// 如果用 type alias 來定義型別會長這樣
type GreetFunc2 = (name: string, age: number) => void;

// 可以不用額外指定參數型別（參數名稱可以不同）
let greet: GreetFunc = (n, a) => {
  console.log(`Hello ${n}, you are ${a} years old`);
};

// 這個 greet function 會自動符合 Greet 這個 interface
let greet: GreetFunc = (name: string, age: number) => {
  console.log(`Hello ${name}, you are ${age} years old`);
};

// 即使參數名稱不符合，只要型別符合即會算在這個 Interface
// 例如這裡改用 n, a 當作參數名稱，一樣符合 Greet Interface
let greet: GreetFunc = (n: string, a: number) => {
  console.log(`Hello ${n}, you are ${a} years old`);
};
```

函式參數也可以使用物件的解構賦值：

```typescript
interface GreetFunc {
  (person: { name: string; age: number }): void;
}

const greet: GreetFunc = ({ name, age }) => {
  console.log(`Hello ${name}, you are ${age} years old.`);
};

// 或者等同於
const greet = ({ name, age }: { name: string; age: number }) => {
  console.log(`Hello ${name}, you are ${age} years old.`);
};

greet({
  name: "Aaron",
  age: 32,
});
```

另外，當一個函式最終不一定有回傳值時，預設會是回傳 undefined，例如：

```typescript
interface Foo {
  (a: number): number;
}

// 由於 foo 這個函式最終可能沒有回傳值
// ❌ 修改 interface：因此在宣告 Foo 時，回傳值要是 `number | undefined`
const foo: Foo = (a) => {
  if (a > 5) {
    return 3;
  }
};

// ⭕️ 修改函式：或者在函式最後拋出錯誤（never），這時候因為 `number | never` 等同於 `number`
// 因此將符合 interface 的定義
const bar: Foo = (a) => {
  if (a > 5) {
    return 3;
  }
  throw new Error("a is not over 5");
};
```

### 將 Interface 當成 Type：不能多不能少

```typescript
interface Person {
  name: string;
  age: number;
}

// ❌ interface 缺少 key 不行
const martin: Person = {
  name: "Martin",
};

// ❌ interface 多 key 不行
const leo: Person = {
  name: "Martin",
  age: 30,
  job: "DevOps",
};
```

### Object Literal 的額外檢查

{% hint style="info" %}
（excess property checking）：可以多不能少

當使用 Object Literal 是「直接帶入函式參數」或「指派給其他變數」時，會經過額外的檢查（excess property checking），需要與 Interface 完全符合才可以。

💡 excess property checking 的情況，不論是使用 `type alias` 或 `interface` 都存在。
{% endhint %}

```typescript
// 將 Interface 當成函式參數
function greet(person: Person) {
  console.log(`${person.name} is ${person.age} years old`);
}

// ⭕️ 帶入 function 中的參數只要符合 Interface 即可，可以有多餘的屬性（例如，Job）
const aaron = {
  name: "Aaron",
  age: 32,
  job: "Web Developer",
};
greet(aaron);

// ❌ 但如果沒有先宣告成變數會顯示錯誤，這種情況 TS 會直接把參數和 Person 比對，類似 arron: Person
greet({
  name: "Aaron",
  age: 32,
  job: "Web Developer",
});

// ❌ 可以多不能少
const john = {
  name: "John",
};
greet(john);
```

#### 方法一：使用 type assertion 來避免 excess property checking

```typescript
greet({
  name: "Aaron",
  age: 32,
  job: "Web Developer",
} as Person);
```

#### 方法二：使用 index signature 來避免 excess property checking

```typescript
interface Person {
  // name 和 age 一定要存在
  name: string;
  age: string;
  // 但可以接受額外的屬性
  [propName: string]: string;
}
```

#### 方法三：將物件指派成一個變數來避免 excess property checking

```typescript
const aaron = {
  name: "Aaron",
  age: 32,
  job: "Web Developer",
};
greet(aaron);
```

### readonly

定義常數時使用 `const`，如果是希望某個物件的屬性是常數的話，則使用 `readonly`：

```typescript
interface Point {
  readonly x: number;
  y: number;
}

const point: Point = {
  x: 10,
  y: 10,
};

point.x = 15; // ❌ x 是 readonly 不能被修改
point.y = 20; // ⭕️
```

### 重複名稱的 interface (duplicate name)

interface 是 "open" 的，當有同樣名稱的 interface 被定義時，它們會被 merge。

舉例來說，當我們定義了三個相同名稱的 interface：

```typescript
interface Product {
  movie: string;
}

interface Product {
  dessert: string;
}

interface Product {
  book: string;
}
```

實際上這三個 interface 中的屬性會被合併在一起：

```typescript
// 等同於
interface Product {
  movie: string;
  dessert: string;
  book: string;
}
```

{% hint style="warning" %}
💡 當我們使用 Type 搭配交集（`&`）使用時，當有重複屬性名稱，但對應型別有衝突時，該屬性的型別會變成 `never`。
{% endhint %}

### Extending Interfaces

```typescript
interface Shape {
  color: string;
}

interface Square extends Shape {
  width: number;
}

// Triangle 這個 interface 是延伸自 Shape 和 Square
// 中文指的是：想要實作 Triangle 介面時，除了必須符合 Triangle 自身的屬性與方法外，
// 還必須實作出 Shape 和 Square 的屬性與方法
interface Triangle extends Shape, Square {
  height: number;
}

const shape: Shape = {
  color: "red",
};

const square: Square = {
  color: "red",
  width: 20,
};

const triangle: Triangle = {
  color: "red",
  width: 20,
  height: 30,
};
```

* **不同介面之間可以有相同屬性的名稱，但其各自對應的型別不能衝突，否則 TypeScript 會報錯**（ Named property 'width' of types 'Foo' and 'Bar' are not identical.）
