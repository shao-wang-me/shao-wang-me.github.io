---
publish: true
en_title: LeetCode Offer 52. First Shared Node in Two Linked Lists
sr-due: 2023-06-01
sr-interval: 17
sr-ease: 290
tags:
  - algo
---


#algo

链接：[剑指 Offer 52. 两个链表的第一个公共节点 - 力扣（LeetCode）](https://leetcode.cn/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)

做法：[[链表|链表]]；[[双指针]]

竟然是简单。最笨的方法当然是弄一个哈希表，然后先扫一个链表，再扫一个链表。不过想到它们的后半部分是重合的，只要知道两个的长度 m 和 n，然后再把长的那个先走掉，再一起走，如果是同一个就行了。不过注意到还有可能没有重合，那么就会出现两个链表都走完了都没有重合的情况，返回 None 即可。

```python
class Solution:
  def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
    if not headA or not headB:
      return None
    a = headA
    m = 0
    while a:
      m += 1
      a = a.next
    b = headB
    n = 0
    while b:
      n += 1
      b = b.next
    if m >= n:
      extra_len = m - n
      long, short = headA, headB
    else:
      extra_len = n - m
      long, short = headB, headA
    for _ in range(extra_len):
      long = long.next
    while long:
      if long == short:
        return long
      long = long.next
      short = short.next
    return None
```

看了题解，还有一种更简洁的做法，即[[双指针]]同步走，A 走完走 B，B 走完走 A，这样长度差别自动抹平！代码简单到无法想象！

```python
class Solution:
  def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
    # This part is omittable and is still correct
    # They will eventually be both None
    # if not headA or not headB:
    #   return None
    a, b = headA, headB
    while a != b:
      a = a.next if a else headB
      b = b.next if b else headA
    return a
```