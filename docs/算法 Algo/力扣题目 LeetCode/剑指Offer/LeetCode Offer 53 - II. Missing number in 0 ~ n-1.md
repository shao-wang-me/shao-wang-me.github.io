---
sr-due: 2023-06-03
sr-interval: 17
sr-ease: 290
publish: true
en_title: LeetCode Offer 53 - II. Missing number in 0 ~ n-1
tags:
  - algo
---


#algo

链接:: [剑指 Offer 53 - II. 0～n-1中缺失的数字 - 力扣（LeetCode）](https://leetcode.cn/problems/que-shi-de-shu-zi-lcof/)

> 输入: [0,1,3]
> 输出: 2
> 
> 输入: [0,1,2,3,4,5,6,7,9]
> 输出: 8
> 
> 1 <= 数组长度 <= 10000
> 
> 一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。
> 
> (my translation) An increasing array of length n-1 has unique integers in the range 0 ~ n-1. There is one and only one integer in the range 0 ~ n-1 that is not present in the array. Please find this integer.
> 
> 1 <= len(nums) <= 10000

由于是排序的，所以直接遍历即可得到缺失的数字，时间为 $O(n)$。但或许可以使用二分查找。看例子：

Because it is sorted, we can directly scan for the missing integer, but the time is $O(n)$. We can probably use binary search. Example:

```text
index    0,1,2,3,4,5,6,7,8,9
nums     0,1,2,3,4,6,7,8,9
compare  =,=,=,=,=,>,>,>,>
look in  >,>,>,>,>,!,<,<,<
```

所以实际上是找左边第一个 > index 的数字。

We are in fact searching for the first integer that > index.

```python
class Solution:
  def missingNumber(self, nums: List[int]) -> int:
    l, r = 0, len(nums) - 1
    while l <= r:
      m = (l + r) // 2
      if nums[m] == m:
        l = m + 1
      # can be written as else directly
      # writing down the condition here
      # only for clarity
      elif nums[m] > m:
        r = m - 1
      # l stops at the right spot
    return l
```