---
title: 'Python 排序算法实现'
date: 2018-10-23T19:14:18+08:00
lastmod: 2020-10-19T11:50:19+08:00
tags: ['Python']
categories: ['Notes']
authors:
  - '潘峰'
---

## 稳定排序

---

### 冒泡排序

**工作原理**

它重复地走访过要排序的数列，一次比较两个元素，如果他们的顺序错误就把他们交换过来。走访数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成。

- 最差时间复杂度：O(n²)
- 最优时间复杂度：O(n)
- 空间复杂度：O(1)
- 稳定性：稳定

**代码实现**

```python
def bubble_sort(li):
    # 冒泡排序
    count = len(li)
    for i in range(count-1):
        for j in range(count-1-i):
            if li[j] > li[j + 1]:
                li[j], li[j + 1] = li[j + 1], li[j]
    return li
```

**算法优化**

(1) 标志变量优化：

```python
def bubble_sort(li):
    # 冒泡排序（标志变量）
    # 最优时间复杂度：O(1)
    change = 1
    count = len(li)
    for i in range(count - 1):
        if change == 1:
            change = 0
            for j in range(count - 1 - i):
                if li[j] > li[j + 1]:
                    li[j], li[j + 1] = li[j + 1], li[j]
                    change = 1
    return li
```

(2) 双向冒泡排序 / 鸡尾酒排序：

```python
def cocktail_sort(li):
    # 双向冒泡排序/鸡尾酒排序
    left = 0
    right = len(li) - 1
    while left < right:
        # 先正向冒泡
        for i in range(left, right):
            if li[i] > li[i + 1]:
                li[i], li[i + 1] = li[i + 1], li[i]
        right -= 1

        # 再反向冒泡
        for j in range(right, left, -1):
            if li[j] < li[j - 1]:
                li[j], li[j - 1] = li[j - 1], li[j]
        left += 1
    return li
```

---

### 插入排序

**工作原理**

通过构建有序序列，对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。

- 最差时间复杂度：O(n²)
- 最优时间复杂度：O(n)
- 平均时间复杂度：O(n²)
- 最差空间复杂度：О(n) total, O(1) auxiliary
- 稳定性：稳定

**代码实现**

```python
def insertion_sort(li):
    # 插入排序
    count = len(li)
    for i in range(1, count):
        key = li[i]
        j = i - 1
        while j >= 0 and li[j] > key:
            li[j + 1] = li[j]
            j -= 1
        li[j + 1] = key
    return li
```

---

### 归并排序

**核心思想**

归并排序是建立在归并操作上的一种有效的排序算法,该算法是采用分治法（Divide and Conquer）的一个非常典型的应用。将已有序的子序列合并，得到完全有序的序列；即：先使每个子序列有序，再使子序列段间有序。若将两个有序表合并成一个有序表，称为二路归并。

归并过程为：比较 a[i] 和 a[j] 的大小，若 a[i]≤a[j]，则将第一个有序表中的元素 a[i] 复制到 r[k] 中，并令 i 和 k 分别加上 1；否则将第二个有序表中的元素 a[j] 复制到 r[k] 中，并令 j 和 k 分别加上 1，如此循环下去，直到其中一个有序表取完，然后再将另一个有序表中剩余的元素复制到 r 中从下标 k 到下标 t 的单元。

归并排序的算法我们通常用递归实现，先把待排序区间 [s,t] 以中点二分，接着把左边子区间排序，再把右边子区间排序，最后把左区间和右区间用一次归并操作合并成有序的区间 [s,t]。

- 平均时间复杂度：O(nlogn)
- 空间复杂度：O(1)
- 稳定性：稳定

**代码实现**

```python
def merge_sort(li):
    # 归并排序
    if len(li) <= 1:
        return li
    num = int(len(li) / 2)
    left = merge_sort(li[:num])
    right = merge_sort(li[num:])
    return merge(left, right)


def merge(left, right):
    i, j = 0, 0
    result = []
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    result += left[i:]
    result += right[j:]
    return result
```

---

### 基数排序

**核心思想**

基数排序（radix sort）属于“分配式排序”（distribution sort），又称“桶子法”（bucket sort）或 bin sort，顾名思义，它是透过键值的部份资讯，将要排序的元素分配至某些“桶”中，藉以达到排序的作用，基数排序法是属于稳定性的排序，其时间复杂度为 O (nlog(r)m)，其中 r 为所采取的基数，而 m 为堆数，在某些时候，基数排序法的效率高于其它的稳定性排序法。

- 稳定性：稳定

**代码实现**

```python
import math
def radix_sort(li, radix=10):
    k = int(math.ceil(math.log(max(li), radix)))
    bucket = [[] for i in range(radix)]
    for i in range(1, k+1):
        for j in li:
            bucket[j/(radix**(i-1)) % (radix**i)].append(j)
        del li[:]
        for z in bucket:
            li += z
            del z[:]
    return li
```

## 不稳定排序

---

### 快速排序

**工作原理**

通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。

- 核心思想：分而治之、递归
- 最差时间复杂度：O(n²)
- 最优时间复杂度：O(nlogn)
- 平均时间复杂度：O(nlogn)
- 最差空间复杂度：根据实现的方式不同而不同
- 稳定性：不稳定

**代码实现**

```python
def quick_sort(li):
    # 快速排序
    if len(li) <= 1:
        return li
    else:
        pivot = li[0]
        less = [x for x in li[1:] if x < pivot]
        greater = [x for x in li[1:] if x >= pivot]
        return quick_sort(less) + [pivot] + quick_sort(greater)
```

---

### 选择排序

**工作原理**

首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕。

- 最差时间复杂度：O(n²)
- 最优时间复杂度：O(n²)
- 平均时间复杂度：O(n²)
- 最差空间复杂度：О(n) total, O(1) auxiliary
- 稳定性：不稳定

**代码实现**

```python
def selection_sort(li):
    # 选择排序
    count = len(li)
    for i in range(count - 1):
        min_index = i
        for j in range(i + 1, count):
            if li[min_index] > li[j]:
                min_index = j
        li[min_index], li[i] = li[i], li[min_index]
    return li
```

### 希尔排序

**核心思想**

希尔排序(Shell Sort)是插入排序的一种。也称缩小增量排序，是直接插入排序算法的一种更高效的改进版本。希尔排序是非稳定排序算法。该方法因 DL．Shell 于 1959 年提出而得名。 希尔排序是把记录按下标的一定增量分组，对每组使用直接插入排序算法排序；随着增量逐渐减少，每组包含的关键词越来越多，当增量减至 1 时，整个文件恰被分 成一组，算法便终止。

- 稳定性：不稳定

**代码实现**

```python
def shell_sort(li):
    # 希尔排序
    group = len(li) // 2
    while group > 0:
        for i in range(0, group):
            j = i + group
            while j < len(li):
                k = j - group
                key = li[j]
                while k >= 0:
                    if li[k] > key:
                        li[k + group] = li[k]
                        li[k] = key
                    k -= group
                j += group
        group //= 2
    return li
```

---

### 堆排序

**工作原理**

堆排序（Heapsort）是指利用堆积树（堆）这种数据结构所设计的一种排序算法，它是选择排序的一种。可以利用数组的特点快速定位指定索引的元 素。堆分为大根堆和小根堆，是完全二叉树。大根堆的要求是每个节点的值都不大于其父节点的值，即 A[PARENT[i]] >= A[i]。在数组的非降序排序中，需要使用的就是大根堆，因为根据大根堆的要求可知，最大的值一定在堆顶。

**代码实现**

```python
# 堆排序
def heap_sort(li):
    size = len(li)
    build_heap(li, size)
    for i in range(0, size)[::-1]:
        li[0], li[i] = li[i], li[0]
        adjust_heap(li, 0, i)


# 创建堆
def build_heap(li, size):
    for i in range(0, (size/2))[::-1]:
        adjust_heap(li, i, size)


# 调整堆
def adjust_heap(li, i, size):
    lchild = 2 * i + 1
    rchild = 2 * i + 2
    max = i
    if i < size / 2:
        if lchild < size and li[lchild] > li[max]:
            max = lchild
        if rchild < size and li[rchild] > li[max]:
            max = rchild
        if max != i:
            li[max], li[i] = li[i], li[max]
            adjust_heap(li, max, size)
```

---

### 桶排序

**核心思想**

为了节省空间和时间，我们需要指定要排序的数据中最小以及最大的数字的值，来方便桶排序算法的运算。

- 平均时间复杂度：O(n)
- 空间复杂度：О(n)

**代码实现**

```python
def bucket_Sort(li):
    # 选择一个最大的数
    max_num = max(li)
    # 创建一个元素全是0的列表, 当做桶
    bucket = [0]*(max_num+1)
    # 把所有元素放入桶中, 即把对应元素个数加一
    for i in li:
        bucket[i] += 1
    # 存储排序好的元素
    sort_nums = []
    # 取出桶中的元素
    for j in range(len(bucket)):
        if bucket[j] != 0:
            for y in range(bucket[j]):
                sort_nums.append(j)

    return sort_nums
```

> 相关链接：  
> https://www.cxyxiaowu.com/725.html  
> https://github.com/ZQPei/Sorting_Visualization
