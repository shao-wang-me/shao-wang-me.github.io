---
publish: true
en_title: LeetCode Offer 51. Count Inversions
sr-due: 2023-06-13
sr-interval: 16
sr-ease: 290
tags:
  - algo
---


#algo 离散化树状数组做法

链接：[剑指 Offer 51. 数组中的逆序对 - 力扣（LeetCode）](https://leetcode.cn/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

做法：[[归并排序|归并排序]]；离散化树状数组？

> 输入: [7,5,6,4]
> 输出: 5
> 
> 0 <= 数组长度 <= 50000

上面例子中，我们有五个逆序对：

```text
7 5
7 6
7 4
5 4
6 4
```

最后返回的是逆序对的数量，而不是所有逆序对。

如果选取一个数字，和后续每个数字比较，时间是 $O(n^2)$。而在归并排序时，如果有先从右边拿数字的情况，则此时左边都比它大，即可得到逆序对数量。

对但是超时的归并：

```python
class Solution:  # merge sort
  def reversePairs(self, nums: List[int]) -> int:
    ans = 0

    def merge_sort(nums):
      n = len(nums)
      if n <= 1:
        return nums
      m = n // 2
      left = merge_sort(nums[:m])
      right = merge_sort(nums[m:])
      # now merge left and right
      # and count reverse pairs
      nums = []
      nonlocal ans
      while left and right:
        if left[0] > right[0]:
          ans += len(left)
          nums.append(right.pop(0))
        else:
          nums.append(left.pop(0))
      nums += left or right or []
      return nums

    merge_sort(nums)
    return ans
```

需要用 index 而不是直接返回数组提高效率：

```python
class Solution:
  def reversePairs(self, nums: List[int]) -> int:
    ans = 0

    def merge_sort(l, r):
      if r - l <= 1:
        return
      m = (l + r) // 2
      merge_sort(l, m)
      merge_sort(m, r)
      # merge [l, m) and [m, r)
      nonlocal ans
      i, j = l, m
      tmp = []
      while i < m and j < r:
        if nums[i] > nums[j]:
          ans += m - i
          tmp.append(nums[j])
          j += 1
        else:
          tmp.append(nums[i])
          i += 1
      tmp += nums[i:m] or nums[j:r]
      for k, n in enumerate(tmp):
        nums[l + k] = n

    merge_sort(0, len(nums))
    return ans
```