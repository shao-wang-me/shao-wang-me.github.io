---
publish: true
en_title: LeetCode Offer 50. First Character that Appears Only Once
sr-due: 2023-08-19
sr-interval: 84
sr-ease: 310
tags:
  - algo
---


#algo 做了哈希表的，下次做队列的

链接：[剑指 Offer 50. 第一个只出现一次的字符 - 力扣（LeetCode）](https://leetcode.cn/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)

做法：[[哈希表|哈希表]]；[[队列]]

第一个只出现一次的字符。第一反应是用哈希表存储，但是具体怎么弄，还是举例看看：

> 输入：s = "abaccdeff"
> 输出：'b'
> 
> 输入：s = "" 
> 输出：' '

对 abaccdeff 来说，a 出现了两次，b 是第一个出现了一次的字符。我们能够扫一遍，看哪些字符只出现一次，然后再扫一遍？这样的时间是 $O(n)$，空间是 $O(26)$，因为只包含小写字母。最好也就是把空间再变小，然后只扫一遍。

```python
class Solution:
  def firstUniqChar(self, s: str) -> str:
    chars = {}
    for c in s:
      chars[c] = chars[c] + 1 if c in chars else 1
    for c in s:
      if chars[c] == 1:
        return c
    return ' '
```

看了力扣题解，哈希表存储字符第一次见到的位置，如果第二次见到则改值为 -1，再加一个额外的队列，存储第一次见到字符的顺序，并且 dequeue 出不止一次的字符，则最后队列的 head 就是我们要的字符。

```python
from collections import deque

# abaccdeff
# {a:0} | [a]
# {a:0,b:1} | [a,b]
# {a:-1,b:1} | [b]  pop out a because it appears again
# {a:-1,b:1,c:3} | [b,c]
# {a:-1,b:1,c:-1} | [b,c]   because b is valid at front, don't bother popping out c
class Solution:
  def firstUniqChar(self, s: str) -> str:
    seen = {}
    q = deque()
    for i, c in enumerate(s):
      if c not in seen:  # if first seen, record in seen and q
        seen[c] = i
        q.append(c)
      else:  # if seen, seen[c] = -1, q.pop() until q[0] is valid (appear only once)
        seen[c] = -1
        while q and seen[q[0]] == -1:
          q.popleft()
    return q[0] if q else ' '
```

但这真的是每必要的麻烦，时间和空间都没有节约太多，且实施上处理小数据时的额外开销更多。