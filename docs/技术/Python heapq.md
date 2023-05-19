---
publish: true
---

Python heapq is a library that represent a [[堆|heap]]'s [[完全二叉树|complete binary tree]] in a list.

```python
>>> from random import randint
>>> xs = [randint(0,99) for _ in range(10)]
>>> xs
[0, 54, 44, 3, 59, 32, 90, 16, 52, 22]
>>> import heapq
>>> heapq.heapify(xs)
>>> xs
[0, 3, 32, 16, 22, 44, 90, 54, 52, 59]
# heapq gives you a min heap
# you can get a max heap by storing -x for each element
# xs is still just a list, but elements reordered as a heap
# where xs[i] <= xs[2*i+1] and xs[i] <= xs[2*i+2]
# we can represent a heap in this list because
# the heap is a complete binary tree
>>> heapq.heappush(xs, -1)
>>> xs
[-1, 0, 32, 16, 3, 44, 90, 54, 52, 59, 22]
>>> heapq.heappush(xs, 36)
>>> xs
[-1, 0, 32, 16, 3, 36, 90, 54, 52, 59, 22, 44]
>>> heapq.heappop(xs)  # pop the smallest element
-1
>>> xs
[0, 3, 32, 16, 22, 36, 90, 54, 52, 59, 44]
>>> xs[0]  # xs[0] is the smallest element
0
```