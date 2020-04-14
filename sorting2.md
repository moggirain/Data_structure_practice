# Sorting2

* **Definition:** Rearrange a sequence of objects into logic order. 
* **Logic order:** the key for arrangement \(usually numerical / alphabetical order\)
* **Three concerns:** Algorithm performance \(see below\), memory\(in-place or extra space\), data type. 
* 
![A tour of the top 5 sorting algorithms with Python code](https://miro.medium.com/max/596/1*ipkeWQ_Lb0lbkhB8rigxTA.png)

* **Important algorithms:**

  | Algorithms | Requirement | Time Complexity | Space | Stability |
  | :--- | :--- | :--- | :--- | :--- |
  | Bubble Sort | Familiar | O\(n^2\) | O\(1\) | Yes |
  | Insertion Sort | Familiar | O\(n^2\) | O\(1\) | Yes |
  | Selection Sort | Master | O\(n^2\) | O\(1\) | No |
  | Merge Sort | Master | O\(nlogn\) | O\(n\)/O\(1\) | Yes |
  | Quick Sort | Master | O\(nlogn\) | O\(logn\) | No |
  | Heap Sort | Master | O\(nlogn\) | O\(1\) | No |

## Bubble Sort

* **Description:** Bubble sort repeatedly traverse through the list, compares adjacent elements and swaps if they are in the wrong order. The pass through the list is repeated until the list is sorted. 
* **Analogy:** Each pass, a min/max number is like a bubble, float to the end of the list, as sorted. 

!\[6.7. The Bubble Sort — Problem Solving with Algorithms and Data ...\]\([https://runestone.academy/runestone/books/published/pythonds/\_images/bubblepass.png](https://runestone.academy/runestone/books/published/pythonds/_images/bubblepass.png)

### Algorithm Implementation

* Two loops \(internal and external\)
* One controls pass; One controls the sorted swap

  ![img](https://miro.medium.com/proxy/1*LllBj5cbV91URiuzAB-xzw.gif)

```python
# Version A front--end
def bubble_sort(array):
      n = len(array)
      for i in range(n): # 0..n-1
            for j in range(0, n-1-i): # 0, n-2-i
              if array[j] > array[j+1]:
                        array[j], array[j+1] = array[j+1], array[j]

# Version B e
def bubble_sort(array):
  # assume the last one is sorted
    n = len(array)
        for i in range(n-1, 0, -1): # n-1..1
                for j in range(i):# 0..n-2
                  if array[j] > array[j+1]:
                        array[j], array[j+1] = array[j+1], array[j]



if __name__=="__main__":
        array = [54,26,93,17,77,31,44,55,20]
        bubble_sort(array)
```

#### Optimization

If there is no swap in one iteration, then the previous sorting are done. Best: O\(n\) Worst: O\(n^2\)

```python
def bubble_sort(alist):

    for j in range(len(alist)-1,0,-1):
        count = 0 
        for i in range(j): # traverse to the second last 
            if alist[i] > alist[i+1]: # if second is larger than the first, then swap 
                alist[i],alist[i+1] = alist[i+1],alist[i]
                count +=1

        if count ==0:
            return
```

## Insertion Sort

* **Description:** Insertion assumps the sullist of items is already sorted in the lower positions of the list. Each new items in inserted back into the previous sublist, and shift the item that are greater to the right, until find the correct position to insert. 
* **Analogy:** Consider playing cards, insert each card into its proper place among those already considered \(keeping them sorted\). 
* The running time depended on the initial order of the items. 
* Insertion sort is an excellent method for partially sorted arrays and is also a fine method for tiny arrays.
  * partially sorted array: If the number of inversions in an array is less than a constant multiple of the array size, we say that the array is _partially sorted_. 

### Algorithm Implementation

```text
insertionSort (arr, n)
Loop from i = 1 to n-1.
……a) Pick element arr[i] and insert it into sorted sequence arr[0…i-1]
```

![img](https://miro.medium.com/proxy/1*onU9OmVftR5WeoLWh14iZw.gif)

```python
# version 1
def insertion_sort(array):
    n = len(array)
    for i in range(1, n): # first one sorted
        j = i 
        while j > 0:
          if array[j] < array[j-1]:
            array[j], array[j-1] = array[j-1], array[j]
            j-=1
          else: # optimization
            break 
# version 2
def insertion_sort(array):
        n = len(array)
    for i in range(1, n):# 1..n-1
            for j in range(i, 0, -1): # n-1..1
                if array[j] < array[j-1]:
                    array[j], array[j-1] = array[j-1], array[j]
            else:
                  break 

if __name__ == "__main__":
    array = [5,3,2,8,3,9]
    insertion_sort(array)
    print(array)
```

​

## Seleciton Sort

* **Description:** Assume the sublist is sorted, and the remaining sublist is unsorted. Repeatedly traverse the array and find the the minimum element from unsorted array and put it at the beginning \(verse versa, find the maximum and put it at the end\), until the list is fully sorted. 
* Selection sort uses **$ N \* \(N-1\) / 2 = N^{2}/2 $**    compares and _N_ exchanges to sort an array of length _N_.

### Algorithm Implementation

* 选择排序 极值两边
* 起始极值 loop调换
* 全部遍历 极值更新

![../\_images/selectionsortnew.png](https://runestone.academy/runestone/books/published/pythonds/_images/selectionsortnew.png)

![img](https://miro.medium.com/proxy/1*OA7a3OGWmGMRJQmwkGIwAw.gif)

```python
def selection_sort(array):
      n = len(array)
    for i in range(n): 
          _min = i # inialize the min as i 
        for j in range(i+1, n): # the first one is sorted
              if array[_min] > array[j]:  # find the min in the list 
                    _min = j 
       # after one pass, swap the initial min with the current min 
       array[_min], array[i] = array[i], array[_min] 

if __name__ == "__main__":
    array = [2,3,1,9,5,4]
    selection_sort(l)
    print(l)
```

## Merge Sort

**Description:** Divide and conquer strategy, to sort an array, dide the into two halves recursively, and conquer: to merge the results and reach a combined sorted new list.

### Algorithm Implementation

* Top-down MergeSort: Divide Conquer
* Optimization: improve the algorithm performance by handling small subarrays with insertion sort.
* Pay attention to the call of recursion \(See Figure [https://algs4.cs.princeton.edu/22mergesort/](https://algs4.cs.princeton.edu/22mergesort/)\)

![img](https://miro.medium.com/proxy/1*fE7yGW2WPaltJWo6OnZ8LQ.gif)

```python
def MergeSort(array):

      if len(array) <=1: # base case 
          return array
    mid = len(array)//2 # divide 
    a = MergeSort(array[:mid])
    b = MergeSort(array[mid:])

      def merge(a,b): # merge
        i = j = 0 
        res = []
        while i < len(a) and j < len(b):
            if a[i] < b[j]:
                res.append(a[i])
                i+=1
            else:
                res.append(b[j])
                j+=1
         # one list has value left
        while i < len(a):
            res.append(a[i])
            i+=1
        while j < len(b):
            res.append(b[j])
            j+=1
        return res 

    return merge(a,b)

if __name__ == "__main__":
    nums = [6,9,4,8,9,3,5,2]
    sort_list = MergeSort(nums)
    print(sort_list)
```

* **Bottom-up merge sort** : handles the situation where most arrays are small subarrays. When merge the array, it starts with the signle lement array, and the 2-by-2 adjacent pairs, and then 4-by-4 arrays.

  **Compare two merge processes: Top-down**

  ![Merging Two Lists](https://s3-us-west-2.amazonaws.com/tutorials-image/merging+of+two+lists.png)

  _\*Bottom-up in iteration 1, 2, 3:_

![Bottom-up Iteration 1](https://s3-us-west-2.amazonaws.com/tutorials-image/Bottom-Up+Merge+Sort.png)

![Bottom-up Iteration 2](https://s3-us-west-2.amazonaws.com/tutorials-image/bottom+up+Iteration+2.png)

![Bottom-up implementation](https://s3-us-west-2.amazonaws.com/tutorials-image/bottom+up+implementation.png)

```python
def mergeSort(self, array): 
    if not array: return [] # corner ase 
    n = len(array)
    array = [[x] for x in array]
    while n > 1:
      array = self.merge_sort(array)
      return array[0]

def merge_sort(self, array):
    res = []
    for i in range(0, n//2):
            res.append(self.merge(array[i*2], array[i*2 + 1]))
    if n%2:
          res.append(array[-1])
    return res 

def merge(self, a,b): # merge
        i = j = 0 
        res = []
        while i < len(a) and j < len(b):
            if a[i] < b[j]:
                res.append(a[i])
                i+=1
            else:
                res.append(b[j])
                j+=1
         # one list has value left
        # res.extend(a[i:])
        # res.extend(b[i:])
        while i < len(a):  
            res.append(a[i]) 
            i+=1
        while j < len(b):
            res.append(b[j])
            j+=1
        return res
```

Source: [https://gist.github.com/etrepum/6689110](https://gist.github.com/etrepum/6689110)

## Quick Sort

* **Description:** The **quick sort** uses divide and conquer to gain the same advantages as the merge sort, while not using additional storage. Pick the pivot value in the list, as the split point. Partition the array by putting the elements smaller than pivot to its left and the elements greater than pivot to its right, until its fully sorted.
* Different methods to select pivot:

  * first element 
  * last element 
  * random element 
  * median 

  ​

### Algorithm Implementation

![img](https://miro.medium.com/proxy/1*hk2TL8m8Kn1TVvewAbAclQ.gif)

```python
# version 1: pick the fist item
def quickSort(array):
    n = len(array)
    if n<=1: 
        return array
    pivot = array[0]
    left_array = quickSort([x for x in array[1:] if x < pivot])
    right_array = quickSort([x for x in array[1:] if x >= pivot])
    return left_array + [pivot] + right_array

def quick_sorted(array, reverse = False):
    array = self.quickSort(array)
    if reverse:
        array = array[::-1]
    return array

if __name__== "__main__":
    alist = [54,26,93,17,77,31,44,55,20]
    print(quickSort(alist))
```

```python
# version 2: pick the random 
import random 
class Solution(object):
  def quickSort(self, array):
    self.quick_sort(array, 0, len(array)-1)
    return array 

  def partition(self, array, left, right):
    start = left
    end = right -1
    rand = random.randint(left, right) # generate a random number
    array[rand], array[right] = array[right], array[rand]
    pivot = array[right]
    while start <= end:
      if array[start] < pivot:
        start += 1
      elif array[end] >= pivot:
        end -= 1
      else: # array[start] >= pivot and array[end]< pivot
        array[start],array[end] = array[end],array[start]
        start +=1
        end -=1
    array[start],array[right] = array[right], array[start]
    return start

  def quick_sort(self, array, left, right):
    if left >= right:
      return 
    pivot = self.partition(array, left, right)
    self.quick_sort(array, left, pivot -1)
    self.quick_sort(array, pivot+1, right)
```

## Heap Sort

#### Priority Queue

* **Priority Queue**

Priority Queue: The highest priority items are at the front of the queue and the lowest priority items are at the back. Thus when you enqueue an item on a priority queue, the new item may move all the way to the front.

* **Queue:** FIFO 
* **Priority:** dynamic change the order based on priority 按优先顺序动态变动
* Static and Dynamic both works 动态和静态皆可
* Enqueue 
* Dequeue\( According to priority level \)

|  | 入队 | 出队 |
| :--- | :--- | :--- |
| 普通数组 | O\(1\) \(放末尾\) | O\(n\) 扫描查找 |
| 顺序数组 | O\(n\)（查找合适插入顺序） | O\(1\) （优先级最高元素队头） |
| 堆 | O\(lgn\) | O\(lgn\) |

* Worse case scenario for 
  * Array and SortedArray: Avg O\(n^2\) 
  * Heap: O\(nlgn\)
* Binary Heap: complete binary tree 

### Binary Heap

**Property**

1. **Max \(min\) Heap**: Any child node is less than \(greater than\) its parent node
2. **Complete binary tree**: Except the last level, all nodes are full. The node in the last level needs to order from left to right.
3. Max heap vs min heap
4. Implementation: Array

**Structure**

![image](https://runestone.academy/runestone/books/published/pythonds/_images/heapOrder.png)

* Parent Index: p \(c hild index // 2\) index starts with 0
* Left child Index = 2 \* P
* Right child index = 2 \* p +1

知道孩子找父亲 index = \(x-1\)/2

Min-heap: 每个node都比自己的孩子节点小 （Root = Min）

Max-heap:每个node都比自己的孩子节点大 （Root = Max）

孩子之间大小没有关系

Implementation: Tree node or Array

Why？ Array存储简单--不存储parent 和 child 的link

### Algorithm Implementation

![heapsort](https://media.giphy.com/media/14iTOxmpkjAa08/giphy.gif)

```python
from array import Array

class MaxHeap: 
  def __init__(self, arr = None, capacity = None):
    if isinstance(arr, Array): # heapify 
        self._data = arr 
      # 构建heap，从最后一个节点的父节点往前面一个个进行shift down
      for i in range(self._parent(arr.get_size()-1), -1, -1):
              self._shift_down(i) # 
               return 
        if not capacity:
        self._data = Array() #default array
      else:
        self._data = Array(capacity = capacity) # 指定长度的array

  def size(self):
        return self._data.get_size()

  def is_empty(self):
        return self._data.is_empty()

  def _parent(self, index): # search parent index of given node
        if index == 0: 
              raise ValueError("index-0 doesn't have parent.")
          return (index - 1) //2 

  def _left_child(self, index): #search child index of given node
        return index * 2 + 1 # left child 2 * i + 1

  def _right_child(self, index): 
        return index * 2 + 2 # right child 2 * i +2

  def add(self, item):
        self._data.add_last(item) # 先加数字
        self._shift_up(self._data.get_size() - 1) #从最后shiftup

  def _shift_up(self, i): # i为传入数字index，swap until larger than parent 
          while i >0 and self._data[i] > self._data[self._parent(i)]:
          self._data.swap(i, self._parent(i))
          i = self._parent(i)

    def _sift_up(self, k): # k为刚传入的数字的index, swap unitl larger than parent 
        while k > 0 and self._data.get(k) > self._data.get(self._parent(k)):
                self._data.swap(k, self._parent(k))
                k = self._parent(k) # update index

    def find_max(self):
          if self._data.get_size() == 0:
            raise ValueError("Can not find_max when heap is empty.")
              return self._data[0]

    def extract_max(self): # 删除最大值
          res = self.find_max()
          self._data.swap(0, self._data.get_size()-1)
          self._data.remove_last()
          self._shift_down(0)
          return res 

   def _sift_down(self, k): # 原地排序而不是删除
        # 当此节点的左孩子还有节点的时候，就一直比较执导break
        while self._left_child(k) < self._data.get_size():
            j = self._left_child(k) # self.get_size() = 最大的index-1
            # 有右节点且右节点大于左节点，获得两个节点中，比较大的那个数字的index，和现在的parent进行比较
            if j + 1 < self._data.get_size() and self._data.get(j + 1) > self._data.get(j):
                # 说明右孩子的值比左孩子的值大
                j = self._right_child(k)
            # 此时self._data.get(j)是左孩子和右孩子中的最大值
            if self._data.get(k) > self._data.get(j):
                break
            self._data.swap(k, j)
            k = j

    def extract_max(self):   
        res = self.find_max()
        self._data.swap(0, self._data.get_size() - 1)
        self._data.remove_last()
        self._sift_down(0)
        return res

        def swap(self, i, j):
        if i < 0 or i >= self._size or j < 0 or j >= self._size:
            raise ValueError('Index is illegal.')
        self._data[i], self._data[j] = self._data[j], self._data[i]

    def replace(self, e):
        ret = self.find_max()
        # 这样可以一次logn完成
        self._data.set(0, e)
        self._sift_down(0)
        return ret


if __name__ == '__main__':
    n = 10000000
    from time import time

    start_time1 = time()
    max_heap = MaxHeap()
    from random import randint
    for i in range(n):
        max_heap.add(randint(0, 1000))
    print('heap add: ', time() - start_time1) # head add:  5.748132228851318

    start_time2 = time()
    arr = Array()
    from random import randint
    for i in range(n):
        arr.add_last(randint(0, 1000))
    max_heap = MaxHeap(arr)
    print('heapify: ', time() - start_time2) # heapify:  4.680660963058472
```

