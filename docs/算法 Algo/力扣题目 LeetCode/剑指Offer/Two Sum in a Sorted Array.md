---
publish: true
en_title: Two Sum in a Sorted Array
sr-due: 2023-06-15
sr-interval: 24
sr-ease: 290
tags:
  - algo
---


#algo

链接：[剑指 Offer 57. 和为s的两个数字 - 力扣（LeetCode）](https://leetcode.cn/problems/he-wei-sde-liang-ge-shu-zi-lcof/)

做法：[[双指针]]

递增数组，找两个数字，其和是给定目标数。

> 输入：nums = [10,26,30,31,47,60], target = 40
> 输出：[10,30] 或者 [30,10]

```text
10,26,30,31,47,60
l              r  70 > 40
怎样决定移动 l 还是 r？此处 70 已经大于 40 了，所以移动 r
l           r     57 > 40
l        r        41 > 40
l     r           40 = 40, return [10, 30]
```

```python
class Solution:
  def twoSum(self, nums: List[int], target: int) -> List[int]:
    l, r = 0, len(nums) - 1
    while l < r:
      sum_ = nums[l] + nums[r]
      if sum_ == target:
        return [nums[l], nums[r]]
      elif sum_ < target:
        l += 1
      else:
        r -= 1
    return []
```