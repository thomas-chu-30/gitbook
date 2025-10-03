# 05 interface merged

在 TypeScript 開發中，我們經常需要定義複雜的型別結構。有時候，我們會遇到需要將多個相同名稱的函式、介面或類別合併成一個的情況。TypeScript 提供了一個強大的特性叫做「宣告合併」（Declaration Merging），讓我們可以更靈活地組織和擴展型別定義。

### 什麼是宣告合併？

宣告合併是 TypeScript 的一個特性，當你定義了兩個或多個相同名字的函式、介面或類別時，TypeScript 會自動將它們合併成一個型別。這個特性讓我們可以將型別定義分散在多個地方，然後在編譯時自動合併。

### 函式的合併

函式合併是宣告合併中最常見的應用，也就是我們熟知的函式過載（Function Overloading）。

{% hint style="info" %}
下面範例展示如何定義多個函式過載，並提供一個實作來處理不同的輸入型別。
{% endhint %}

{% code title="reverse.ts" %}
```typescript
// 定義多個函式過載
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
  if (typeof x === "number") {
    return Number(x.toString().split("").reverse().join(""));
  } else if (typeof x === "string") {
    return x.split("").reverse().join("");
  }
}

// 使用範例
const numResult = reverse(123); // 回傳 321
const strResult = reverse("hello"); // 回傳 'olleh'
```
{% endcode %}

在上面的例子中，我們定義了兩個函式過載，分別處理 `number` 和 `string` 類型的參數，最後提供實際的實作。TypeScript 會將這些宣告合併，提供正確的型別檢查。

### 介面的合併

介面合併是另一個強大的特性，它允許我們將多個同名介面的屬性合併到一個介面中。

#### 基本屬性合併

{% code title="alarm-basic.ts" %}
```typescript
interface Alarm {
  price: number;
}

interface Alarm {
  weight: number;
}

// 上述兩個介面會自動合併成：
interface Alarm {
  price: number;
  weight: number;
}
```
{% endcode %}

#### 型別一致性要求

在介面合併時，**相同屬性的型別必須保持一致**：

{% code title="alarm-type-consistency.ts" %}
```typescript
// ✅ 正確：型別一致
interface Alarm {
  price: number;
}

interface Alarm {
  price: number; // 型別相同，不會報錯
  weight: number;
}

// ❌ 錯誤：型別不一致
interface Alarm {
  price: number;
}

interface Alarm {
  price: string; // 型別不一致，會報錯
  weight: number;
}

// 編譯錯誤：
// error TS2403: Subsequent variable declarations must have the same type.
// Variable 'price' must be of type 'number', but here has type 'string'.
```
{% endcode %}

#### 方法合併

介面中的方法合併與函式合併的規則相同：

{% code title="alarm-methods.ts" %}
```typescript
interface Alarm {
  price: number;
  alert(s: string): string;
}

interface Alarm {
  weight: number;
  alert(s: string, n: number): string;
}

// 合併後的介面：
interface Alarm {
  price: number;
  weight: number;
  alert(s: string): string;
  alert(s: string, n: number): string;
}
```
{% endcode %}

### 實際應用場景

#### 擴展第三方函式庫

{% code title="jquery-extend.ts" %}
```typescript
// 擴展 jQuery 的型別定義
interface JQuery {
  tooltip(): JQuery;
  tooltip(options: any): JQuery;
}

// 在其他地方繼續擴展
interface JQuery {
  modal(): JQuery;
  modal(options: any): JQuery;
}
```
{% endcode %}

#### 模組化型別定義

```typescript
// user.ts
interface User {
  id: number;
  name: string;
}

// user-permissions.ts
interface User {
  permissions: string[];
  isAdmin: boolean;
}

// user-profile.ts
interface User {
  email: string;
  avatar?: string;
}

// 最終合併的 User 介面包含所有屬性
```

#### 配置物件擴展

```typescript
interface Config {
  apiUrl: string;
  timeout: number;
}

// 在開發環境中擴展
interface Config {
  debug: boolean;
  logLevel: "info" | "warn" | "error";
}

// 在生產環境中擴展
interface Config {
  enableAnalytics: boolean;
  cacheSize: number;
}
```

### 注意事項

{% stepper %}
{% step %}
### 型別一致性

合併的屬性型別必須完全一致，否則會產生編譯錯誤。
{% endstep %}

{% step %}
### 命名空間

宣告合併只在全域範圍或模組內部有效，得注意宣告所在的作用域。
{% endstep %}

{% step %}
### 執行順序

合併的順序會影響最終的型別定義（例如同一屬性若在不同檔案以不同方式擴展，載入順序可能會影響可見性或錯誤）。
{% endstep %}

{% step %}
### 可讀性

過度使用宣告合併會讓程式碼難以理解，務必保持清晰的模組與檔案組織。
{% endstep %}
{% endstepper %}

### 總結

宣告合併是 TypeScript 中一個非常實用的特性，它讓我們可以：

* 模組化型別定義：將複雜的型別定義分散到多個檔案中
* 擴展第三方函式庫：為現有的函式庫添加型別定義
* 保持程式碼組織性：避免單一檔案過於龐大

適當地使用宣告合併可以讓你的 TypeScript 程式碼更加靈活和可維護。不過也要注意不要過度使用，以免影響程式碼的可讀性。

希望這篇文章能幫助你更好地理解和應用 TypeScript 的宣告合併特性！
