# 07 todo

### 實例分享 (一)

```ts
export const sendEmail = async (payload: IMailPayload, file: File): Promise<any> => {
  const formData = new FormData();
  for (const key in payload) {
    formData.append(key, payload[key]); // 👈 這裡會有問題
  }
};
```

> Element implicitly has an 'any' type because expression of type 'string' can't be used to index type 'IMailPayload'. No index signature with a parameter of type 'string' was found on type 'IMailPayload'.ts(7053)

針對 `payload[key]` 部分, 有幾種改進的方法，以下將以 stepper 呈現：

{% stepper %}
{% step %}
### 使用 Object.entries()

```typescript
const formData = new FormData();
Object.entries(payload).forEach(([key, value]) => {
  formData.append(key, value);
});
```

這種方式可以直接遍歷 `payload` 對象的鍵值對，並將它們添加到 `FormData` 中。
{% endstep %}

{% step %}
### 使用 for...in 並檢查 own property

```typescript
const formData = new FormData();
for (const key in payload) {
  if (Object.prototype.hasOwnProperty.call(payload, key)) {
    formData.append(key, payload[key]);
  }
}
```

這種方式可以確保只對 `payload` 對象自身的屬性進行迭代，而不包括原型鏈上的屬性。
{% endstep %}

{% step %}
### 使用 Object.keys()

```typescript
const formData = new FormData();
Object.keys(payload).forEach((key) => {
  formData.append(key, payload[key]);
});
```

如果您確定 `payload` 是一個純對象 (non-prototype-based)，這種方式比較簡單，但要注意不要有來自原型鏈的屬性。
{% endstep %}

{% step %}
### 使用 for...of 搭配 Object.entries()

```typescript
const formData = new FormData();
for (const [key, value] of Object.entries(payload)) {
  formData.append(key, value);
}
```

結合前面方法的優點，通常在類型安全與可讀性之間取得不錯的平衡。
{% endstep %}
{% endstepper %}

選擇哪種方式取決於您的具體需求和 `payload` 對象的結構。如果您確定 `payload` 是一個純對象，Object.keys()（第三種）可能是最簡單的。如果您不確定 `payload` 的結構，使用 Object.entries()（第四種）通常比較安全。

***

### 實例分享 (二)

> 在 TypeScript 中，要表達 **互斥的邏輯（mutually exclusive types）**，通常使用的是 **聯合類型（union types）** 搭配 **discriminated union（判別聯合）** 或 **條件類型與 never 搭配排除不合法的組合**。

以下是兩種常見的寫法：

#### ✅ 方法一：使用 discriminated union（最常見）

這種方式會使用一個「判別字段」（如 type）來區分類型。

```ts
type A = {
  type: "a";
  aProp: string;
  bProp?: never; // 禁止出現 bProp
};

type B = {
  type: "b";
  bProp: number;
  aProp?: never; // 禁止出現 aProp
};

type AB = A | B;

// 用法示例
const example1: AB = {
  type: "a",
  aProp: "hello",
};

const example2: AB = {
  type: "b",
  bProp: 123,
};

// 錯誤示例（aProp 與 bProp 不應同時存在）
// const invalid: AB = {
//   type: 'a',
//   aProp: 'hello',
//   bProp: 123, // ❌ bProp 不應出現
// };
```

#### ✅ 方法二：排除非法組合（進階寫法）

這種方式可以用在沒有 discriminator 字段的情況下，例如：

```ts
type OnlyA = { a: string; b?: never };
type OnlyB = { a?: never; b: number };

type AorB = OnlyA | OnlyB;

// 正確：
const val1: AorB = { a: "hello" };
const val2: AorB = { b: 42 };

// 錯誤（互斥邏輯）
// const val3: AorB = { a: 'hello', b: 42 }; // ❌ 會錯
```

#### 延伸：多個選項互斥的情況

你可以利用工具類型來動態建立互斥關係（例如在表單選項中只允許其中一個出現）：

```ts
type XOR<T, U> = (T & { [K in keyof U]?: never }) | (U & { [K in keyof T]?: never });

type A = { a: string };
type B = { b: number };

type AorB = XOR<A, B>;

const good1: AorB = { a: "foo" };
const good2: AorB = { b: 123 };
// const bad: AorB = { a: 'foo', b: 123 }; // ❌
```
