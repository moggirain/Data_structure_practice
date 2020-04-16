# Sorting Practice

1. Classical sorting 
2. Merge Sort   88, 21
3. Different sorting 
4. Heap 

## **Merge Sort** 

**Leetcode 88 Merge Sorted Array** 

{% embed url="https://leetcode-cn.com/problems/merge-sorted-array/" %}

```python
# Method 1: brute force 
# merge nums2 to nums1 in place 

# TC: O((m + n)log (m + n)) SC: O(m + n)
# m = len(nums1) n = len(nums2)
def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None
    nums1[:] = sorted(nums[:m] + nums2)
```

```python
# Method 2: extra space with merge sort 
# TC: O(m + n) SC: O(m)
def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None
    temp = nums1[:m] # copy nums1
    nums1[:] = []  # clear nums1
    i = j = 0 
    while i < m and j < n:
        if temp[i] < nums2[j]:
            nums1.append(temp[i])
            i+=1
        else:
            nums1.append(nums2[j])
            j+=1
    if i < m:
        nums1[i + j:] = temp[i:] # items after i+j 
    if j < n:
        nums1[i + j:] = nums2[j:]
```

```python
# Method 3: in-place 
# TC: O(m+n) SC: O(1)
def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None
    i, j = m-1, n-1
    p = m + n-1
    while i >= 0 and j >= 0:
        if nums1[i] > nums2[j]:
            nums1[p] = nums1[i]
            i-=1
        else:
            nums1[p] = nums2[j]
            j-=1
        p-=1
    # nums2 left elements
    nums1[:j+1] = nums2[:j+1] 
```

![Method 3: end to front pointer i = p1, j = p2, p = p](.gitbook/assets/image.png)

**Leetcode  21 Merge Two Sorted Lists**

{% embed url="https://leetcode-cn.com/problems/merge-two-sorted-lists/" %}

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

# solution 1: iteration 
# TC: O(m + n) SC: O(1)
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        dummy = ListNode(None) # track the link 
        cur = dummy
        while l1 and l2:
            if l1.val < l2.val:
                cur.next = l1
                l1 = l1.next 
            else:
                cur.next = l2
                l2 = l2.next 
            cur = cur.next 
        
        cur.next = l1 or l2
        return dummy.next 
    
# solution 2: recursion 
# TC: O(m + n) SC: O(m + n)
    def mergeTwoLists_rec(self, l1: ListNode, l2: ListNode) -> ListNode:
        if not l1:
            return l2
        if not l2:
            return l1
        else:
            if l1.val < l2.val:
                l1.next = self.mergeTwoLists_rec(l1.next, l2)
                return l1
            else:
                l2.next = self.mergeTwoLists_rec(l1, l2.next)
                return l2

    
    def print_list(self, head):
    
        while head:
            print(str(head.val) + "->", end = "")
            head = head.next
            
if __name__== "__main__":
    l1 = ListNode(1)
    l1.next = ListNode(2)
    l1.next.next = ListNode(4)
    l2 = ListNode(1)
    l2.next = ListNode(3)
    l2.next.next = ListNode(4)
   
    s.print_list(s.mergeTwoLists(l1, l2))
    #s.print_list(s.mergeTwoLists_rec(l1, l2))
```

#### 

#### 

#### 

#### 

## **Heap** 

**Problem: Find smallest k items in an unsorted array**

Find smallest k elements from an unsorted array of size n. The output should be in ascending order.

```python
# Solution 1: Minheap
# heapify all elements  O(n)
# call pop() k times to get k smallest items (klogn)
# TC: O(n + klogn) SC: O(k)
import heapq
def kSmallest(array, k):
		if not array:
      return[]
    heapq.heapify(array)
    res = []
    for i in range(min(len(array), k)):
      	res.append(heapq.heappop(array))
    return res
```

```python
# Solution 2: Maxheap
# 1,5,3,4,6,2 k = 3    1 2 3    
#     -5      [4, 6, 2]    pop: -5, -4, -6     -3
#  -1   -3                                  -1    -2
# heapify the first k items in a maxheap with size = k  O(k)
# iterate the rest n-k items and update the top of the heap O((n-k)logk)
# item > top, ignore, else: add into the heap and update 
# TC: O(k + (n-k)logk) SC: O(k)
  
def Ksmallest(array, k):
  	if not array:
      return []     
    res = [-i for i in array[:k]] # maxheap 
    heapq.heapify(res) 
    for i in range(k, len(array)):
      	heapq.heappushpop(res, -array[i]) # pop k largest number
   	return sorted([-i for i in res]) # klogk  
```

