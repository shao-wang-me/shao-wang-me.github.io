---
publish: true
en_title: LeetCode Offer 59 - I. Maximum Number in Sliding Window
sr-due: 2023-05-25
sr-interval: 4
sr-ease: 220
tags:
  - algo
---


#algo 下次分块处理

链接：[剑指 Offer 59 - I. 滑动窗口的最大值 - 力扣（LeetCode）](https://leetcode.cn/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)

相同：[[../../../../239. 滑动窗口最大值|239. 滑动窗口最大值]]

做法：[[../../../../堆|堆]]；[[../../Monotonous Queue|Monotonous Queue]]

> ```text
> 输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
> 输出: [3,3,5,5,6,7] 
> 解释: 
> 
>   滑动窗口的位置                最大值
> ---------------               -----
> [1  3  -1] -3  5  3  6  7       3
>  1 [3  -1  -3] 5  3  6  7       3
>  1  3 [-1  -3  5] 3  6  7       5
>  1  3  -1 [-3  5  3] 6  7       5
>  1  3  -1  -3 [5  3  6] 7       6
>  1  3  -1  -3  5 [3  6  7]      7
> ```

最好的时间复杂度肯定是 $O(n)$。如果 $k$ 比较小，可以直接用 $O(k)$ 的空间追踪滑动窗口，但是 $1 ≤ k ≤ nums.length$。

如果使用 $O(k)$ 空间，则该数据结构应该支持：

```text
xs.add(x)      O(logn)   排序数组
xs.max()       O(1)
xs.remove(x)   O(logn)   二分查找
```

可以使用排序数组，但是总时间是 $O(nlogn)$，超时了：

```python
import bisect

class Solution:
  def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
    window = nums[:k]
    window.sort()
    ans = [window[-1]]
    for i, n in enumerate(nums[k:]):
      # 移除左边元素
      i += k
      to_remove = nums[i - k]
      j = bisect.bisect_left(window, to_remove)
      window.pop(j)
      # 添加右边元素
      j = bisect.bisect_right(window, n)
      window.insert(j, n)
      ans.append(window[-1])
    return ans
```

如果使用堆，我们可以在堆中同时存储元素的值和元素的 index，这样在移除左侧元素 x, i 时，就可以把 >= x 且 <= i 的元素移除，剩下的 < i 但是 < x 留下来无所谓。

```python
import heapq

class Solution:
  def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
    q = [(-x, i) for i, x in enumerate(nums[:k])]
    heapq.heapify(q)
    ans = [-q[0][0]]
    for i, n in enumerate(nums[k:]):
      while q and q[0][1] <= i:
        heapq.heappop(q)
      heapq.heappush(q, (-n, i + k))
      ans.append(-q[0][0])
    return ans
```

添加元素总时间为 $O(nlogn)$，因为堆每次添加为 $O(logn)$，删除元素总时间为 $O(n)$，因为每个元素只会被删除一次，且单次删除为 $O(1)$，返回最大值总时间为 $O(n)$。相比于上面排序数组的做法，删除元素的时间从 $O(nlogn)$ 变成了 $O(n)$。

另外一个方法是使用所谓[[../../Monotonous Queue|Monotonous Queue]]，其实和排序数组的想法很像，但是注意到**小于新 push 数字的元素已经没有价值了，接下来不会成为最大值，可以 pop 掉**。

```text
1,3,-1,-3,5,3,6,7    queue
--------------------------
1                    1
1,3                  3          < 3 的数字去除
1,3,-1               -1,3       < -1 的数字去除，-1 置于队首
  3,-1,-3            -3,-1,3    < -3 的数字去除，-3 置于队首，1 不在队尾
    -1,-3,5          5          < 5 的数字去除，5 置于队首，3 不在队尾
       -3,5,3        3,5        < 3 的数字去除，3 置于队首，-1 不在队尾
          5,3,6      6          < 6 的数字去除，-3 不在队尾
            3,6,7    7          < 7 的数字去除，5 不在队尾
```

总结下来就是**把小于新数的数字去除，把新数放在队首（最小值）。所以 queue 中左边的数字肯定比右边的数字新，那么要移出窗口的数字最老，一定在队尾，若队尾等于该数，移出即可**。时间复杂度是 $O(n)$，因为每个数字最多只会被遍历、添加、删除一次，空间是 $O(k)$。

```python
from collections import deque

class Solution:  # 单调队列 monotonous queue (min queue / max queue)
  def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
    xs, ans = deque(), []
    for i, n in enumerate(nums):
      while xs and xs[0] <= n:
        xs.popleft()
      xs.appendleft(n)
      if i >= k:
        if len(xs) > 1 and xs[-1] == nums[i-k]:
          xs.pop()
        ans.append(xs[-1])
    return ans
```