---
publish: true
en_title: LeetCode Offer 61. Straight in Poker
sr-due: 2023-05-28
sr-interval: 3
sr-ease: 250
tags:
  - algo
---


#algo

链接：[剑指 Offer 61. 扑克牌中的顺子 - 力扣（LeetCode）](https://leetcode.cn/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)

1 - 13 都正常，0 可以代替任何数字。顺子代表数字都是连续的，且注意牌是乱序的！比较麻烦的就是 0。

那么可以排序之后，如果没有 0 简单判断，如果有 0 就看能不能合理插入，如果全是 0 也可以。

```python
class Solution:
  def isStraight(self, nums: List[int]) -> bool:
    nums.sort()
    k = 0  # 前四个数中 0 的个数
    for i in range(4):
      if nums[i] == 0:
        k += 1
      elif nums[i] == nums[i + 1]:
        return False  # 不能有重复
    if nums[4] - nums[k] < 5:  # 不要太远就都能用 0 填
      return True
    return False
```

或者统计 gap 大小：

```python
class Solution:
    def isStraight(self, nums: List[int]) -> bool:
        nums.sort()
        zeros = nums.count(0)
        
        last = None
        gap = 0
        for x in nums:
            if not last:
                if x:
                    last = x
            else:
                if x == last:
                    return False
                gap += x - last - 1
                last = x
        
        return gap <= zeros
```