# Day2\_Challenge

\*\*\*\*[**LEETCODE 26. Remove Duplicates from Sorted Array**](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)\*\*\*\*

* In place
* Use one variable i to track all unduplicated numbers 
* Use another variable j to traverse the nums 
* If nums\[j\] is not a duplicate, then move it to nums\[i\], and move i to next 
* the final position i will be the length of the non-duplicates 
* i needs to start with 1, because we assume 0th number is not a duplicate 

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if not nums:
            return 0 
        i = 1 
        for j in range(1, len(nums)):
            if nums[j] != nums[j-1]:
                nums[i] = nums[j]
                i+= 1
        return i 
```

