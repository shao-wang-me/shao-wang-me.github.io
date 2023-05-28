---
publish: true
en_title: LeetCode Offer 59 - II. Max-Queue
sr-due: 2023-06-09
sr-interval: 13
sr-ease: 270
tags:
  - algo
---


#algo

链接:: [剑指 Offer 59 - II. 队列的最大值 - 力扣（LeetCode）](https://leetcode.cn/problems/dui-lie-de-zui-da-zhi-lcof/)

实现一个队列，支持 `max_value`、`push_back` 和 `pop_front`，且均摊时间复杂度都是 $O(1)$。

Implement a queue with `max_value`, `push_back` and `pop_front`, and the amortised complexity are all $O(1)$.

入队和出队都是 $O(1)$ 没问题，但是最大值如何实现 $O(1)$ 呢？可以用一个辅助队列，且一个元素只会被操作常数次。如：

Enqueue (push) and dequeue (pop) are both easily $O(1)$, but how to achieve $O(1)$? We can use an auxiliary queue, and make sure each element will only be operated constant times. For example:

```text
      q        max_q  max
-------------------------
5     5        5      5     + 5
2     5,2      5,2    5     2 < 5, + 2
pop   2        2      2     5 = 5, - 5
1     2,1      2,1    2     1 < 5, + 1
7     2,1,7    7      7     while max_q[-1] < n: max_q.pop(); max_q.append(n)
                            从小到大把元素 pop 出
pop   1,7      7      7     max_q 左边元素老，pop 的一定最老，若还在 max_q 中就在最左边
                            if max_q[0] == q.popleft(): max_q.popleft()
3     1,7,3    7,3    7
9     1,7,3,9  9      9
```

```python
from collections import deque

class MaxQueue:
  def __init__(self):
    self.q = deque()
    self.max_q = deque()

  def max_value(self) -> int:
    return self.max_q[0] if self.max_q else -1

  def push_back(self, value: int) -> None:
    self.q.append(value)
    while self.max_q and self.max_q[-1] < value:  # push 的时候一定是从小到大 pop 出后面的元素！
      self.max_q.pop()                            # must use < not <= otherwise pop_front() is broken
    self.max_q.append(value)

  def pop_front(self) -> int:
    if not self.q:
      return -1
    value = self.q.popleft()
    if self.max_q[0] == value:
      self.max_q.popleft()
    return value
```