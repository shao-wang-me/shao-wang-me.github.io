---
publish: true
en_title: LCOF 62. Last Number Left in the Cicle
sr-due: 2023-05-29
sr-interval: 2
sr-ease: 230
tags:
  - algo
---


#algo

链接：[剑指 Offer 62. 圆圈中最后剩下的数字 - 力扣（LeetCode）](https://leetcode.cn/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)

做法：[[../../Josephus Problem|Josephus Problem]]

```text
0 1 2 3 4
      ^
3 4 0 1
^
1 3 4
  ^
1 3
  ^
3
^
```

如上，这个元素最后在 index 0 位置，如果能找到 $f(n) = g(f(n - 1))$ 的递归公式，就可以从 $f(0)$ 反推 $f(n)$。

```text
0 ... k-1 k k+1 ... n-1           n 个的情形
LLLLLLLLL D RRRRRRRRRRR           k 位置元素删除，k = n % m - 1
                                  右边部分向左移动 k+1 位（左边部分加被删除元素大小）
                                  左边部分向右移动 n-k-1 位（右边元素大小）
k+1 ... n-1   | 0     ... k-1     n-1 个时，上面的元素的位置
RRRRRRRRRRR   | LLLLLLLLLLLLL
0   ... n-k-2 | n-k-1 ... n-2     及其新的 index
```

尝试找到新老 index 的关系。对左半部分，即当 $I(n)<k$ 时，$I(n-1)=I(n)-(k+1)+n$，而当 $I(n)>k$ 时，$I(n-1)=I(n)-(k+1)$。

所以：

$$
I(n-1)=
  \begin{cases}
    I(n)-(k+1)+n & \text{if }I(n)<k \\
    I(n)-(k+1)   & \text{if }I(n)>k
  \end{cases}
$$

反过来即：

$$
I(n)=
  \begin{cases}
    I(n-1)+(k+1)-n & \text{if }0<=I(n)<k \\
    I(n-1)+(k+1)   & \text{if }k<I(n)<=n-1
  \end{cases}
$$

上面的不等式化简，或者观察第一行其实对应原左边部分，第二行对应原右边部分，可得：

$$
I(n)=
  \begin{cases}
    I(n-1)+(k+1)-n & \text{if }I(n-1)>=n-k-1 \\
    I(n-1)+(k+1)   & \text{if }I(n-1)<=n-k-2
  \end{cases}
$$

可进一步化简为：

$$I(n)=(I(n-1)+(k+1))\bmod n$$

```python
class Solution:
  def lastRemaining(self, n: int, m: int) -> int:
    ans = 0
    for i in range(2, n + 1):
      k = m % i - 1
      ans = (ans + k + 1) % i
      # 可以简化为 ans = (ans + m % i) % i = (ans + m) % i
      # 效率更高
    return ans
```