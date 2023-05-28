---
publish: true
en_title: LCOF 47. Maximum Value of Presents
sr-due: 2023-05-28
sr-interval: 19
sr-ease: 290
tags:
  - algo
---


#algo

链接：[剑指 Offer 47. 礼物的最大价值 - 力扣（LeetCode）](https://leetcode.cn/problems/li-wu-de-zui-da-jie-zhi-lcof/)

做法：[[动态规划|动态规划]]

从左上到右下的路径和的最大值，DP 问题。`f(i, j)` 基于 `f(i-1,j)` 和 `f(i,j-1)`，每次取这两个的最大值再加上本格子的值即可。

```python
class Solution:
  def maxValue(self, grid: List[List[int]]) -> int:
    m = len(grid)
    n = len(grid[0])
    dp = [[0] * n for _ in range(m)]
    dp[0][0] = grid[0][0]
    for i in range(m):
      for j in range(n):
        dp[i][j] = grid[i][j] + max(i and dp[i-1][j], j and dp[i][j-1])
    return dp[-1][-1]
```

观察到由于我们只需要两个值，这两个值都来自于最后两行或者两列，所以我们可以只维护最后两行或者两列。更甚者是只维护一行或者一列（因为一个值只会被使用一次，所以可以立马替换为下一个值）。