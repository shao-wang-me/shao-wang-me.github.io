---
publish: true
en_title: Josephus Problem
tags:
  - flashcards
---


#flashcards 

见[[./力扣题目 LeetCode/剑指Offer/LCOF 62. Last Number Left in the Cicle|LCOF 62. Last Number Left in the Cicle]]

约瑟夫环问题代码
?
```python
class Solution:
  def lastRemaining(self, n: int, m: int) -> int:
    ans = 0
    for i in range(2, n + 1):
      # k = m % i - 1
      # ans = (ans + k + 1) % i
      ans = (ans + m) % i
    return ans
```