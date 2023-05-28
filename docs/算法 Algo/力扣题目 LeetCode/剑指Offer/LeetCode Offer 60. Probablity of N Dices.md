---
publish: true
en_title: LeetCode Offer 60. Probablity of N Dices
sr-due: 2023-06-04
sr-interval: 8
sr-ease: 250
tags:
  - algo
---


#algo

链接:: [剑指 Offer 60. n个骰子的点数 - 力扣（LeetCode）](https://leetcode.cn/problems/nge-tou-zi-de-dian-shu-lcof/)

> 输入: 1
> 输出: [0.16667,0.16667,0.16667,0.16667,0.16667,0.16667]
> 
> 输入: 2
> 输出: [0.02778,0.05556,0.08333,0.11111,0.13889,0.16667,0.13889,0.11111,0.08333,0.05556,0.02778]

$f(n,x)=\sum_{i=1}^{6}f(n-1,x-i)p_i$，其中 $p_i=1/6$，$x \in [n,6n]$，共 $5n+1$个，上一个有 $5(n-1)+1=5n-4$，也就是多了 $5$ 个。

但是具体计算的时候可以不用这样 $\sum$，而是将每一个 $f(n-1, x)$ 加到 $f(n,x+1)$、$f(n,x+2)$、$f(n,x+3)$、$f(n,x+4)$、$f(n,x+5)$、$f(n,x+6)$ 上。

```python
class Solution:
  def dicesProbability(self, n: int) -> List[float]:
    ans = [1 / 6] * 6
    for i in range(2, n + 1):
      new_ans = [0] * (5 * i + 1)
      for j, p in enumerate(ans):
        for k in range(6):         # 从 j + 1 开始到 j + 6，但正好 new_ans[0] 对应 j + 1
          new_ans[j + k] += p / 6  # k 也是从 0 开始，所以是 j + k
      ans = new_ans
    return ans
```