---
publish: true
en_title: LeetCode Offer 58 - II. Left shift string
sr-due: 2023-05-22
sr-interval: 4
sr-ease: 270
tags:
  - algo
---


#algo

链接：[剑指 Offer 58 - II. 左旋转字符串 - 力扣（LeetCode）](https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

> 输入: s = "abcdefg", k = 2
> 输出: "cdefgab"

```python
class Solution:
  def reverseLeftWords(self, s: str, n: int) -> str:
    return s[n:] + s[:n]
```