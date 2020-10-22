---
description: Binary Search and Sorting Problem
---

# Day1\_Challenge

## [Leetcode 169 Majority Element ](https://leetcode-cn.com/problems/majority-element/)

### Solution 1: Hash Table 

* Traverse the array once and count the numbers
* Find the num with count &gt; n//2 
* TC: O\(n\) SC: O\(n\)

```python
# Solution 1 
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        n = len(nums) 
        dic = {}
        for i in nums:
            dic[i] = dic.get(i, 0) + 1 
        
        for k, v in dic.items():
            if v > n/2:
                return k 
```

### Solution 2: Sort 

* If the space complexity is a requirement, then we can optimize space requirement by using count
* Sort the values and if there is a mode, then when mid-index of the array will be the mode number. 
  * E.g, \[1,2,2,2,3\]  nums\[mid\] = 2 
* TC: O\(nlogn\) SC: O\(logn\)

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        n = len(nums) 
        nums.sort()
        return nums[n//2] 
```

### Solution3: Boyer-Moore Majority Vote Algorithm 

{% embed url="https://leetcode-cn.com/problems/majority-element/solution/tu-jie-mo-er-tou-piao-fa-python-go-by-jalan/" %}

* We simplify the problem to a binary case. When see the target candidate once,  count+=1, when not seeing the target candidate count-=1. 
* When the count=0, it means we offset the candidate and other's votes. So we reset the candidate to restart the count. 
* Whichever the candidate left, it will be the candidate who gained most votes. 
* TC: O\(n\) SC: O\(1\)

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        n = len(nums) 
        candidate = 0  # set the major candidate 
        count = 0      # set the count 
        for i in nums:
            if count == 0:
                candidate = i 
            if i == candidate:
                count+= 1 
            else:
                count-= 1 
        return candidate 
```

### \*\*\*\*[**229. Majority Element II**](https://leetcode-cn.com/problems/majority-element-ii/)\*\*\*\*

### \*\*\*\*



