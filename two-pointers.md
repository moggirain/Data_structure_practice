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



