---
publish: true
en_title: Paths in Tree
---

树的路径本质上还是用DFS遍历。

```python
class Solution:  # 递归
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        ans = []

        def dfs(root, path):
            path.append(root.val)  # 先append
            if root.left:
                dfs(root.left, path)
            if root.right:
                dfs(root.right, path)
            if not root.left and not root.right:  # 到leaf，就有一条路径了
                ans.append(path[:])
            path.pop()  # 这一步是关键，最后要pop

        dfs(root, [])
        # return ans
        return ['->'.join(str(x) for x in path) for path in ans]


class Solution:  # 迭代
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        # 迭代的话得用后序，整个path还在stack里
        # 和标准迭代没有区别，只是原来append的地方现在ignore了
        ans, stack = [], [(root, 0)]  # 0: normal, 1: ignore, 2: right
        while stack:
            node, t = stack.pop()
            if node:
                if t == 1:
                    pass
                else:
                    stack.append((node, 1))
                    stack.append((node.right, 2))
                    stack.append((node.left, 0))
                    if not node.left and not node.right:
                        ans.append([x.val for x, t in stack if x and t != 2])
        # return ans
        return ['->'.join(str(x) for x in path) for path in ans]
```
