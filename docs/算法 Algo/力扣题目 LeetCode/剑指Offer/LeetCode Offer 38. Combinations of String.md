---
publish: true
en_title: LeetCode Offer 38. Combinations of String
sr-due: 2023-06-25
sr-interval: 34
sr-ease: 230
tags:
  - algo
---


#algo

链接：[剑指 Offer 38. 字符串的排列 - 力扣（LeetCode）](https://leetcode.cn/problems/zi-fu-chuan-de-pai-lie-lcof/)

做法：[[排列]]、[[../../Lexicographic Order|Lexicographic Order]]

剑指书上的思路是对于一组元素，前面是固定的，后面是要排列的，用递归将逐个位置排列出来。

```python
def permutation(xs):
  if not xs:
    return []
  return permutation_core(xs, 0)

def permutation_core(xs,  i):
  if i == len(xs) - 1:
    return [xs]
  ans = []
  for j in range(i, len(xs)):
    ys = xs[:]
    ys[i], ys[j] = ys[j], ys[i]
    ans += permutation_core(ys, i + 1)
  return ans
```

另一种是每次把改元素剔出，代码更简洁，但是不知道为什么更慢。我本来以为是字符串比 list 慢，但是改成 list 之后更慢。

```python
class Solution:
  def permutation(self, s: str) -> List[str]:
    def dfs(s):
      if not s:
        return ['']
      ans = []
      for i, x in enumerate(s):
        # 把第 i 个（x）踢出，最后再放到头
          for combination in dfs(s[:i] + s[i + 1:]):
            ans.append(x + combination)
      return ans
    return list(set((dfs(s))))
```

剑指官方题解提供了一种保证没有重复元素的思路，即先将数组排序。比如如有两个$a$，$a_1a_2$，则保证一定先选择 $a_1$，防止 $a_2a_1$ 的情况出现。这个也是回溯法。

```python
class Solution:
  def permutation(self, s: str) -> List[str]:
    s = sorted(s)
    visited = [False] * len(s)
    locked = ''
    ans = []

    def backtrack(i):
      nonlocal locked
      if i == len(s):
        ans.append(locked)
        return
      for j in range(len(s)):
        # not visited and also the first in the group of the same character
        if not visited[j] and (j == 0 or s[j] != s[j - 1] or visited[j - 1]):
          locked += s[j]
          visited[j] = True
          backtrack(i + 1)  # here is i + 1, not j + 1
          locked = locked[:-1]
          visited[j] = False

    backtrack(0)
    return ans
```

力扣官方还有一个按照字典序的方法，每次生成下一个[[../../Lexicographic Order|Lexicographic Order]]，即从后往前找比后面挨着的元素小的元素，与从后往前第一个比它大的元素交换（比如 cbiga 中 b 是第一个比后面挨着的小的字符，我们从后往前找第一个比它大的，即 g 与之交换，得到 cgiba，这确实是下一个字典序。我把它与剑指 Offer 的方法结合了一下：

```python
class Solution:
  def permutation(self, s: str) -> List[str]:
    # 把首字母与后面所有的字母进行对调
    # 一个优化是结合字典序的方法，先排序，每次调用的时候保证要比上一个更大的
    # 从而避免重复
    def lock(s):
      if not s:
        return ['']
      ans = []
      h = s[0]  # last choosen head char
      for i, c in enumerate(s):
        # put each char in unlocked string s to the head
        # and generate combinations
        # but the char must be "larger" than the last choosen char
        # or we will have duplicates
        # for example, we can get ['aa', 'aa'] if c == h
        # and ['abc', 'abc'] if c < h and c > h are both allowed
        # allow i == 0 because we don't have last choosen char
        # we use c > h here because we have sorted(s)
        # if we have sorted(s, reverse=True) then we can have c < h
        if i == 0 or c > h:
          ans += [c + t for t in lock(s[:i] + s[i+1:])]
          h = c  # update h here because now the last choosen head char is c
      return ans
    
    return lock(sorted(s))
```