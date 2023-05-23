---
publish: true
en_title: LeetCode Offer 46. Convert Number to String
sr-due: 2023-06-28
sr-interval: 38
sr-ease: 270
tags:
  - algo
---


#algo

链接：[剑指 Offer 46. 把数字翻译成字符串 - 力扣（LeetCode）](https://leetcode.cn/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)

方法：[[递归]]、[[../../../../动态规划|动态规划]]

> 输入: 12258
> 输出: 5
> 解释: 12258有5种不同的翻译，分别是"bccfi", "bwfi", "bczi", "mcfi"和"mzi"

有没有可能一个数字无法翻译为字符串？不可能，因为个位数总能对应到一个字符。那么就要看接下来的数字在 0 - 25 之内的所有可能性。例如，12258 的第一个字符可以是 1 对应的 b，也可以是 12 对应的 m，如此往后，便可以用递归解决问题。

由于 n 满足 `0 <= num < 2^31`，n 可能很大，所以递归可能会栈溢出，因此使用动态规划（或自顶向下的动态规划即记忆化搜索）。无论如何找到状态转移函数：`f(i) = f(i - 1) + (f(i - 2) if '10' <= s[i-1:i+1] <= '25' else 0)`，为了解决 i - 1 和 i - 2 在开头的问题，在 DP 数组开头预置两个  0。又注意到这是一个一维数组，并且仅仅依赖前面个两个数字，我们可以进维护两个值。

```python
class Solution:
  def translateNum(self, num: int) -> int:
    x, y, num = 1, 1, str(num)
    for i in range(len(num)):
      z = y
      if '10' <= num[i-1:i+1] <= '25':
        z += x
      x, y = y, z
    return y
```

这个思路与官方题解一致，大家基本上都使用这个方法。只有一个解法利用只有 0 - 26，使用 n % 100 来获取后面的两个字母，但是这就是递归了（虽然应该也能改成 DP，不好写就是了）。