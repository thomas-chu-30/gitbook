# 03 extends implements

### 繼承(Interitance)

不知道大家有沒有覺得一直看到`extends`關鍵字？在看到類別也有`extends`關鍵字時，其實很疑惑介面和類別使用`extends`差異。目前的理解是介面和類別的擴展(Extension)/繼承(Class Inheritance)都是透過`extends`語法達成，但兩者的功能和意義不盡相同。

> **介面 v.s 類別擴展/繼承**
>
> 介面使用`extends`關鍵字，稱作介面擴展(Interface Extension)，也有介面繼承(Interface Inheritance)的說法; 而類別使用`extends`關鍵字則大部分視作類別繼承(Class Inheritance)。不論是介面或類別使用`extends`關鍵字都同樣形成父子的層級關係，舉例來說：interface A extends B 形成 A 為子 B 為父的關係，同樣的，Class C extends D 表示 D 為父類別 B 為子類別。
>
> 然而，介面和類別兩者使用`extends`後產生的功能和意義不盡相同。對於介面來說，interface A(介面) extends B(介面) 代表的含義是「若要實作子介面 A ，必須符合父介面 B 定義出來的規範。」，而對於類別來說，Class A(類別) extends B(類別) 則代表「子類別 A 可以使用父類別 B 所擁有的屬性和方法。」
>
> 另外，一個介面可以繼承多個介面(多重繼承)，但一個類別無法繼承多個類別，若真的要實現多重繼承，需要使用 Mixins 方法和`implements`關鍵字(下面會說明)。

[Day 20 的鐵人文章](https://ithelp.ithome.com.tw/articles/10224976)有初步介紹了類別繼承，[昨天](https://ithelp.ithome.com.tw/articles/10225366)也有提到不同存取裝飾符對於類別繼承的影響，這邊再來複習整理一下。

#### ⭐️ 類別繼承(Class Inheritance)

倘若宣告類別 A ，程式碼如下：

{% code title="A.ts" %}
```typescript
class A {
    public P1 : T1,
    private P2 : T2,
    protected P3 : T3
    static P4: T4

    public F1 (){...}
    private F2 (){...}
    protected F3 (){...}
    static F4(){...}
}


// 創建類別 B 使用 extends 語法繼承類別 A
class B extends A {}

//實例化類別 B
let obj = new B()
```
{% endcode %}

⭐&#xFE0F;_**這時候，類別 B 是子類別(Child Class)，類別 A 是父類別(Parent Class)。類別 B 具有以下特性：**_

* 類別 B 可以使用類別 A 中 非 private 的屬性和方法（除 P2 、 F2 外都可使用）
* 類別 B 使用 new 關鍵字實例化出來的物件，該物件型別除了屬性 B 類別之外，因為繼承緣故也同時屬於類別 A，但類別 A 所實例化出來的物件屬於 A 類別，但不屬於 B 類別。
* 類別 A 的靜態屬性 P4 和靜態方法 F4 不會出現在實例化物件中。另外，類別 A 中的 protected 和 private 屬性和方法 P2、P3、F2、F3 也無法在實例化物件中存取。

倘若 A 變成抽象類別呢？

{% code title="abstract-A.ts" %}
```typescript
abstract class A {
    public P1 : T1,
    private P2 : T2,
    protected P3 : T3
    static P4: T4

    public F1 (){...}
    private F2 (){...}
    protected F3 (){...}
    static F4(){...}
    abstract F5(){...}
}

class B extends A{ } // Error: 必須實作抽象方法F5
class B extends A {  // OK
    F5(){
        return 1
    }
}
```
{% endcode %}

昨天有提到`abstract`關鍵字，當類別中有抽象方法，則該類別就是抽象類別(Abstract Class)，必須在 class 前面加上 abstract 關鍵字，倘若類別 B 繼承了類別 A ，除了抽象方法 F5 必須實作外，在類別 B 中可以使用父類別 A 非 private 的屬性和方法。

**類別的多重繼承**

類別無法直接使用 extends 關鍵字進行多重繼承，例如：

{% code title="multi-extends-error.ts" %}
```typescript
class A{...}
class B{...}
class C extends A,B{ } //Error: Classes can only extend a single class.
```
{% endcode %}

但是類別可以透過 Mixins 方式達到多重繼承的效果，先定義兩個類別

{% code title="person-student.ts" %}
```typescript
// Person 類別
class Person {
  name: string;
  sayHello() {
    console.log("Hello");
  }
}

// Student 類別
class Student {
  grade: number;
  study() {
    console.log("I like Study");
  }
}
```
{% endcode %}

而後創建新的類別 SmartObject ，接著把這個類別當成介面使用，繼承 Person 和 Student 類別

{% code title="smartobject-interface.ts" %}
```typescript
class SmartObject {
    ...
}

interface SmartObject extends Person, Student  {}
```
{% endcode %}

最後將 mixins 混入定義的類別實踐(Class implementation)，並利用輔助函式進行混合

{% code title="applyMixins.ts" %}
```typescript
applyMixins(SmartObject, [Person, Student]);

//輔助函式
function applyMixins(derivedCtor: any, baseCtors: any[]) {
  baseCtors.forEach((baseCtor) => {
    Object.getOwnPropertyNames(baseCtor.prototype).forEach((name) => {
      Object.defineProperty(derivedCtor.prototype, name, Object.getOwnPropertyDescriptor(baseCtor.prototype, name));
    });
  });
}
```
{% endcode %}

### 實踐（Implements）

前面講到了 extends，現在要來討論 `implement`。`implement`關鍵字是做什麼用的呢？它和 extends 有什麼差別？

實踐(implements)通常用在實踐抽象的介面(Interface)。介面(Interface)主要表示抽象的行為，不能初始化屬性和方法，當類別實踐(implement)介面時就可以具體化行為了。

舉個例子來說，門是一個類別，防盜門是門的子類別，倘若防盜門有警報器的功能，現在如果有另外一個類別車子也有警報器的功能，這時候，就可以考慮把警報器功能提取出來成一個介面，讓防盜門和車子去實踐(implement)。

{% code title="alarm-example.ts" %}
```typescript
interface Alarm {
  alert(): void;
}

class Door {}

class SecurityDoor extends Door implements Alarm {
  alert() {
    console.log("SecurityDoor alert");
  }
}

class Car implements Alarm {
  alert() {
    console.log("Car alert");
  }
}
// 創建利用Car類別作為原型的新物件
const car: Car = new Car();
car.alert();
```
{% endcode %}

> ⭐️ 簡單的來說，只要是實踐過的物件，它就必需要有相對應的 function 在物件中。

從上面的程式碼可以看到在介面裡，我們定義了一個警報器的功能，但是具體的警報行為則是在車子類別和防盜門類別實踐(implement)的過程中才定義。

**一個類別可以實踐多個介面**

{% code title="multiple-implements.ts" %}
```typescript
interface Alarm {
  alert(): void;
}

interface Light {
  lightOn(): void;
  lightOff(): void;
}

class Car implements Alarm, Light {
  alert() {
    console.log("Car alert");
  }
  lightOn() {
    console.log("Car light on");
  }
  lightOff() {
    console.log("Car light off");
  }
}
```
{% endcode %}

上面的程式碼範例，Car 實踐了 Alarm 和 Light 介面，既能報警也能開關車燈。但要注意的是，所有介面所定義的屬性和方法都必須實踐，倘若少了 alert 、lightOn 或 lightOff 任一函式就會報錯。

### 小結

{% stepper %}
{% step %}
### 類別與介面的關係

類別會使用 extends 來建立子類別，類別會使用 implements 來實踐一個或多個介面。
{% endstep %}

{% step %}
### 子類別繼承父類別的特性

子類別繼承父類別後，可以使用父類別的屬性和方法（private 除外），也可以覆寫父類別的方法。
{% endstep %}

{% step %}
### 類別多重繼承

一個類別只能繼承一個類別，若要實現多個類別的功能（多重繼承）需要使用 Mixins。
{% endstep %}

{% step %}
### 介面的多重繼承

一個介面可以繼承多個介面，直接使用 `extends` 關鍵字，每個介面用逗號隔開即可。
{% endstep %}

{% step %}
### 類別實踐多個介面

一個類別可以實踐多個介面，直接使用 `implements` 關鍵字，每個介面用逗號隔開即可，類別中必須包含所有介面定義的屬性和方法。
{% endstep %}

{% step %}
### 多對多的實踐關係

多個類別可以實踐同一個介面，一個類別也可以實踐多個介面。
{% endstep %}
{% endstepper %}

[參考文章](https://ithelp.ithome.com.tw/articles/10225777)
