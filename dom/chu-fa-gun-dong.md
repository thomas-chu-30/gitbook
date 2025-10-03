# 觸發滾動



### 寬度/高度 <a href="#kuan-du-gao-du" id="kuan-du-gao-du"></a>

關於元素的寬度和高度，有三組屬性可以使用，分別是`offsetWidth`/`offsetHeight`，`clientWidth`/`clientHeight`，及`scrollWidth`/`scrollHeight`。雖然名字很像，但是功用卻大不同！下面就讓我們來看看這三組屬性的差別。

> 注意：再計算寬度或高度之前，必須先問自己：邊界 (border) 及捲軸的寬度/高度需要被包含在內嗎？

* `offsetWidth`/`offsetHeight` 是「元素本身」的寬度/高度，並完整了包含了邊界、捲軸及padding。
* `clientWidth`/`clientHeight` 則是元素所包含的「子元素」的寬度/高度，其中包含了padding，但不包含邊界及捲軸。
* `scrollWidth`/`scrollHeight` 也是元素所包含的「子元素」的「完整」寬度和高度，其中包含了超出捲軸之外的部分的寬度與高度。在沒有捲軸的情況下，這個值就等於 `clientWidth`/`clientHeight`。



<div align="center"><img src="broken-reference" alt=""></div>

<div align="center"><img src="broken-reference" alt=""></div>

## 獲取DOM的相關尺寸

javascript中获取dom元素高度和宽度的方法如下：

网页可见区域宽： document.body.clientWidth\
网页可见区域高： document.body.clientHeight\
网页可见区域宽： document.body.offsetWidth (包括边线的宽)\
网页可见区域高： document.body.offsetHeight (包括边线的高)\
网页正文全文宽： document.body.scrollWidth\
网页正文全文高： document.body.scrollHeight\
网页被卷去的高： document.body.scrollTop\
网页被卷去的左： document.body.scrollLeft

对应的dom元素的宽高有以下几个常用的：

元素的实际高度：document.getElementById("div").offsetHeight\
元素的实际宽度：document.getElementById("div").offsetWidth\
元素的实际距离左边界的距离：document.getElementById("div").offsetLeft\
元素的实际距离上边界的距离：document.getElementById("div").offsetTop

## 移動到指定位子

&#x20;js-頁面開啟後滾動到特定位置的效果

滾動頁面的方法有scroll、scrollBy和scrollTo,三個方法都帶兩個引數:x(X軸上的偏移量)和y(Y軸上的偏移量)。因為是要滾動到頁面底部,所以引數x為0,y為頁面的滾動高度。另外,頁面的滾動高度必須在網頁載入完成後才能獲取到,所以觸發事件用onload。 \
具體步驟: \
方法一:用scroll方法實現。 \
\<body onload="scroll(0,document.body.scrollHeight) "> \
\<script> \
document.write(new Array(100).join("\<br>")) \
\</script> \
方法二:用scrollBy方法實現。 \
\<body onload="scrollBy(0,document.body.scrollHeight) "> \
\<script> \
document.write(new Array(100).join("\<br>")) \
\</script> \
方法三:用scrollTo方法實現。 \
\<body onload="scrollTo(0,document.body.scrollHeight)"> \
\<script> \
document.write(new Array(100).join("\<br>")) \
\</script>

注意:因為頁面載入完後預設滾動在最頂端,所以在本例中用scroll、scrollBy和scrollTo方法的效果一樣,然而它們之間其實是有區別的。

scroll()  此方法接收两个参数，依次为X坐标和Y坐标；设置滚动条的偏移位置

scrollTo() 此方法和scroll()作用一样，都是设置滚动条的偏移位置。

scrollBy() 此法发同样接收两个参数，不过参数分别为X轴的偏移量和Y轴的偏移量，并且可以增加或者减少。

scroll()例子： scroll(0, 200)  ==>  设置滚动条Y轴位置在200像素的地方。比如：当前坐标为0，执行后便是200，当前坐标为100，执行后是200。

scrollTo()例子： scrollTo(0, 200) ==> 同scroll()方法,设置Y轴在200像素的位置。

scrollBy()例子：scrollBy(0, 200) ==> 使得滚动条Y轴的位置，在当前的基础上增加200。比如：当前Y轴位置为0，执行后便是200；当前为100，执行后便是300。





{% embed url="https://shubo.io/element-size-scrolling/" %}

