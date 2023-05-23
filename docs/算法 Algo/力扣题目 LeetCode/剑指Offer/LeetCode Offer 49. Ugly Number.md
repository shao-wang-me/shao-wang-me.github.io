---
publish: true
en_title: LeetCode Offer 49. Ugly Number
sr-due: 2023-07-08
sr-interval: 48
sr-ease: 270
tags:
  - algo
---


#algo

链接：[剑指 Offer 49. 丑数 - 力扣（LeetCode）](https://leetcode.cn/problems/chou-shu-lcof/)

相同：[[../../../../264. 丑数 II|264. 丑数 II]]

做法：[[../../../../动态规划|动态规划]]

输出第 n 个丑数（只包含质因子 2、3、5 的数）。例如 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。

Output the nth ugly number (integer that has only 2, 3 and 5 as prime factors). For example, the first 10 ugly numbers are 1, 2, 3, 4, 5, 6, 8, 9, 10, 12.

直觉是使用[[../../../../动态规划|动态规划]]，维护前面的丑数，对下一个丑数，我们考虑是多增加 2、3 或 5。

The intuition is to use [[../../../../动态规划|dynamic programming]], maintain previous ugly numbers. For the next ugly number, we consider multiply by 2, 3 or 5.

| number | prime factors |
| ------ | ------------- |
| 1      | 1             |
| 2      | 2             |
| 3      | 3             |
| 4      | 2,2           |
| 5      | 5             |
| 6      | 2,3,3         |
| 8      | 2,2,2         |
| 9      | 3,3           |
| 10     | 2,5           |
| 12     | 2,2,3         |

下一个丑数一定是之前某一个丑数乘以 2、3 或 5 得到的。那么维护之前乘以 2、3 或 5 的指针，一开始都是在 1 的位置上。每次用 2、3 或 5 乘以一个数字的时候，我们就将该指针向右移动。

Next ugly number must be a previous ugly number multiplied by 2, 3 or 5. We maintain pointers for 2, 3 and 5 respectively. Every time we multiply by 2, 3 or 5, we increment the pointer by one.

```python
class Solution:
  def nthUglyNumber(self, n: int) -> int:
    dp = [1]
    p2 = p3 = p5 = 0
    for __ in range(1, n):
      u2, u3, u5 = dp[p2] * 2, dp[p3] * 3, dp[p5] * 5
      dp.append(min(u2, u3, u5))
      if dp[-1] == u2:
        p2 += 1
      if dp[-1] == u3:  # 一定不要使用 elif，否则无法避免重复！
        p3 += 1         # don't use elif, otherwise can't avoid duplicates!
      if dp[-1] == u5:
        p5 += 1
    return dp[-1]
```