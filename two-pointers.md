# Two Pointers

### Problem: Find the minimum absolute difference in two sorted array

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

