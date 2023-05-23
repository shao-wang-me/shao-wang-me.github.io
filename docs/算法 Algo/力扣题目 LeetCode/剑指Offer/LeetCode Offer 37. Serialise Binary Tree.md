---
publish: true
en_title: LeetCode Offer 37. Serialise Binary Tree
sr-due: 2023-06-17
sr-interval: 28
sr-ease: 230
tags:
  - algo
---


#algo 官方解法的几种看看。

链接：[剑指 Offer 37. 序列化二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/xu-lie-hua-er-cha-shu-lcof/)

相同：[[../../../../297. 二叉树的序列化与反序列化|297. 二叉树的序列化与反序列化]]

做法： [[../../BFS|BFS]]、[[../../DFS of Binary Trees|DFS of Binary Trees]]；[[../../../../BNF|BNF]]

[[../../BFS|BFS]] 做法：

```python
from collections import deque

def serialise(root: 'TreeNode'):
  ans = []
  queue = deque([root])
  while queue:
    node = queue.popleft()
    ans.append(node.val if node else None)
    if node:
      queue.append(node.left)
      queue.append(node.right)
  return str(ans)
  # '[1, 2, 3, None, None, 4, 5, None, None, None, None]'
  # '[None]' for empty tree

def deserialise(data: str):
  nodes = data[1:-1].split(', ')
  nodes = [TreeNode(int(x)) if x != 'None' else None for x in nodes]
  # 用一个 flag 记录是左边还是右边
  # 如果不是 None 就放在 queue 中，作为接下来的 parent
  # 每次插入一个 node
  root = nodes[0]
  queue = deque([root])
  left = True
  for node in nodes[1:]:
    if not queue:
      # 这样跳过不会跳过应该插入的 node 么？
      # 毕竟我们用的是 for 循环
      # 答案是不会，因为 queue 为空时，必然是末尾的 None
      break
    if left:
      queue[0].left = node
    else:
      queue[0].right = node
      queue.popleft()
    left = not left
    if node:
      # 而且我们不会把 None 放入 queue
      # queue 都是要做 parent 的，None 没有意义
      queue.append(node)
  return root
```

[[../../数据结构/树/Traversal of Trees#DFS|二叉树的遍历 > DFS]] 其实是一样的，只是重构的时候按照 DFS 的方法来，用 stack 而不是 queue。DFS 没有 BFS 好些倒是。

还有一种是用 ((Tree)Number(Tree)) 的 BNF 语法，核心是把树用括号括起来，然后解析。

```python
class Codec:
  def serialize(self, root):
    # 用括号框起来
    if not root:
      return 'X'
    return f'({self.serialize(root.left)}){root.val}({self.serialize(root.right)})'

  def deserialize(self, data):
    n = len(data)
    i = 0

    def parse():
      nonlocal i
      if data[i] == 'X':
        i += 1
        return None
      left = parse_tree()
      val = parse_int()
      right = parse_tree()
      return TreeNode(val, left, right)
    
    def parse_int():
      nonlocal i
      s = ''
      sign = 1
      if data[i] == '-':
        sign = -1
        i += 1
      while i < n and data[i].isdigit():
        s += data[i]
        i += 1
      return int(s) * sign
    
    def parse_tree():
      nonlocal i
      i += 1  # skip (
      tree = parse()
      i += 1  # skip )
      return tree

    return parse()
```