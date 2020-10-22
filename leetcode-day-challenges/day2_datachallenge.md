# Day2\_Challenge

### \*\*\*\*[**LEETCODE 26. Remove Duplicates from Sorted Array**](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)\*\*\*\*

* **In place**
* Use one variable i to track all unduplicated numbers 
* Use another variable j to traverse the nums 
* If nums\[j\] is not a duplicate, then move it to nums\[i\], and move i to next to replace with the current number 
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

### \*\*\*\*[**LEETCODE 80. Remove Duplicates from Sorted Array II**](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/)\*\*\*\*

* first thought about comparing nums\[j\] with nums\[j-1\] and nums\[j\] with nums\[j+1\] is wrong, because the "sandwich number" did not count into this situation.
* Second thought: compare nums\[j\] with nums\[j-2\] is also wrong, because if nums\[j-2\] number is replaced, then it is no more the original number 
* Third thought: Think about comparing nums\[j\] with nums\[i-2\], because i keeps all the twice duplicate number

```python
# Solution1: compare j with i-2th item 
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        # edge case 
        if not nums:
            return 0 
        # edge case 
        if len(nums) < 3:
            return len(nums) 
        
        i = 2
        for j in range(2, len(nums)):
            if nums[j] > nums[i-2]: # i-2 is stable, j-2 has been replaced by the new value 
                nums[i] = nums[j]
                i+=1
        return i 

```



