# Sorting Practice

1. Classical sorting 
2. Merge Sort 
3. Different sorting 

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



