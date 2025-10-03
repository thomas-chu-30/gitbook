---
description: 正規表示式通常被稱為一個模式（pattern），為用來描述或者符合一系列符合某個句法規則的字串，透過他我們可以快速搜尋符合指定模式的文字。
---

# 正規表示式 Regular Expression

### 工具網站 <a href="#gong-ju-wang-zhan" id="gong-ju-wang-zhan"></a>

* [iHateRegex](https://ihateregex.io/) 查詢常用的正規表示式
* [RegExr - Regular Expression Online](http://regexr.com/) 線上測試正規表示式
* [JavaScript Regular Expression Visualizer](https://jex.im/regulex/) 視覺化呈現正規表示式的規則路徑

### 效能 <a href="#xiao-neng" id="xiao-neng"></a>

正規表示式可以幫助我們快速找到符合文字模式的字串，但執行效率未必”快速”，不精確的寫法還是很容易造成效能低落的問題。

以 C# 為例，有幾個提升效率的注意事項：

#### 能使用靜態全域變數時盡量用 <a href="#neng-shi-yong-jing-tai-quan-yu-bian-shu-shi-jin-liang-yong" id="neng-shi-yong-jing-tai-quan-yu-bian-shu-shi-jin-liang-yong"></a>

如下寫法，透過設定 `RegexOptions.Compiled` 可將該正規表達式編譯並成程式集，這設定雖然會增加啟動時間，但重複使用時，會有更好的執行速度。

```
readonly static Regex regex = new Regex("[ABC]", RegexOptions.Compiled);
```

另外還有一些設定項，可以參考看看，但不確定是否影響執行速度：

* `RegexOptions.IgnoreCase` 不區分大小寫，只限用在英文字
* `RegexOptions.Multiline` 多行模式
* `RegexOptions.Singleline` 單行模式

#### 小技巧 <a href="#xiao-ji-qiao" id="xiao-ji-qiao"></a>

* 善用 `^` 標示起始位置，如 `Regex("^Abc")` 只會找 `Abc` 開頭的字串，而 `aAbc` 就忽略
* 善用 `\b` 偵測字元邊界，和 `^` 意思很類似
* 善用 `.*?` 忽略後續字串，如 `Regex("^A.*?")` 只比對字串第一個字是否為 `A`，後面忽略

### 正規表示式說明 <a href="#zheng-gui-biao-shi-shi-shui-ming" id="zheng-gui-biao-shi-shi-shui-ming"></a>

| 正規表示式          | 說明及範例                                                                       | 比對不成立之字串      |
| -------------- | --------------------------------------------------------------------------- | ------------- |
| /a/            | 含字母 “a” 的字串，例如 “ab”, “bac”, “cba”                                           | “xyz”         |
| /a./           | 含字母 “a” 以及其後任一個字元的字串，例如 “ab”, “bac”（若要比對.，請使用 \\.）                          | “a”, “ba”     |
| /^xy/          | 以 “xy” 開始的字串，例如 “xyz”, “xyab”（若要比對 ^，請使用 \\^）                               | “axy”, “bxy”  |
| /xy$/          | 以 “xy” 結尾的字串，例如 “axy”, “abxy”以 “xy” 結尾的字串，例如 “axy”, “abxy” （若要比對 $，請使用 \\$） | “xya”, “xyb”  |
| \[13579]       | 包含 “1” 或 “3” 或 “5” 或 “7” 或 “9” 的字串，例如：”a3b”, “1xy”                          | “y2k”         |
| \[0-9]         | 含數字之字串                                                                      | 不含數字之字串       |
| \[a-z0-9]      | 含數字或小寫字母之字串                                                                 | 不含數字及小寫字母之字串  |
| \[a-zA-Z0-9]   | 含數字或字母之字串                                                                   | 不含數字及字母之字串    |
| b\[aeiou]t     | “bat”, “bet”, “bit”, “bot”, “but”                                           | “bxt”, “bzt”  |
| \[^0-9]        | 不含數字之字串（若要比對 ^，請使用 \\^）                                                     | 含數字之字串        |
| \[^aeiouAEIOU] | 不含母音之字串（若要比對 ^，請使用 \\^）                                                     | 含母音之字串        |
| \[^\\^]        | 不含 “^” 之字串，例如 “xyz”, “abc”                                                  | “xy^”, “a^bc” |



| 正規表示式的特定字元 | 說明       | 等效的正規表示式        |
| ---------- | -------- | --------------- |
| \d         | 數字       | \[0-9]          |
| \D         | 非數字      | \[^0-9]         |
| \w         | 數字、字母、底線 | \[a-zA-Z0-9\_]  |
| \W         | 非 \w     | \[^a-zA-Z0-9\_] |
| \s         | 空白字元     | \[ \r\t\n\f]    |
| \S         | 非空白字元    | \[^ \r\t\n\f]   |

\


| 正規表示式     | 說明                         |
| --------- | -------------------------- |
| /a?/      | 零或一個 a（若要比對? 字元，請使用 \\?）   |
| /a+/      | 一或多個 a（若要比對+ 字元，請使用 \\+）   |
| /a\*/     | 零或多個 a（若要比對\* 字元，請使用 \\\*） |
| /a{4}/    | 四個 a                       |
| /a{5,10}/ | 五至十個 a                     |
| /a{5,}/   | 至少五個 a                     |
| /a{,3}/   | 至多三個 a                     |
| /a.{5}b/  | a 和 b中間夾五個（非換行）字元          |

\


| 字元                  | 說明                                                                                                                                           | 簡單範例                                                                                        |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| \\                  | 避開[特殊字元](https://stackoverflow.com/questions/399078/what-special-characters-must-be-escaped-in-regular-expressions?answertab=active#tab-top) | /A\\\*/ 可用於比對 “A\*”，其中 \* 是一個特殊字元，為避開其特殊意義，所以必須加上 “\”                                       |
| ^                   | 比對輸入列的啟始位置                                                                                                                                   | /^A/ 可比對 “Abcd” 中的 “A”，但不可比對 “aAb”                                                          |
| $                   | 比對輸入列的結束位置                                                                                                                                   | /A$/ 可比對 “bcdA” 中的 “A”，但不可比對 “aAb”                                                          |
| \*                  | 比對前一個字元零次或更多次                                                                                                                                | /bo\*/ 可比對 “Good boook” 中的 “booo”，亦可比對 “Good bk” 中的 “b”                                     |
| +                   | 比對前一個字元一次或更多次，等效於 {1,}                                                                                                                       | /a+/ 可比對 “caaandy” 中的 “aaa”，但不可比對 “cndy”                                                    |
| ?                   | 比對前一個字元零次或一次                                                                                                                                 | /e?l/ 可比對 “angel” 中的 “el”，也可以比對 “angle” 中的 “l”                                              |
| .                   | 比對任何一個字元（但換行符號不算）                                                                                                                            | /.n/ 可比對 “nay, an apple is on the tree” 中的 “an” 和 “on”，但不可比對 “nay”                          |
| (x)                 | 比對 x 並將符合的部分存入一個變數                                                                                                                           | /(a\*) and (b\*)/ 可比對 “aaa and bb” 中的 “aaa” 和 “bb”，並將這兩個比對得到的字串設定至變數 RegExp.$1 和 RegExp.$2。 |
| xy                  | 比對 x 或 y                                                                                                                                     | /a\*b\*/g 可比對 “aaa and bb” 中的 “aaa” 和 “bb”                                                  |
| {n}                 | 比對前一個字元 n 次，n 為一個正整數                                                                                                                         | /a{3}/ 可比對 “lllaaalaa” 其中的 “aaa”，但不可比對 “aa”                                                 |
| {n,}                | 比對前一個字元至少 n 次，n 為一個正整數                                                                                                                       | /a{3,}/ 可比對 “aa aaa aaaa” 其中的 “aaa” 及 “aaaa”，但不可比對 “aa”                                     |
| {n,m}               | 比對前一個字元至少 n 次，至多 m 次，m、n 均為正整數                                                                                                               | /a{3,4}/ 可比對 “aa aaa aaaa aaaaa” 其中的 “aaa” 及 “aaaa”，但不可比對 “aa” 及 “aaaaa”                    |
| \[xyz]              | 比對中括弧內的任一個字元                                                                                                                                 | /\[ecm]/ 可比對 “welcome” 中的 “e” 或 “c” 或 “m”                                                   |
| \[^xyz]             | 比對不在中括弧內出現的任一個字元                                                                                                                             | /\[^ecm]/ 可比對 “welcome” 中的 “w”、”l”、”o”，可見出其與 \[xyz] 功能相反。（同時請注意 /^/ 與 \[^] 之間功能的不同。）        |
| \[\b]               | 比對退位字元（Backspace character）                                                                                                                  | 可以比對一個 backspace ，也請注意 \[\b] 與 \b 之間的差別                                                     |
| \b                  | 比對英文字的邊界，例如空格                                                                                                                                | <p>例如 /\bn\w/ 可以比對 “noonday” 中的 “no”<br>/\wy\b/ 可比對 “possibly yesterday.” 中的 “ly”</p>       |
| \B                  | 比對非「英文字的邊界」                                                                                                                                  | <p>例如, /\w\Bn/ 可以比對 “noonday” 中的 “on”<br>另外 /y\B\w/ 可以比對 “possibly yesterday.” 中的 “ye”</p>  |
| \cX                 | 比對控制字元（Control character），其中 X 是一個控制字元                                                                                                       | /\cM/ 可以比對 一個字串中的 control-M                                                                 |
| \d                  | 比對任一個數字，等效於 \[0-9]                                                                                                                           | /\[\d]/ 可比對 由 “0” 至 “9” 的任一數字 但其餘如字母等就不可比對                                                  |
| \D                  | 比對任一個非數字，等效於 \[^0-9]                                                                                                                         | /\[\D]/ 可比對 “w” “a”… 但不可比對如 “7” “1” 等數字                                                     |
| \f                  | 比對 form-feed                                                                                                                                 | 若是在文字中有發生 “換頁” 的行為 則可以比對成功                                                                  |
|                     | 比對換行符號                                                                                                                                       | 若是在文字中有發生 “換行” 的行為 則可以比對成功                                                                  |
|                     | 比對 carriage return                                                                                                                           |                                                                                             |
| \s                  | 比對任一個空白字元（White space character），等效於 \[ \f\n\r\t\v]                                                                                          | /\s\w\*/ 可比對 “A b” 中的 “b”                                                                   |
| \S                  | 比對任一個非空白字元，等效於 \[^ \f\n\r\t\v]                                                                                                               | /\S/\w\* 可比對 “A b” 中的 “A”                                                                   |
|                     | 比對定位字元（Tab）                                                                                                                                  |                                                                                             |
| \v                  | 比對垂直定位字元（Vertical tab）                                                                                                                       |                                                                                             |
| \w                  | 比對數字字母字元（Alphanumerical characters）或底線字母（“\_”），等效於 \[A-Za-z0-9\_]                                                                            | /\w/ 可比對 “.A \_!9” 中的 “A”、“\_”、“9”。                                                         |
| \W                  | 比對非「數字字母字元或底線字母」，等效於 \[^A-Za-z0-9\_]                                                                                                         | /\W/ 可比對 “.A \_!9” 中的 “.”、“ ”、“!”，可見其功能與 /\w/ 恰好相反。                                         |
| \&#x6F;_&#x6F;ctal_ | 比對八進位，其&#x4E2D;_&#x6F;ctal是八進位數目_                                                                                                            | /\oocetal123/ 可比對 與 八進位的ASCII中 “123” 所相對應的字元值。                                              |
| \&#x78;_&#x68;ex_   | 比對十六進位，其&#x4E2D;_&#x68;ex是十六進位數目_                                                                                                            | /\xhex38/ 可比對 與 16進位的ASCII中 “38” 所相對應的字元。                                                   |
| ?:                  | 不予擷取的群組，括號括住的部份不列入 Group 中                                                                                                                   | (a(b\*))+ 會有 Group#1 和 Group#2 但是 (a(?:b\*))+ 只會有 Group#1 一個群組                              |

REF:

* [不予擷取的群組](https://dotblogs.com.tw/johnny/2010/03/02/13855)

### 常用範例 <a href="#chang-yong-fan-li" id="chang-yong-fan-li"></a>

* IPv4

```regex
/^((?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?))*$/
```

* MAC \_ IEEE 802 MAC-48 標準格式 \_ 6 組由 `:` 或 `-` 做區隔的雙位數 16 進制數字

```
/^([0-9A-Fa-f]{2}[:-]){5}([0-9A-Fa-f]{2})$/
```

* 驗證使用者帳號，第一個字不為數字，只接受 大小寫字母、數字及底線

```regex
/^[a-zA-Z]\w*$/
```

* 密碼 \_ 高強度密碼，6 位數以上，並且至少包含**大寫字母**、**小寫字母**、**數字**、**符號**各一 \_ 若需要調整，將其對應的小括號內容拿掉即可

```regex
/^(?=.*[^a-zA-Z0-9])(?=.*[A-Z])(?=.*[a-z])(?=.*\d).{6,}$/
```

* 電子郵件，以下的範例並沒有相容 RFC5322 規範，但是已經可以驗證大多數的電子郵件

```regex
/^([a-zA-Z0-9._%-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,4})*$/
```

* URL 網址，允許 http, https, ftp 協定，並且可取出 Protocol, Domain, Path, Query

```regex
/^(?:(https?|ftp):\/\/)?((?:[a-zA-Z0-9.\-]+\.)+(?:[a-zA-Z0-9]{2,4}))((?:/[\w+=%&.~\-]*)*)\??([\w+=%&.~\-]*)$/
```

* 主流信用卡

```regex
/^(?:4[0-9]{12}(?:[0-9]{3})?|5[1-5][0-9]{14}|6011[0-9]{12}|622((12[6-9]|1[3-9][0-9])|([2-8][0-9][0-9])|(9(([0-1][0-9])|(2[0-5]))))[0-9]{10}|64[4-9][0-9]{13}|65[0-9]{14}|3(?:0[0-5]|[68][0-9])[0-9]{11}|3[47][0-9]{13})*$/
```

* 美國運通信用卡

```regex
/^(3[47][0-9]{13})*$/
```

* MasterCard

```regex
/^(5[1-5][0-9]{14})*$/
```

* Visa 卡

```regex
/^(4[0-9]{12}(?:[0-9]{3})?)*$/
```

* 日期 (MM/DD/YYYY)

```regex
/^((0?[1-9]|1[012])[- /.](0?[1-9]|[12][0-9]|3[01])[- /.](19|20)?[0-9]{2})*$/
```

* 日期 (YYYY/MM/DD)

```regex
/^(((?:19|20)[0-9]{2})[- /.](0?[1-9]|1[012])[- /.](0?[1-9]|[12][0-9]|3[01]))*$/
```

* 台灣手機號碼

```regex
/^09\d{2}-?\d{3}-?\d{3}$/
```

* 中文 (Unicode)

```regex
[\u4e00-\u9fa5]
```

* 簡易驗證台灣身份證，仍然需要一些進階的檢查，&#x5982;_&#x9A57;證檢查碼_，或前往[內政部戶政司](http://www.ris.gov.tw/zh_TW/307)驗證

```regex
/^[A-Za-z][1-2]\d{8}$/
```

* 正整數

```regex
/^\+?\d+$/
```

* 整數

```regex
/^[+-]?\d+$/
```

* float

```regex
/^[-+]?[0-9]*\.?[0-9]+([eE][-+]?[0-9]+)?$/
```

***

參考資料：

* [RegExr - Regular Expression Online](http://regexr.com/)
* [就是愛程式 - 正規表示式 Regular Expression](https://atedev.wordpress.com/2007/11/23/%E6%AD%A3%E8%A6%8F%E8%A1%A8%E7%A4%BA%E5%BC%8F-regular-expression/)
* [A Single Page Perl Regular Expression Quick Reference](http://www.erudil.com/pdf/preqr.pdf)
* [What special characters must be escaped in regular expressions?](https://stackoverflow.com/questions/399078/what-special-characters-must-be-escaped-in-regular-expressions?answertab=active#tab-top)
* [JavaScript Regular Expression Visualizer](https://jex.im/regulex/)
* [i Hate Regex](https://ihateregex.io/)
