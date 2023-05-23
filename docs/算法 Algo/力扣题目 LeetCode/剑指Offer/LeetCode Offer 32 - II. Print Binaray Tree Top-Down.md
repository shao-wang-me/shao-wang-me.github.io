---
publish: true
en_title: LeetCode Offer 32 - II. Print Binaray Tree Top-Down
sr-due: 2023-08-11
sr-interval: 80
sr-ease: 270
tags:
  - algo
---


#algo

做法：[[../../Level-Order Traversal|Level-Order Traversal]]

链接：[剑指 Offer 32 - II. 从上到下打印二叉树 II - 力扣（LeetCode）](https://leetcode.cn/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)

相同：[[../../../../102. 二叉树的层序遍历|102. 二叉树的层序遍历]]

```python
class Solution:  # 迭代，一次扫一层，可用于层序遍历
  def levelOrder(self, root: TreeNode) -> List[List[int]]:
    ans, queue = [], [root] if root else []
    while queue:
      nodes, queue = queue, []
      ans.append([node.val for node in nodes])
      for node in nodes:
        if node.left:
          queue.append(node.left)
        if node.right:
          queue.append(node.right)
    return ans


class Solution:  # 行数少的 Python 代码，可读性不强，权当好玩
  def levelOrder(self, root: TreeNode) -> List[List[int]]:
    ans, queue = [], [root] if root else []
    while queue:
      ans.append([node.val for node in queue])
      queue = [n for node in queue for n in (node.left, node.right) if n]
    return ans


class Solution:  # 递归，一次扫一层，但真的没什么必要的
    def levelOrder(self, root: TreeNode) -> List[int]:
        ans = []

        def process_layer(layer, ans):
            if not layer:
                return
            new_layer = []
            for node in layer:
                if node:
                    ans.append(node.val)
                    new_layer.append(node.left)
                    new_layer.append(node.right)
            process_layer(new_layer, ans)
        
        process_layer([root], ans)
        return ans
```