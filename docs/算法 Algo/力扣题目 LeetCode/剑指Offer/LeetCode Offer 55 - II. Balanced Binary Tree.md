---
publish: true
en_title: LeetCode Offer 55 - II. Balanced Binary Tree
sr-due: 2023-06-14
sr-interval: 25
sr-ease: 290
tags:
  - algo
---


#algo

链接:: [剑指 Offer 55 - II. 平衡二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/ping-heng-er-cha-shu-lcof/)

相同:: [[../../../../110. 平衡二叉树|110. 平衡二叉树]]

做法:: [[递归]]

判断一个二叉树是不是平衡的。平衡二叉树的任意节点的左右子树的深度差不会超过 1，我们就按照这个定义做即可。

Check if a binary tree is balanced. A balanced binary tree's any node's left and right trees' depths difference is smaller than 1, we can just follow this definition.

```python
class Solution:
  def isBalanced(self, root: TreeNode) -> bool:
    def dfs(node):
      """Return depth or -1 if not balanced."""
      if not node:
        return 0
      left_depth = dfs(node.left)
      if left_depth == -1:
        return -1
      right_depth = dfs(node.right)
      if right_depth == -1 or abs(left_depth - right_depth) > 1:
        return -1
      return max(left_depth, right_depth) + 1

    depth = dfs(root)
    return depth != -1
```