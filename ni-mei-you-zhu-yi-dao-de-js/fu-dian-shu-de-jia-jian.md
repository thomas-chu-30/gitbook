# 浮點數的加減

### 情境描述

用Javascript來做加減乘除，如果是有`小數點`的數字的話，跑出來的數字會非常奇特，例如：

```javascript
const number1 = 5.10;
const number2 = 100.00;
console.log(number1 * number2);

// 509.99999999999994
```

### 原因

發現主要問題在於Javascript是用二進制來計算數值，之後再轉換成十進制，舉個例子，0.1 + 0.2，在Javascript會是這樣子計算的。

```javascript
// 0.1 和 0.2 都轉化成二進位制後再進行運算
0.00011001100110011001100110011001100110011001100110011010 
+
0.0011001100110011001100110011001100110011001100110011010 
=
0.0100110011001100110011001100110011001100110011001100111

// 轉成十進位制正好是 0.30000000000000004
```

### 解決方法

1.  #### 外部套件

    引用Math.js或者BigDecimal.js等，均可以順利解決浮點數運算的問題。
2. #### 自訂四則運算程式碼

```javascript
//除法
function accDiv(arg1,arg2){ 
    var t1=0,t2=0,r1,r2; 
    try {
        t1=arg1.toString().split(".")[1].length;
    } catch(e){} 
    try {
        t2=arg2.toString().split(".")[1].length;
    } catch(e){} 
    with(Math){ 
        r1=Number(arg1.toString().replace(".",""));
        r2=Number(arg2.toString().replace(".",""));
        return (r1/r2)*pow(10,t2-t1); 
    } 
}

//乘法
function accMul(arg1,arg2) 
{ 
    var m=0,s1=arg1.toString(),s2=arg2.toString(); 
    try {
        m+=s1.split(".")[1].length;
    } catch(e){} 
    try {
        m+=s2.split(".")[1].length;
    } catch(e){} 
    return Number(s1.replace(".",""))*Number(s2.replace(".",""))/Math.pow(10,m);
}

//加法
function accAdd(arg1,arg2){ 
    var r1,r2,m; 
    try{r1=arg1.toString().split(".")[1].length}catch(e){r1=0} 
    try{r2=arg2.toString().split(".")[1].length}catch(e){r2=0} 
    m=Math.pow(10,Math.max(r1,r2));
    return (arg1*m+arg2*m)/m;
} 

//減法
function accSubtr(arg1,arg2){
    var r1,r2,m,n;
    try {
        r1=arg1.toString().split(".")[1].length;
    } catch(e){r1=0}
    try {
        r2=arg2.toString().split(".")[1].length;
    } catch(e){r2=0}
    m=Math.pow(10,Math.max(r1,r2));
    n=(r1>=r2)?r1:r2;
    return ((arg1*m-arg2*m)/m).toFixed(n);
}
```

####

