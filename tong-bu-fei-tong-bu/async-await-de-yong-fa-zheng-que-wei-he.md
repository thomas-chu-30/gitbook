---
description: >-
  async await 是在 ES7 之後才出來的 JS 語法，相較起 promise, then 等用寫法，可讀性更高，非常推薦使用。也可以搭配 try
  catch 輕鬆處理 error handling 的問題。
---

# async await

### 起手式

在 function 的前面加上 async ，在你需要等待的地方加 await ，就可以等到此行的 JS 執行完成了，在接續下一行指令。

```javascript
❌ Bad
async getchild(){
    await this.$http.get('api')
        .then(r = >{
            consol.log(r.data.Data)
            return r.data.Data
        }).catch(e=>{
            this.$stdErr(err. methodName)
        })
}
⭕️ Good
async getchild(){
    const map = await this.$http.get('api');
    return map.data.Data
}

async getchild(){
    let map = ''
    try { map = await this.$http.get('api');}
    catch (e) {alert(e);};
    
    return map.data.Data
 }
```

### try catch

基礎的在 try 的區域中寫上你要執行的非同步的程式，在發生錯誤時，可以在 catch 中取回錯誤訊息，最後可以在 finally 中在執行一段，結束後會跑的程式。

```javascript
try {
    // await callAPI()
} catch(error) {
    // console.error(error)
} finally {
    console.log("finally")
}
```
