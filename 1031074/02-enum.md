# 02 enum

> 列舉（Enum）型別用於取值被限定在一定範圍內的場景，比如一週只能有七天，顏色限定為紅綠藍等。

### 基本語法

列舉使用 `enum` 關鍵字來定義：

```typescript
enum Days {
  Sun,
  Mon,
  Tue,
  Wed,
  Thu,
  Fri,
  Sat,
}
```

列舉成員會被賦值為從 `0` 開始遞增的數字，同時也會對列舉值到列舉名進行反向對映：

```typescript
enum Days {
  Sun,
  Mon,
  Tue,
  Wed,
  Thu,
  Fri,
  Sat,
}

console.log(Days["Sun"] === 0); // true
console.log(Days["Mon"] === 1); // true
console.log(Days["Tue"] === 2); // true
console.log(Days["Sat"] === 6); // true
console.log(Days[0] === "Sun"); // true
console.log(Days[1] === "Mon"); // true
console.log(Days[2] === "Tue"); // true
console.log(Days[6] === "Sat"); // true
```

事實上，上面的例子會被編譯為：

```typescript
var Days;
(function (Days) {
  Days[(Days["Sun"] = 0)] = "Sun";
  Days[(Days["Mon"] = 1)] = "Mon";
  Days[(Days["Tue"] = 2)] = "Tue";
  Days[(Days["Wed"] = 3)] = "Wed";
  Days[(Days["Thu"] = 4)] = "Thu";
  Days[(Days["Fri"] = 5)] = "Fri";
  Days[(Days["Sat"] = 6)] = "Sat";
})(Days || (Days = {}));
```

### 手動賦值

我們也可以給列舉項手動賦值：

```typescript
enum Days {
  Sun = 7,
  Mon = 1,
  Tue,
  Wed,
  Thu,
  Fri,
  Sat,
}

console.log(Days["Sun"] === 7); // true
console.log(Days["Mon"] === 1); // true
console.log(Days["Tue"] === 2); // true
console.log(Days["Sat"] === 6); // true
```

上面的例子中，未手動賦值的列舉項會接著上一個列舉項遞增。

{% hint style="warning" %}
重要提醒：如果未手動賦值的列舉項與手動賦值的重複了，TypeScript 不會察覺到這一點，可能導致值被覆蓋而不報錯。
{% endhint %}

例如：

```typescript
enum Days {
  Sun = 3,
  Mon = 1,
  Tue,
  Wed,
  Thu,
  Fri,
  Sat,
}

console.log(Days["Sun"] === 3); // true
console.log(Days["Wed"] === 3); // true
console.log(Days[3] === "Sun"); // false
console.log(Days[3] === "Wed"); // true
```

上面的例子中，遞增到 `3` 的時候與前面的 `Sun` 的取值重複了，但是 TypeScript 並沒有報錯，導致 `Days[3]` 的值先是 `"Sun"`，而後又被 `"Wed"` 覆蓋了。編譯的結果是：

```typescript
var Days;

(function (Days) {
  Days[(Days["Sun"] = 3)] = "Sun";
  Days[(Days["Mon"] = 1)] = "Mon";
  Days[(Days["Tue"] = 2)] = "Tue";
  Days[(Days["Wed"] = 3)] = "Wed";
  Days[(Days["Thu"] = 4)] = "Thu";
  Days[(Days["Fri"] = 5)] = "Fri";
  Days[(Days["Sat"] = 6)] = "Sat";
})(Days || (Days = {}));
```

手動賦值的列舉項可以不是數字，此時需要使用型別斷言來讓 TypeScript 編譯器無視型別檢查（編譯出的 JavaScript 仍然是可用的）：

```typescript
enum Days {
  Sun = 7,
  Mon,
  Tue,
  Wed,
  Thu,
  Fri,
  Sat = <any>"S",
}

var Days;

(function (Days) {
  Days[(Days["Sun"] = 7)] = "Sun";
  Days[(Days["Mon"] = 8)] = "Mon";
  Days[(Days["Tue"] = 9)] = "Tue";
  Days[(Days["Wed"] = 10)] = "Wed";
  Days[(Days["Thu"] = 11)] = "Thu";
  Days[(Days["Fri"] = 12)] = "Fri";
  Days[(Days["Sat"] = "S")] = "Sat";
})(Days || (Days = {}));
```

當然，手動賦值的列舉項也可以為小數或負數，此時後續未手動賦值的項的遞增步長仍為 `1`：

```typescript
enum Days {
  Sun = 7,
  Mon = 1.5,
  Tue,
  Wed,
  Thu,
  Fri,
  Sat,
}

console.log(Days["Sun"] === 7); // true
console.log(Days["Mon"] === 1.5); // true
console.log(Days["Tue"] === 2.5); // true
console.log(Days["Sat"] === 6.5); // true
```

### 常數項和計算所得項

列舉項有兩種型別：常數項（constant member）和計算所得項（computed member）。

前面我們所舉的例子都是常數項，一個典型的計算所得項的例子：

```typescript
enum Color {
  Red,
  Green,
  Blue = "blue".length,
}
```

上面的例子中，`"blue".length` 就是一個計算所得項。

注意：如果緊接在計算所得項後面的是未手動賦值的項，那麼它就會因為無法獲得初始值而報錯：

```typescript
enum Color {
  Red = "red".length,
  Green,
  Blue,
}

// index.ts(1,33): error TS1061: Enum member must have initializer.
// index.ts(1,40): error TS1061: Enum member must have initializer.
```

下面是常數項和計算所得項的完整定義（部分參考自中文手冊）：

當滿足以下條件時，列舉成員被當作是常數：

* 不具有初始化函式並且之前的列舉成員是常數。在這種情況下，當前列舉成員的值為上一個列舉成員的值加 `1`。但第一個列舉元素是個例外。如果它沒有初始化方法，那麼它的初始值為 `0`。
* 列舉成員使用常數列舉表示式初始化。常數列舉表示式是 TypeScript 表示式的子集，它可以在編譯階段求值。當一個表示式滿足下面條件之一時，它就是一個常數列舉表示式：
  * 數字字面量
  * 參考之前定義的常數列舉成員（可以是在不同的列舉型別中定義的）。如果這個成員是在同一個列舉型別中定義的，可以使用非限定名來參考
  * 帶括號的常數列舉表示式
  * `+`, `-`, `~` 一元運算子應用於常數列舉表示式
  * `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `>>>`, `&`, `|`, `^` 二元運算子，且常數列舉表示式作為其中一個操作數。若常數列舉表示式求值後為 NaN 或 Infinity，則會在編譯階段報錯

所有其它情況的列舉成員被當作是需要計算得出的值。

### 常數列舉

常數列舉是使用 `const enum` 定義的列舉型別：

```typescript
const enum Directions {
  Up,
  Down,
  Left,
  Right,
}
let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

常數列舉與普通列舉的區別是，它會在編譯階段被刪除，並且不能包含計算所得項。

上例的編譯結果是：

```typescript
var directions = [0 /* Up */, 1 /* Down */, 2 /* Left */, 3 /* Right */];
```

如果包含了計算所得項，則會在編譯階段報錯：

```typescript
const enum Color {
  Red,
  Green,
  Blue = "blue".length,
}

// index.ts(1,38): error TS2474: In 'const' enum declarations member initializer must be constant expression.
```

### 外部列舉

外部列舉（Ambient Enums）是使用 `declare enum` 定義的列舉型別：

```typescript
declare enum Directions {
  Up,
  Down,
  Left,
  Right,
}
let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

`declare` 定義的型別只會用於編譯時的檢查，編譯結果中會被刪除。

上例的編譯結果是：

```typescript
var directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

外部列舉常出現在宣告檔案中。也可以同時使用 `declare` 和 `const`：

```typescript
declare const enum Directions {
  Up,
  Down,
  Left,
  Right,
}
let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

編譯結果：

```typescript
var directions = [0 /* Up */, 1 /* Down */, 2 /* Left */, 3 /* Right */];
```

### 列舉與命名空間結合

TypeScript 支援將列舉與命名空間結合使用，這種模式可以為列舉添加靜態方法，提供更豐富的功能。

#### 基本用法

```typescript
export enum DeviceNumber {
  DeviceOne = 0x16,
  DeviceTwo = 0x17,
}

export namespace DeviceNumber {
  export function fromNumber(value: number): DeviceNumber {
    switch (value) {
      case 1:
        return DeviceNumber.DeviceOne;
      case 2:
        return DeviceNumber.DeviceTwo;
      default:
        throw new Error(`Invalid auxiliary device number: ${value}`);
    }
  }
}
```

#### 使用範例

```typescript
// 直接使用列舉值
const device1 = DeviceNumber.DeviceOne; // 0x16
const device2 = DeviceNumber.DeviceTwo; // 0x17

// 使用命名空間中的靜態方法
const deviceFromNumber = DeviceNumber.fromNumber(1); // DeviceNumber.DeviceOne
const deviceFromNumber2 = DeviceNumber.fromNumber(2); // DeviceNumber.DeviceTwo

// 錯誤處理
try {
  DeviceNumber.fromNumber(3); // 拋出錯誤
} catch (error) {
  console.error(error.message); // "Invalid auxiliary device number: 3"
}
```

#### 編譯結果

```typescript
var DeviceNumber;
(function (DeviceNumber) {
  DeviceNumber[(DeviceNumber["DeviceOne"] = 22)] = "DeviceOne";
  DeviceNumber[(DeviceNumber["DeviceTwo"] = 23)] = "DeviceTwo";
})(DeviceNumber || (DeviceNumber = {}));

(function (DeviceNumber) {
  function fromNumber(value) {
    switch (value) {
      case 1:
        return DeviceNumber.DeviceOne;
      case 2:
        return DeviceNumber.DeviceTwo;
      default:
        throw new Error(`Invalid auxiliary device number: ${value}`);
    }
  }
  DeviceNumber.fromNumber = fromNumber;
})(DeviceNumber || (DeviceNumber = {}));
```

#### 進階範例

```typescript
enum Status {
  Pending = "pending",
  Approved = "approved",
  Rejected = "rejected",
}

namespace Status {
  export function isActive(status: Status): boolean {
    return status === Status.Approved;
  }

  export function getDisplayName(status: Status): string {
    const displayNames = {
      [Status.Pending]: "等待中",
      [Status.Approved]: "已核准",
      [Status.Rejected]: "已拒絕",
    };
    return displayNames[status];
  }

  export function getAllStatuses(): Status[] {
    return [Status.Pending, Status.Approved, Status.Rejected];
  }
}
```

這種模式特別適合以下場景：

* 需要為列舉添加工具方法
* 需要類型安全的轉換函數
* 需要集中管理列舉相關的邏輯
* 需要提供列舉的工廠方法

> TypeScript 的列舉型別的概念[來源於 C#](https://msdn.microsoft.com/zh-cn/library/sbbt4032.aspx)。

參考文章：

* https://medium.com/enjoy-life-enjoy-coding/javascript-es6-%E4%B8%AD%E6%9C%80%E5%AE%B9%E6%98%93%E8%AA%A4%E6%9C%83%E7%9A%84%E8%AA%9E%E6%B3%95%E7%B3%96-class-%E5%9F%BA%E6%9C%AC%E7%94%A8%E6%B3%95-23e4a4a5e8ed
* https://pjchender.dev/typescript/course-fullstack-typescript/
