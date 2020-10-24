# Two Pointers

### Problem: Find count of solution pairs in duplicated two sum

input:  \[10, 2, 2, 13, 2\], target = 4 

Solution1: output = 3  

Solution2: output =  \(1,2\), \(1,4\), \(2,4\) 

```python
# Solution1: use extra var count to record the frequency 
# TC: O(n) SC: O(n)
def two_sum_duplicated(arr, target):
    dic = {}
    count = 0 
    for key in dic:
        if target - key in dic:
            count += dic[target - key]
        if key in dic:
            dic[key] += 1
        else:
            dic[key] = 1
    return count 
```

```python
# Solution2: if outputs are index pairs
# dic = {10: {0}, 2: {1,2,4}}
def two_sum_duplicated(arr, target):
    dic = {}
    for key, val in enumerate(arr):
        if target - key in dic:
            for idx in dic[target - key]:
                print(key, idx)
        if val in dic:
            dic[val].add(key)
        else:
            dic[val] = {key}
```

### **LeetCode 653. Two Sum IV - Input is a BST**

{% page-ref page="two-pointers.md" %}

```python
# solution 1: hashset 
# res = k - p  given p is one node.val in the tree 
# traverse the tree to check if res in hashset and save result 
# TC: O(n) # of nodes SC: O(n) # of nodes in worse case
class Solution:
def findTarget(self, root: TreeNode, k: int) -> bool:
    self.visited = set()
        return self.searchResult(root, k)
    
def searchResult(self, root, k):
    if not root:
        return False 
    if k - root.val in self.visited:
        return True 
    self.visited.add(root.val)
    return self.searchResult(root.left, k) or self.searchResult(root.right, k)
```

```python
# solution 2: BST property
# BST: in-order traversal is an increasing order 
# use binary search to find the solution 
# TC: O(n) SC: O(n)
class Solution:
    def findTarget(self, root: TreeNode, k: int) -> bool:
        
        def inOrder(root):
            if not root: # base case 
                return []
            return inOrder(root.left) + [root.val] + inOrder(root.right)
        
        arr = inOrder(root)
        l, r = 0, len(arr)-1
        while l < r:
            res = arr[l] + arr[r]
            if res == k:
                return True 
            elif res < k:
                l+=1
            else:
                r-=1
        return False
```

### **Problem: Find the minimum absolute difference in two sorted array**

```python
def minDiff(arr1, arr2):
    m, n = len(arr1), len(arr2)
    diff = float("inf")
    
    i, j = 0, 0
    while i < m and j < n:
        if abs(arr1[i] - arr2[j]) < diff:
            diff = abs(arr1[i] - arr2[j])
        
        if arr1[i] > arr2[j]:
            j+=1
        else:
            i+=1

    return diff

if __name__== "__main__":
    a = [1,2,5,8,15]
    b = [3,4,6,9,10]
    print(minDiff(a,b))
```

### Problem 2: Continuous Maximum Subarray

Given an array having N positive integers, find the contiguous subarray having sum as great as possible, but not greater than M.

```python
def max_subarray(nums, M):
    
   
    n = len(nums)
    cum = []
    cum_sum = [sum(nums[0:i]) for i in range(n+1)]
   
    l, r =0, 1 # two pointers start at tip of the array.
    maximum = 0
    while l < len(cum_sum):
        while r < len(cum_sum) and cum_sum[r] - cum_sum[l] <= M:
            r += 1
        if cum_sum[r - 1] - cum_sum[l] > maximum: # since cum_sum[0] = 0, thus r always > 0.
            maximum = cum_sum[r - 1] - cum_sum[l]
            pos = (l, r - 2)
        l += 1
    return pos

if __name__=="__main__":
    a = [4, 7, 12, 1, 2, 3, 6]
    m = 15
    print(max_subarray(a, m))
```

