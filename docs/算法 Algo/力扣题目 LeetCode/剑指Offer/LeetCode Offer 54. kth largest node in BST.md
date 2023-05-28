---
publish: true
en_title: LeetCode Offer 54. kth largest node in BST
sr-due: 2023-06-14
sr-interval: 20
sr-ease: 250
tags:
  - algo
---


#algo

链接:: [剑指 Offer 54. 二叉搜索树的第k大节点 - 力扣（LeetCode）](https://leetcode.cn/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

做法：[[../../数据结构/树/Traversal of Trees|Traversal of Trees]]

BST 的从右到左的中序序遍历就是从大到小排序的数组。

Find the largest kth node in a BST. BST's right-to-left in-order DFS is sorted in decreasing order.

```python
class Solution:  # recursion
  def kthLargest(self, root: TreeNode, k: int) -> int:
    def dfs(node):
      nonlocal k
      if node.right and (right := dfs(node.right)) != None:
        return right
      k -= 1
      if k == 0:
        return node.val
      if node.left and (left := dfs(node.left)) != None:
        return left
    return dfs(root)
```

同样是递归，另一个控制流程写法：

```python
class Solution:
  def kthLargest(self, root: TreeNode, k: int) -> int:
    count, ans = 0, None

    def dfs(root):
      nonlocal count, ans
      if root and ans == None:
        dfs(root.right)
        count += 1
        if count == k:
          ans = root.val
        dfs(root.left)

    dfs(root)
    return ans
```

迭代（颜色标记法）：

```python
class Solution:
  def kthLargest(self, root: TreeNode, k: int) -> int:
    # 第 k 大的节点，比如第 1 大的就是最大的那个
    # 所以是中序遍历，且从右到左
    # 迭代写法（最好记忆的颜色标记法）
    stack = [(root, False)]  # 节点, 是否取出
    count = 0
    while stack:
      node, pick = stack.pop()
      if pick:
        count += 1
        if count == k:
          return node.val
      else:
        if node.left: stack.append((node.left, False))
        stack.append((node, True))
        if node.right: stack.append((node.right, False))
```