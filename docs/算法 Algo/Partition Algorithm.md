---
publish: true
en_title: Partition Algorithm
sr-due: 2023-05-30
sr-interval: 3
sr-ease: 253
tags:
  - draft
  - flashcards
---


#draft three-way

算法的核心是：

1. 选择一个元素为 pivot，并进行 partition：
   1. 把剩下的元素中小于该元素的放在左边，大于该元素的放在右边
   2. 且该 pivot 最终处于正确的位置上
2. 选择哪个元素为 pivot。最基础的是选择第一个，但对已排序的数组时间会变成 $O(n^2)$。一般采用：
   1. 随机选择
   2. 中位数
   3. 其它更高级选择方式

Partition 有两种方法，比较好写的 Lomuto 和发明快速排序的 Hoare 的算法。

1. [[Partition 中的 Lomuto 算法|Partition 中的 Lomuto 算法]]
2. [[./Hoare's Partition Algorithm|Hoare's Partition Algorithm]]

代码详见：[algo/sorting.py at main · shao-wang-me/algo (github.com)](https://github.com/shao-wang-me/algo/blob/main/sorting.py)

---

#flashcards

Partition 的复杂度:: $O(n)$，因为要遍历所有元素，无论是快慢指针还是首尾指针
<!--SR:!2023-06-13,31,270-->

Partition 的应用
?
1. [[快速排序|快速排序]]，$O(n)$
2. 快速选择，即寻找第 k 大的元素（故包含寻找中位数），$O(n)$
<!--SR:!2023-07-09,57,270-->