---
publish: true
en_title: LeetCode Offer 57 - II. Sum of Continuous Positive Integers
sr-due: 2023-06-02
sr-interval: 11
sr-ease: 270
tags:
  - algo
---


#algo

链接：[剑指 Offer 57 - II. 和为s的连续正数序列 - 力扣（LeetCode）](https://leetcode.cn/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)

> 输入：target = 9
> 输出：[[2,3,4],[4,5]]
> 
> 输入：target = 15
> 输出：[[1,2,3,4,5],[4,5,6],[7,8]]

以 9 为例：

```text
1 2 3 4 5 6 7 8 9
      4 = 9 // 2
      数组的第一个元素不会比 4 更大
从 1 开始
1 2 3       6 < 9  右扩
1 2 3 4     10 > 9 左缩
  2 3 4     9 = 9  左缩
    3 4 5   12 = 9 左缩
      4 5   9 = 9  停止（因为已经是 4，且相等了，若大于也停止）
```

```python
class Solution:
  def findContinuousSequence(self, target: int) -> List[List[int]]:
    i = j = 1
    ans = []
    half = target // 2
    while i <= half:
      sum_ = (i + j) * (j - i + 1) / 2
      if sum_ < target:
        j += 1
      else:
        if sum_ == target:
          ans.append(list(range(i, j + 1)))
        if i == half:
          break
        i += 1
    return ans
```

用 `for` 循环的版本，慢一些，因为 j 的部分不够优化：

```python
class Solution:
  def findContinuousSequence(self, target: int) -> List[List[int]]:
    half = target // 2
    ans = []
    for i in range(1, half + 1):
      sum_ = i
      for j in range(i + 1, half + 2):
        sum_ +=  j
        if sum_ == target:
          ans.append(list(range(i, j + 1)))
        elif sum_ > target:
          break
    return ans
```