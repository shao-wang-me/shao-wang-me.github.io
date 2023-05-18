---
en_titel: DFS of binary trees
publish: true
---

这里的 order 是对 node 来说的，如果 node 是最先访问的，就是 pre-order，如果是中间访问的，就是 in-order，如果是最后访问的，就是 post-order。

1. 前序（pre-order）：节点 → 左子树 → 右子树，NLR
2. 中序（in-order）：左子树 → 节点 → 右子树，LNR
3. 后序（post-order）：左子树 → 右子树 → 节点，LRN

## 递归

只要会写前序的递归，另外两种的递归写法差不多。

前序递归（纯函数，dfs()的所有依赖都是以参数形式传进来的）。

```python
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        # 递归
        return dfs(root, [])

def dfs(root, resultList):
    return resultList + [root.val] + dfs(root.left, []) + dfs(root.right, []) if root else resultList
```

前序递归（直接处理 scope 内的变量 ans）。

```python
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        # 递归
        ans = []
        def dfs(root):
            if root:
                ans.append(root.val)
                dfs(root.left)
                dfs(root.right)
        dfs(root)
        return ans

# 更简单点写
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        # 递归
        if not root: return []
        return [root] + self.preorderTraversal(root.left) + self.preorderTraversal(root.right)
```

## 迭代

迭代（iterative），利用栈（stack，FIFO）。

#flashcards **二叉树 DFS 迭代写法结论::前序遍历直接用最正常的 `while stack`，中序后序用颜色标记法。**

### 写法 1：`while stack or node` 内套 `while node`

这个方法前中后遍历都可以，虽然后序变化较大，但还是相对好记。

```python
class Solution:  # 前序遍历迭代
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        ans, stack, node = [], [], root
        while stack or node:  # 终止条件
            while node:  # # 一路向左，这部分处理 node 及 node.left
                ans.append(node.val)
                stack.append(node)
                node = node.left
            node = stack.pop()  # 这部分处理 node.right
            node = node.right
        return ans

class Solution:  # 中序遍历迭代
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        ans, stack, node = [], [], root
        while stack or node:  # 终止条件
            while node:  # 一路向左
                stack.append(node)
                node = node.left
            node = stack.pop()
            ans.append(node.val)  # 与前序的区别就是此处再处理 node
            node = node.right
        return ans

class Solution:  # 后序遍历迭代
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        ans, stack, node = [], [], root
        while stack or node:  # 终止条件
            while node: # 一路向左
                stack.append(node)
                node = node.left
            node = stack.pop()  # 先 pop
            # 右边已经遍历完，或没有右边，就可以加入 ans
            if not node.right or ans and node.right.val == ans[-1]:
                ans.append(node.val)
                node = None
            else:
                stack.append(node)  # 不行再摁回去，即右边还没遍历
                node = node.right
        return ans
```

### 写法 2：`while stack`

前序遍历就用这个！

#flashcards DFS 前序遍历迭代
?
```python
class Solution:  # 前序遍历迭代
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        ans, stack = [], [root]
        while stack:
            node = stack.pop()
            if node:
                ans.append(node.val)
                stack.append(node.right)
                stack.append(node.left)
        return ans
```

### 写法 3：`while node`

```python
class Solution:  # 前序遍历
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        # 迭代
        ans, stack = [], []
        node = root
        while node:
            ans.append(node.val)
            if node.right:
                stack.append(node.right)
            node = node.left if node.left else stack.pop() if stack else None
        return ans
```

### 写法 4：`while stack` 加标记（颜色标记法）

可以实现较为统一的写法。

中序、后序用这个！

- [颜色标记法-一种通用且简明的树遍历方法 - 二叉树的中序遍历 - 力扣（LeetCode）](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/solution/yan-se-biao-ji-fa-yi-chong-tong-yong-qie-jian-ming/)
- [【二叉树前序、中序、后序遍历】python 递归、迭代统一写法 - 二叉树的后序遍历 - 力扣（LeetCode）](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/solution/er-by-forestsking-17uq/)

#flashcards DFS 颜色标记法
?
```python
class Solution:  # 前序遍历
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        # 迭代
        ans, stack = [], [(root, 0)]
        while stack:
            node, append = stack.pop()
            if node:
                if append:
                    ans.append(node.val)
                else:
                    stack.append((node.right, 0))
                    stack.append((node.left, 0))
                    stack.append((node, 1))
        return ans

class Solution:  # 中序遍历
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        # 迭代
        ans, stack = [], [(root, 0)]
        while stack:
            node, append = stack.pop()
            if node:
                if append:
                    ans.append(node.val)
                else:
                    stack.append((node.right, 0))
                    stack.append((node, 1))
                    stack.append((node.left, 0))
        return ans

class Solution:  # 后序遍历
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        # 迭代
        ans, stack = [], [(root, 0)]
        while stack:
            node, append = stack.pop()
            if node:
                if append:
                    ans.append(node.val)
                else:
                    stack.append((node, 1))
                    stack.append((node.right, 0))
                    stack.append((node.left, 0))
        return ans
```
