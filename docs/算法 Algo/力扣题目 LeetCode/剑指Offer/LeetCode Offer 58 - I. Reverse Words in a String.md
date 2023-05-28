---
publish: true
en_title: LeetCode Offer 58 - I. Reverse Words in a String
sr-due: 2023-05-28
sr-interval: 4
sr-ease: 270
tags:
  - algo
---


#algo

链接：[剑指 Offer 58 - I. 翻转单词顺序 - 力扣（LeetCode）](https://leetcode.cn/problems/fan-zhuan-dan-ci-shun-xu-lcof/)

相同：[[151. 反转字符串中的单词|151. 反转字符串中的单词]]

把英文句子中的单词（空格间隔就算单词，无视标点）翻转（reverse）过来。不过可能有首尾及中间多余的空格。

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        words = s.split(' ')
        words = [x for x in words if x]
        return ' '.join(reversed(words))
```

由于 `s.split()` 正好可以忽略空格，所以改进为：

```python
class Solution:
  def reverseWords(self, s: str) -> str:
    s = s.split()
    s = reversed(s)
    s = ' '.join(s)
    return s
```

或：

```python
class Solution:
  def reverseWords(self, s: str) -> str:
    ans = []
    i = 0
    while i < len(s):
      while i < len(s) and s[i] == ' ':
        i += 1
      word = ''
      while i < len(s) and s[i] != ' ':
        word += s[i]
        i += 1
      if word:
        ans.append(word)
    return ' '.join(reversed(ans))
```