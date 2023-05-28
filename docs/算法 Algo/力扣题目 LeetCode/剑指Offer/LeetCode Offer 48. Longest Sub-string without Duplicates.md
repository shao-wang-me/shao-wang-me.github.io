---
publish: true
en_title: LeetCode Offer 48. Longest Sub-string without Duplicates
sr-due: 2023-06-12
sr-interval: 26
sr-ease: 250
tags:
  - algo
---


#algo

链接:: [剑指 Offer 48. 最长不含重复字符的子字符串 - 力扣（LeetCode）](https://leetcode.cn/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)

相同:: [[3. 无重复字符的最长子串|3. 无重复字符的最长子串]]

> 输入: "abcabcbb"
> 输出: 3 
> 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

以上面例子来看，abc 都没问题，直到又出现了 a，如果我们保存每一个字符最后出现的位置，就可以知道上一次出现的位置，就可以知道当前的 streak 是不是要结束了，并更新 `ans = max(ans, current_streak_length`。新的 streak 则是从该字符上一次出现的位置的下一个位置开始。

```python
class Solution:
  def lengthOfLongestSubstring(self, s: str) -> int:
    ans = 0
    seen = {}
    current_length = 0
    for i, c in enumerate(s):
      if c in seen:
        if seen[c] >= i - current_length:  # i = 1, current_length = 1, seen[c] = 0
          ans = max(ans, current_length)
          current_length = i - seen[c] - 1
      current_length += 1
      seen[c] = i
    ans = max(ans, current_length)
    return ans
```

另一种记录不重复子字符串的开始：

```python
class Solution:
  def lengthOfLongestSubstring(self, s: str) -> int:
    # hash table
    seen = {}
    ans = 0
    streak_start = 0
    for i, c in enumerate(s):
      if c in seen:
        if seen[c] >= streak_start:
          # this is a dup, update ans
          ans = max(ans, i - streak_start)
          # the streak will be starting from the next from last seen
          streak_start = seen[c] + 1
      seen[c] = i
    ans = max(ans, len(s) - streak_start)
    return ans
```

最简洁的版本：

```python
class Solution:
  def lengthOfLongestSubstring(self, s: str) -> int:
    start, ans, seen = 0, 0, {}
    for i, c in enumerate(s):
      if c in seen and start <= seen[c]:  # don't forget this start <= seen[c]
        start = seen[c] + 1
      ans = max(ans, i - start + 1)
      seen[c] = i
    return ans
```