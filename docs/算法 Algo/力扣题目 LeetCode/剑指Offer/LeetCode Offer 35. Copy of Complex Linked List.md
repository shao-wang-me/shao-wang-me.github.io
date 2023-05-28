---
publish: true
en_title: LeetCode Offer 35. Copy of Complex Linked List
sr-due: 2023-08-24
sr-interval: 91
sr-ease: 290
tags:
  - algo
---


#algo

链接：[剑指 Offer 35. 复杂链表的复制 - 力扣（LeetCode）](https://leetcode.cn/problems/fu-za-lian-biao-de-fu-zhi-lcof/)

做法：[[链表|链表]]

相同：[[138. 复制带随机指针的链表|138. 复制带随机指针的链表]]

```python
class Node:
  def __init__(self, x, next=None, random=None):
    self.val = int(x)
    self.next = next
    self.random = random
```

```python
def copy_complex_linked_list(head):
  # create N' for N
  node = head
  while node:
    copy = Node(node.val, next=node.next, random=node.random)
    node.next = copy
    node = copy.next
  # move random links to node copies
  node = head
  while node:
    copy = node.next
    if copy.random:
      copy.random = copy.random.next
    node = copy.next
  # separate N' from N
  node = head
  while node:
    copy = node.next
    node_next = copy.next
    if copy.next:
      copy.next = copy.next.next
    node = node_next
  return head.next if head else None
```