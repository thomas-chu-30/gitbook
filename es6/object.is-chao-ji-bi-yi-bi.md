# Object.is() == === 超級比一比

| X                  | Y                  |        ==        |     ===     |     Object.js    |
| ------------------ | ------------------ | :--------------: | :---------: | :--------------: |
| undefined          | undefined          |      _true_      |    _true_   |      _true_      |
| null               | null               |      _true_      |    _true_   |      _true_      |
| true               | true               |      _true_      |    _true_   |      _true_      |
| false              | false              |      _true_      |    _true_   |      _true_      |
| 'foo'              | 'foo'              |      _true_      |    _true_   |      _true_      |
| 0                  | 0                  |      _true_      |    _true_   |      _true_      |
| +0                 | -0                 | t&#x72;_&#x75;e_ |    _true_   |    _**false**_   |
| 0                  | false              | t&#x72;_&#x75;e_ | _**false**_ |    _**false**_   |
| ""                 | false              | t&#x72;_&#x75;e_ | _**false**_ |    _**false**_   |
| ""                 | 0                  | t&#x72;_&#x75;e_ | _**false**_ |    _**false**_   |
| "0"                | 0                  | t&#x72;_&#x75;e_ | _**false**_ |    _**false**_   |
| "17"               | 17                 | t&#x72;_&#x75;e_ | _**false**_ |    _**false**_   |
| \[1,2]             | "1,2"              | t&#x72;_&#x75;e_ | _**false**_ |    _**false**_   |
| new String ("foo") | "foo"              | t&#x72;_&#x75;e_ | _**false**_ |    _**false**_   |
| null               | undefined          | t&#x72;_&#x75;e_ | _**false**_ |    _**false**_   |
| null               | false              |    _**false**_   | _**false**_ |    _**false**_   |
| undefined          | false              |    _**false**_   | _**false**_ |    _**false**_   |
| {foo:"bar"}        | {foo:"bar"}        |    _**false**_   | _**false**_ |    _**false**_   |
| new String ("foo") | new String ("foo") |    _**false**_   | _**false**_ |    _**false**_   |
| 0                  | null               |    _**false**_   | _**false**_ |    _**false**_   |
| 0                  | NaN                |    _**false**_   | _**false**_ |    _**false**_   |
| "foo"              | NaN                |    _**false**_   | _**false**_ |    _**false**_   |
| NaN                | NaN                |    _**false**_   | _**false**_ | t&#x72;_&#x75;e_ |

<div align="left"><img src="../.gitbook/assets/pCyqkLc.png" alt=""></div>
