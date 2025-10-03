---
description: 寫網頁的時候常常會有需控制DOM元素的時候，但是DOM尚未出來的話怎麼辦，只好用nextTick等它一下了。
---

# nextTick

nextTick讓你的function可以等到DOM元素出之後在執行你的function&#x20;

```javascript
//以vue 為例
created(){
    //DOM還沒有被render出來
    let DOM = document.getElementById('app')
    console.log(DOM) // null
}
mounted(){
    let DOM = document.getElementById('app')
    console.log(DOM) //<div id='app'></div>
}
```

但是如果加了nextTick在created裡面，這個時候DOM就會被找到喔。

```javascript
created(){
    //DOM還沒有被render出來
    this.nextTick(()=>{
        let DOM = document.getElementById('app')
        console.log(DOM) // <div id='app'></div>
    })
    
}
```
