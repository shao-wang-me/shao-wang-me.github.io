---
publish: true
en_title: "LeetCode Offer 56 - I. Find the number appeared only once in an array other numbers appeared twice"
sr-due: 2023-05-21
sr-interval: 4
sr-ease: 250
---

#algo 试试别的 `div` 获取方法

链接:: [剑指 Offer 56 - I. 数组中数字出现的次数 - 力扣（LeetCode）](https://leetcode.cn/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)

做法:: [[位运算]]

> 一个整型数组 `nums` 里除两个数字之外，其他数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度是O(n)，空间复杂度是O(1)。
> 
> `2 <= nums.length <= 10000`
> 
> 输入：nums = [4,1,4,6]
> 输出：[1,6] 或 [6,1]
> 
> 输入：nums = [1,2,10,4,1,4,3,3]
> 输出：[2,10] 或 [10,2]

是输出**只出现一次**的**两个**数字，其它数字都出现**两次**！

最简单的办法是计数，但是需要 O(n) 的空间，不符合要求。排序也不行。看了提示，居然是[[位运算]]。应该是 n XOR n = 0，这个可以跨越不同数字么？

例如 [4,1,4,6]，`4 ^ 1 ^ 4 ^ 6 = 7`，虽然 4 都消掉了，但是从 7 能得到什么呢？这个值是 1 XOR 6：

```text
    001
XOR 110
  = 111
```

根据异或的性质，如果这个值二进制某位是 1，表示答案的这两个数字该位数字不同。利用这个信息，我们只需要选取某一为 1 的位，把所有数字分为该为为 1 和 0 的两组数字，两个部分再分别做异或即可得到两个答案数字了。

```python
import functools

class Solution:
  def singleNumbers(self, nums: List[int]) -> List[int]:
    xor = functools.reduce(lambda x, y: x ^ y, nums)
    # 只留一个 1
    div = xor
    while next_div := div & (div - 1):  # 一直把最后一个 1 变成 0
      div = next_div  # 最高位 1 保留
    x, y = 0, 0  # 初始值为 0 因为 0 ^ n = n
    for n in nums:
      if n & div:  # 则该位为 1
        x ^= n  # 分组异或
      else:  # 该位为 0
        y ^= n
    return [x, y]
```

其中，`div` 的获得方法有很多种，力扣题解用的是：

```python
div = 1
while div & ret == 0:  # find the last 1 (rightmost 1)
	div <<= 1
```

我之前用的方式，先把最后一位 1 变成 0，在和原数字异或，不需要 while，是最妙的：

```python
# find the different digit (binary)
# i.e. any digit that is 1 in a xor b
digit = xor ^ (xor & (xor - 1))  # this version is finding the last one
```

还可以使用 `bin(n)`：

```python
# 取第一位 1，其他位变成 0
xor_bin = bin(xor)[2:]
n = len(xor_bin)
div = '1' + '0' * (n - 1)
div = int(div, 2)
```