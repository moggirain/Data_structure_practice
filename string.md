# String

**Practice 1**

Remove duplicated and adjacent letters\(leave only one letter in each duplicated section\) in a string. 

Example 1:  "aaabbbccddcc" Output: "abcdc"

Example2:  "aaabbbaaa" Output: "aba"

```python
# Method 1: fast pointer and list 
# time complexity O(n) space complexity O(n)
def removeDuplicate(s):
    # corner case 
    if not s or len(s) < 2:
        return s
    fast = 0  
    res = []
    while fast < len(s):
        if len(res) == 0 or res[-1] != s[fast]:
            res.append(s[fast])
        fast +=1
    return "".join(res)  
    

# Method 2: two pointers
def removeDuplicate(s):
    # corner case 
    if not s or len(s) < 2:
        return s
    lst = list(s)
    fast, slow = 1, 1  
    while fast < len(lst):
        if lst[fast]!=lst[slow-1]:
            lst[slow] = lst[fast]
            slow+=1  
        fast +=1
    return "".join(lst[:slow]) 


if __name__ == "__main__":
    s = "aaabbbccddcc"
    t = "aaabbbaaa"
    print(removeDuplicate(s))
    print(removeDuplicate(t))
```

