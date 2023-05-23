---
publish: true
en_title: LeetCode Offer 55 - I. Depth of Binary Tree
sr-due: 2023-06-16
sr-interval: 26
sr-ease: 290
tags:
  - algo
---


#algo

链接:: [剑指 Offer 55 - I. 二叉树的深度 - 力扣（LeetCode）](https://leetcode.cn/problems/er-cha-shu-de-shen-du-lcof/)

相同:: [[../../../../104. 二叉树的最大深度|104. 二叉树的最大深度]]

做法:: [[递归]]；[[../../Level-Order Traversal|Level-Order Traversal]]

> 节点总数 <= 10000
> number of nodes <= 10000

```python
class Solution:  # recursion
  def maxDepth(self, root: TreeNode) -> int:
	def dfs(node):
	  if not node:
		return 0
	  left_depth = dfs(node.left)
	  right_depth = dfs(node.right)
	  return max(left_depth, right_depth) + 1
	return dfs(root)
```

```python
class Solution:  # short form recursion
  def maxDepth(self, root: TreeNode) -> int:
    return max(self.maxDepth(root.left), self.maxDepth(root.right)) + 1 if root else 0
```

```python
from collections import deque

class Solution:  # level-order traversal
  def maxDepth(self, root: TreeNode) -> int:
    if not root: return 0
    ans = 0
    queue = deque([root])
    while queue:
      ans += 1
      new_queue = deque()
      for node in queue:
        if node.left: new_queue.append(node.left)
        if node.right: new_queue.append(node.right)
      queue = new_queue
    return ans
```