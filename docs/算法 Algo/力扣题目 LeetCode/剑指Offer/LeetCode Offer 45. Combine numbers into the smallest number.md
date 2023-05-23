---
publish: true
en_title: LeetCode Offer 45. Combine numbers into the smallest number
sr-due: 2023-07-26
sr-interval: 65
sr-ease: 290
tags:
  - algo
---


#algo

链接：[剑指 Offer 45. 把数组排成最小的数 - 力扣（LeetCode）](https://leetcode.cn/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)

> 输入: [10,2]
> 输出: "102"

10 比 2 小，因为 1 比 2 小。

> 输入: [3,30,34,5,9]
> 输出: "3033459"

30 比 3 小，因为否则 3 后面肯定接的一位比 0 大。如果是 32 呢？如果数组中还有一个 1 开头的，那么似乎应该先接 3，但是注意如果有 1 开头的，则应该早就被用过了。所以 32 还是比 3 小。33 呢？应该和 3 是一样的，拼出来都是 333。但是 34 肯定比 33 和 3 都大了。

我们不能直接用字符串的比较大小，因为那样 32 > 3。我们要逐位比较，即 3 = 3，而位数少的那个数字补前面的相同数字，即 32 < 33。

另一种比较的方式更简单粗暴，即直接比较字符拼接结果，绝对是更好的办法，即比较 x + y 与 y + x，这里 x 和 y 是数字的字符串表示。

```python
def compare(x, y):
  x, y = str(x), str(y)
  s, t = x + y, y + x
  # functools.cmp_to_key() 需要返回 -1、0、1
  return -1 if s < t else 0 if s == t else 1
```

使用 `functools.cmp_to_key`：

```python
from functools import cmp_to_key

class Solution:
  def minNumber(self, nums: List[int]) -> str:
    def compare(x, y):
      n1, n2 = x + y, y + x
      return -1 if n1 < n2 else 0 if n1 == n2 else 1
    
    return ''.join(sorted([str(n) for n in nums], key=cmp_to_key(compare)))
```

也可以用[[../../../../快速排序|快速排序]]：

```python
from random import randint

class Solution:  # quicksort
  def minNumber(self, nums: List[int]) -> str:
    def compare(x, y):
      return x + y < y + x

    def partition(nums, l, r):
      p = randint(l, r)
      pivot = nums[p]
      nums[l], nums[p] = nums[p], nums[l]
      i, j = l + 1, r
      while i <= j:
        x, y = str(nums[i]), str(pivot)
        if x + y < y + x:
          i += 1
          continue
        x = str(nums[j])
        if x + y >= y + x:
          j -= 1
          continue
        nums[i], nums[j] = nums[j], nums[i]
      nums[l], nums[j] = nums[j], nums[l]
      return j

    def quicksort(nums, l, r):
      if l < r:
        k = partition(nums, l, r)
        quicksort(nums, l, k - 1)
        quicksort(nums, k + 1, r)
    
    quicksort(nums, 0, len(nums) - 1)
    return ''.join(map(lambda n: str(n), nums))
```