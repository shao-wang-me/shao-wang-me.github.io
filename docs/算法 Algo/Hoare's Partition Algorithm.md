---
publish: true
en_title: Hoare's Partition Algorithm
sr-due: 2023-06-01
sr-interval: 4
sr-ease: 250
tags:
  - draft
  - flashcards
---


#draft 为什么 Hoare 比 Lomuto 的效率高，似乎来自算法导论，去看看

发明快速排序的英国计算机科学家 Hoare 的办法。用左右两个指针，根据比对和 pivot 的大小互相交换位置，最后 pivot 和右指针的位置交换。

其实也很好理解，但是不是很好写，有一些细节需要注意。

有用 `until`（`do while`）的写法，也可以用 `while` 直接写。

用 while 也分为用 `while true` 模拟 `until` 和直接 `while`。最好也是在理解的前提下==**背下来**==。

这个算法效率比 [[Partition 中的 Lomuto 算法|Partition 中的 Lomuto 算法]] 的高。

---

#flashcards 

Hoare 的 partition 算法
?
```python
def partition_hoare(nums, l, r):
  pivot = random_pivot(nums, l, r)
  i, j = l + 1, r
  while i <= j:  # 此处与二分一样是 i <= j
    if nums[i] < pivot:
      i += 1
      continue
    if nums[j] >= pivot:
      j -= 1
      continue
    nums[i], nums[j] = nums[j], nums[i]
  nums[l], nums[j] = nums[j], pivot
  return j
```
<!--SR:!2023-05-16,3,230-->

Hoare 的 partition 算法关键点
?
1. `while i <= j`，和二分是一样的
2. `while` 内一定要用 `if` 加 `continue` 防止 `i` 和 `j` 错位
3. `nums[i]` 和 `pivot` 之间用 `<` 或 `<=` 都行
4. 但是 `i` 和 `j` 至少有一个带等于，不过 `nums[j] <= pivot` 最后才能直接 swap 并返回 j
5. 每一个 while 中，直接 swap i 和 j，最后一个 swap 时其实 i == j
6. 最终 swap l 和 j
7. 返回 j
<!--SR:!2023-05-16,3,230-->

Hoare 的 repeat until 版本
?
注意要在快排数组最后加一个 sentinel。
```python
def partition_hoare_repeat_until(nums, l, r):  # handle all equal cases better
  pivot = random_pivot(nums, l, r)             # because i++ and j-- when nums[i] == nums[j] == pivot
  i, j = l, r + 1
  while True:
    while True:
      i += 1
      if nums[i] >= pivot:
        break
    while True:
      j -= 1
      if nums[j] <= pivot:
        break
    nums[i], nums[j] = nums[j], nums[i]
    if i >= j:
      break
  nums[i], nums[j] = nums[j], nums[i]
  nums[l], nums[j] = nums[j], nums[l]
  return j

def quicksort_hoare_repeat_until(nums):
  nums.append(math.inf)  # append a sentinel to prevent index i from advancing beyond position n
  iterations = quicksort_core(nums, 0, len(nums) - 2, partition_hoare_repeat_until)  # thus r = n - 2 here
  return nums[:-1], iterations  # and remove the last math.inf
```

---

参考：[[../书 Books/Introduction to the Design & Analysis of Algorithms (Third Edition)|Introduction to the Design & Analysis of Algorithms (Third Edition)]] p. 204