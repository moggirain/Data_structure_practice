# Sorting

Through question Leetcode 912, let's dive into the sorting algorithms. 

{% embed url="https://leetcode-cn.com/problems/sort-an-array/" %}

### Sorting 

* Search through a sequence of objects based on some key words
* Put a list of elements into order. 

### Property

* **Stability:** Equal keys aren’t reordered. For example, a = 3, b = 3, a is before b and after sorting, the order still maintain. 
* **In place operation:** All operation happens in the memory, requiring O\(1\) extra space.
* **Adaptivity:** Speeds up to O\(n\) when data is nearly sorted or when there are few unique keys.

### Algorithms 

#### Selection Sort 

Array implementation: Unstable; LinkedList implementation: Stable.

TC: O\(n^2\)

```python
def selection_sort(nums):
    n = len(nums)
    i = 0 
    for i in range(n):
        j = i 
        for j in range(i, n):
            if 
            
    
```



