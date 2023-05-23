---
publish: true
en_title: LeetCode Offer 56 - II. Occurrences of numbers II
sr-due: 2023-06-06
sr-interval: 17
sr-ease: 290
tags:
  - algo
---


#algo 等会了什么数字逻辑、卡诺图再做位运算方法吧

链接:: [剑指 Offer 56 - II. 数组中数字出现的次数 II - 力扣（LeetCode）](https://leetcode.cn/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/)

和[[LeetCode Offer 56 - II. Occurrences of numbers II|上一题]]类似，只是一个数字出现一次，其它出现三次。

Like [[LeetCode Offer 56 - II. Occurrences of numbers II|the last problem]], just one number appears once, other numbers appear three times.

> 1 <= nums.length <= 10000
> 1 <= nums[i] < 2^31

异或就没有用了，但还是位运算。以 [3, 4, 3, 3] 为例：

XOR is not useful, but is still bitwise operation. Take [3, 4, 3, 3] as an example:

```text
3 => 0 1 1
3 => 0 1 1
3 => 0 1 1
4 => 1 0 0
----------
     1 3 3  统计逐位的 1 的个数 count the number of 1 bitwise
     1 0 0  用 3 整除得到的余数即答案数字 reminder by 3 is the answer
            不用担心会出现余数 2 remainer can't be 2
            因为这个余数必然是答案数字的二进制
            because the remainer must be a binary number
```

上面 1 的个数也不用真的统计个数，直接 0、1、2 三个数字就可以表示了。

但是二进制无法表示这三个数字，也就是无法使用一个二进制数直接使用位运算 reduce 到 0、1、2。

一种思路是使用两个二进制数表示两位，即 01、10、11 分别表示 0、1、2。很多题解称之为三进制，我觉得比较浑人，其实还是二进制与位运算，真正的三进制是 0、1、2。

```text
    | count
-----------
0 0 | 0
0 1 | 1
1 0 | 2
0 0 | 0 (back to 0)
```

总结下来就是，如果该位数字为 0 时不用更新。下面讨论该位数字为 1 的情况。

对于第二位，观察规律是 0 1 0, 0 1 0, 0 1 0，因此单凭第二位自身是无法判断的，只能确认 1 必然要变成 0。那么对于 0，是继续为 0 还是 变成 1，则是根据第一位。

若第二位是 0，当第一位是 0 时，第二位变成 1，当第一位是 1 时，第二位继续是 0。

综上，我们可以写出第二位的逻辑：

```python
if n == 0:
  two = two
if n == 1:
  if two == 1:
    two = 0
  if two == 0:
    two = ~one
```

力扣官方题解是继续化简为位运算，但是先算 one 再基于更新了的 two 算 one。我如果想先从 two 来或者两个单独来，不会化简，我看评论说到了数字逻辑、卡诺图，属于我不掌握的工具，今后再学习吧。

直接用哈希表统计数字出现的次数是最简单的：

```python
class Solution:
  def singleNumber(self, nums: List[int]) -> int:
    freqs = {}
    for n in nums:
      if n in freqs:
        freqs[n] += 1
      else:
        freqs[n] = 1
    for n in freqs:
      if freqs[n] == 1:
        return n
```

而且其实其实似乎比一众位运算方法不慢些。