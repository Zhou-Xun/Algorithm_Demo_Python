#### 时间复杂度

- 确定问题规模
- 循环减半 → $logn$
- k层关于n的循环 → $n^k$

时间复杂度记为$ O(log_2^n) $ ， 当算法过程出现循环折半，就会出现这个复杂度


```python
n = 64
while n > 1:
    n = n//2
```

#### 空间复杂度： 用来评估算法内存占用大小的式子，算法追求空间换时间

- 算法使用了几个变量：O(1)
- 算法使用了长度为n的一维列表：O(n)
- 算法使用了m行n列的二维列表：O(mn)

#### 递归（recursion）
- 递归的两个特点
    - 调用自身
    - 结束条件


```python
def func(x):
    if x > 0:
        func(x-1)
```


```python
# 汉诺塔问题
def hanoi(n, a, b, c):
    if n > 0:
        hanoi(n-1, a, c, b)
        print("moving from %s to %s" % (a,c))
        hanoi(n-1, b, a, c)
hanoi(2, 'A', 'B', 'C')
```

    moving from A to B
    moving from A to C
    moving from B to C
    

#### 排序

- 冒泡排序
- 选择排序
- 插入排序

1. 冒泡排序：两两比较交换位置，每一次都能排出一个最大值


```python
def bubble_sort(li):
    for i in range(len(li)-1):
        exchange = False
        for j in range(len(li)-i-1):
            if li[j] > li[j+1]:
                li[j], li[j+1] = li[j+1], li[j]
                exchange = True
        if not exchange:
            return 
```

2. 选择排序：每次选出最小的放入结果数组


```python
def select_sort(li):
    temp = []
    for i in range(len(li)-1):
        min_loc = i
        for j in range(i+1, len(li)):
            if li[j] < li[i]:
                min_loc = j
        li[i], li[min_loc] = li[mic_loc], li[i]
```

3. 插入排序：每次从无序区中抽一个数放到有序区中正确的位置


```python
def insert_sort(li):
    for i in range(1, len(li)):
        tmp = li[i]
        j = i - 1
        while j >= 0 and li[j] > tmp:
            li[j+1] = li[j]
            j -= 1
        li[j+1] = tmp
```

4. 快速排序


```python
def partition(arr, low, high):
    i = low - 1
    pivot = arr[high]
    for j in range(low, high):
        if arr[j] < pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i+1], arr[high] = arr[high], arr[i+1]
    return i + 1
```


```python
def quick_sort(arr, low, high):
    if low < high:
        p = partition(arr, low, high)
        quick_sort(arr, low, p-1)
        quick_sort(arr, p+1, high)
```


```python
li = [5,7,4,6,3,1,2,9,8]
quick_sort(li, 0, len(li)-1)
print(li)
```

    [1, 2, 3, 4, 5, 6, 7, 8, 9]
    

5. 堆排序

基础知识
- 树的度， 一个节点分出来的节点数， 二叉树就是度不超过2的树
- 满二叉树：二叉树每层的节点达到最大值
- 完全二叉树：叶子节点只出现在最下层和次下层，最下层的叶子节点集中在该层最左边的若干位置
- 二叉树的顺序存储方式：列表存储，若父节点为i，左右子节点分别为2i+1和2i+2
- 子节点为i，父节点下标为(i-1)//2

堆是一种特殊的完全二叉树<br>
大根堆：任一节点都比其他孩子节点大<br>
小根堆：任一节点都比其他孩子节点小

堆排序算法
1. 建立堆，得到堆顶最大元素
2. 去掉堆顶，将堆最后一个元素放到堆顶
3. 对堆顶元素向下调整，重新得到一个大根堆
4. 重复1-3步骤
5. 空间节省，放下的最大元素与堆最后一个元素进行位置交换


```python
# 向下调整
def sift(arr, low, high):
    """
    low: 堆的根节点位置
    high: 堆的最后一个元素位置
    """
    i = low
    j = 2*i+1 # j开始是i的左子节点
    tmp = arr[low]
    while j <= high: # 只要j有位置
        if j + 1 <= high and arr[j+1] > arr[j]: # 如果有右子节点且右子节点比左子节点大
            j = j + 1
        if arr[j] > tmp: # 需要向下调整
            arr[i] = arr[j]
            i = j
            j = i*2+1
        else:
            break
    arr[i] = tmp
```


```python
# 开始堆排序
def heap_sort(arr):
    n = len(arr)
    # 从最后一个父节点开始自下往上建堆
    for i in range((n-2)//2, -1, -1):
        sift(arr, i, n-1)
    # 建堆完成
    # 接下来不断去出堆顶元素放到堆尾，并不断重建堆
    for i in range(n-1, -1, -1):
        # i始终指向最后一个元素
        # 交换首尾
        arr[0], arr[i] = arr[i], arr[0]
        # 重建堆
        sift(arr, 0, i-1)
```


```python
arr = [i for i in range(100)]
import random
random.shuffle(arr)
print(arr)
heap_sort(arr)
print("After heap sort:")
print(arr)
```

    [30, 1, 22, 52, 4, 53, 34, 33, 6, 92, 76, 51, 63, 26, 2, 89, 41, 91, 57, 46, 86, 55, 43, 87, 67, 44, 96, 50, 94, 93, 17, 59, 83, 18, 0, 66, 12, 31, 23, 48, 21, 8, 20, 40, 27, 25, 77, 14, 58, 99, 19, 39, 69, 32, 62, 81, 49, 97, 85, 60, 24, 54, 95, 84, 70, 90, 9, 47, 64, 73, 35, 71, 36, 10, 11, 37, 75, 78, 65, 13, 79, 61, 72, 68, 29, 74, 88, 80, 82, 98, 5, 45, 28, 3, 42, 38, 56, 15, 7, 16]
    After heap sort:
    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75, 76, 77, 78, 79, 80, 81, 82, 83, 84, 85, 86, 87, 88, 89, 90, 91, 92, 93, 94, 95, 96, 97, 98, 99]
    

堆排序的时间复杂同快排一样同样是$ O(nlog_2^n) $， 但在实际表现中，快排更快

堆的内置模块，heapq


```python
import heapq
nums = [3,2,5,1,54,23,132]
heap = []
for num in nums:
    heapq.heappush(heap, num)
print([heapq.heappop(heap) for _ in range(len(nums))])

nums = [3,2,5,1,54,23,132]
heapq.heapify(nums)
print([heapq.heappop(nums) for _ in range(len(nums))])

dic = {0: 0.07, 1: 0.16, 2: 0.01}
max_n = heapq.nlargest(2, dic.items(), key = lambda x: x[1])
print(max_n)
```

    [1, 2, 3, 5, 23, 54, 132]
    [1, 2, 3, 5, 23, 54, 132]
    [(1, 0.16), (0, 0.07)]
    

6. 归并排序


```python
def merge(arr, l, m, r):
    # 默认l到m和m+1到r都已排好，现在要从l到r排好
    n1 = m-l+1
    n2 = r-m
    # 创建临时数组
    L = [0]*n1
    R = [0]*n2
    for i in range(n1):
        L[i] = arr[l+i]
    for i in range(n2):
        R[i] = arr[m+1+i]
    # 开始归并
    i, j, k = 0, 0, l
    while i<n1 and j<n2:
        if L[i] <= R[j]:
            arr[k] = L[i]
            i += 1
        else:
            arr[k] = R[j]
            j += 1
        k += 1
    while i < n1:
        arr[k] = L[i]
        i += 1
        k += 1
    while j < n2:
        arr[k] = R[j]
        j += 1
        k += 1
```


```python
def merge_sort(arr, l, r):
    if l < r:
        m = (l+r)//2
        merge_sort(arr, l ,m)
        merge_sort(arr, m+1, r)
        merge(arr, l, m, r)
```


```python
arr = [i for i in range(100)]
import random
random.shuffle(arr)
print(arr)
merge_sort(arr, 0, len(arr)-1)
print("After heap sort:")
print(arr)
```

    [79, 69, 19, 44, 95, 52, 98, 94, 42, 97, 84, 78, 31, 76, 26, 43, 68, 16, 28, 22, 5, 49, 55, 80, 0, 90, 82, 91, 18, 61, 86, 8, 27, 12, 73, 70, 24, 92, 48, 36, 11, 87, 17, 1, 51, 46, 57, 60, 71, 85, 63, 38, 15, 56, 23, 13, 89, 33, 77, 50, 81, 83, 40, 53, 25, 58, 64, 37, 67, 21, 54, 14, 62, 93, 34, 39, 65, 74, 66, 88, 75, 20, 59, 45, 35, 7, 6, 2, 10, 29, 99, 32, 41, 30, 96, 4, 47, 72, 9, 3]
    After heap sort:
    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75, 76, 77, 78, 79, 80, 81, 82, 83, 84, 85, 86, 87, 88, 89, 90, 91, 92, 93, 94, 95, 96, 97, 98, 99]
    


```python

```
