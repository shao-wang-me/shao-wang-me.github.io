---
publish: true
en_title: LeetCode Offer 53 - I. Count Target Number in Sorted Array I
sr-due: 2023-05-28
sr-interval: 4
sr-ease: 270
tags:
  - algo
---


#algo 

相同：[[34. 在排序数组中查找元素的第一个和最后一个位置|34. 在排序数组中查找元素的第一个和最后一个位置]]

方法：[[二分|二分]]、[[二分上下界|二分上下界]]

> 统计一个数字在排序数组中出现的次数。

先二分查找数字，再统计个数。但是比较慢，可以先二分查找左边边界，再在左边边界的右边查找右边边界。题解写得都很 fancy，我写了比较好理解的版本：

```python
class Solution:
  def search(self, nums: List[int], target: int) -> int:
    def binary_search(nums, target, left: bool):
      l, r = 0, len(nums) - 1
      k = None
      while l <= r:
        m = (l + r) // 2
        # 此处显式写出 == 时的情况，更清晰
        if nums[m] == target:
          if left: r = m -1
          else: l = m + 1
          k = m
        elif nums[m] < target: l = m + 1
        else: r = m - 1
      return k

    left = binary_search(nums, target, True)
    if left == None:
      return 0
    right = binary_search(nums, target, False)
    return right - left + 1
```

最后的部分为了一点性能提升，可以：

```python
    left = binary_search(nums, target, True)
    if left == None:
      return 0
    right = binary_search(nums[left + 1:], target, False)
    if right == None:
      return 1
    return right + 2
```

或者可读性稍微高点：

```python
    left = binary_search(nums, target, True)
    if left == None:
      return 0
    right = binary_search(nums[left:], target, False) + left
    return right - left + 1
```