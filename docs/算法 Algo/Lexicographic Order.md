---
publish: true
en_title: Lexicographic Order
---

比如 abc 的下一个字典序是 acb。从后向前扫描，如果有后面元素比前面元素“大”，则与之交换，即可得到下一个字典序。核心是我们希望前面的元素比后面的元素大，并且确保下一个是确定的，即可以依次获得所有字典序。所以我们会把字符串或者数组先排序，再依次得到所有的字典序。

For example, `abc`'s next lexicographic order is `acb`. Scan from right to left, if any element is "larger" than a previous element, swap with it, you get the next next lexicographic order. The key is we want elements to be "larger" than later elements, and make sure the next is deterministic and unique, so we can get all lexicographic orders one by one. Therefore we will sort the string or array and then get all the lexicographic orders.