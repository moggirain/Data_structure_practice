---
description: The archive of tree-based questions.
---

# BST

### 110. Balanced Binary Tree 

{% embed url="https://leetcode-cn.com/problems/balanced-binary-tree/" %}

Solution 1: Top-Down DFS   

```python
# Solution 1: Top Down DFS TC: O(nlogn) SC: O(n)
# recursively evaluate left and right subtree balance 
# balance criteria: abs(h(left subtree) - h(right subtree)) < 2 
# tree height: 1 + max(left_height, right_height)

class TreeNode():
    def __init__(self, x):
        self.val = x
        self.left = None 
        self.right = None 

class Solution:
    def isBalanced(self, root):
        if not root: return True 
       
        return abs(self.height(root.left) - self.height(root.right)) < 2 \
        and self.isBalanced(root.left) and self.isBalanced(root.right) 
        
    def height(self, root): 
        if not root: return 0 
        left = self.height(root.left)
        right = self.height(root.right)
        return 1 + max(left, right)
```

```python
# Solution 2: DFS Bottom up TC: O(n) SC: O(n)
# Optimization: solution 1 repeately called height function
# calculate height from left and right at each level and compare height difference 
# only return maximum height when left and right is balanced 

class TreeNode():
    def __init__(self, x):
        self.val = x
        self.left = None 
        self.right = None 

# 
# tree height: 1 + max(left_height, right_height)
class Solution:
    def isBalanced2(self, root):
        if not root: return True 
        return self.height(root) != -1
       
        
    def height(self, root): 
        if not root: return 0 
        left = self.height(root.left)
        if left == -1: 
            return -1
        right = self.height(root.right)
        if right == -1:
   `         return -1
        return 1 + max(left, right) if abs(left - right) < 2 else -1
```

### 112. Path Sum 

{% embed url="https://leetcode-cn.com/problems/path-sum/" %}

```python
# Solution 1: DFS TC: O(n) SC:O(1)
# recursion: resursively search from left and right path sum  
# record sum along the path 
# exit: no left child and no right child and sum == 0
# 
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

class Solution:
    def hasPathSum(self, root: TreeNode, sum: int) -> bool:
        if not root:
            return False 
        sum-= root.val
        # exit 
        if not root.left and not root.right and sum == 0:
            return True 
        return self.hasPathSum(root.left, sum) or self.hasPathSum(root.right, sum)
```

```python
# Solution 2: FFS TC: O(n) SC:O(n)
# exit:  no left child and no right child and sum == 0
# continue to check the sum at each level 
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

class Solution:
    def hasPathSum(self, root: TreeNode, sum: int) -> bool:
        if not root: return False 
        q = collections.deque([[root, sum - root.val]])
        while q:
            node, cursum = q.popleft()
            if not node.left and not node.right and cursum == 0:
                return True 
            if node.left:
                q.append([node.left, cursum - node.left.val])
            if node.right:
                q.append([node.right, cursum - node.right.val])
        return False 
```

### **Problem- Find the second largest item in a BST**

Find the second largest value inside a binary search tree. You can assume that a node inside this tree only contains a single value. 

```python
# with right sub tree, the 2nd largest is the root
# without right sub tree, the 2nd largest is the max of left tree
# TC: O(h) SC: O(1)
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None 
        self.right = None 

class Solution: 
    def FindSecondLargest(self, root):
        self.cnt = 0 
        self.res = None 
        self.dfs(root)
        return self.res if self.res else None
		
    def dfs(self, root):
      	if not root: # base case 
          	return
        self.dfs(root.right)
        self.cnt+=1
        if self.cnt == 2:
          	self.res = root.val 
            print("the second largest element is", root.val)
            return 
        if self.cnt > 2: # prunning
          	return 
       	self.dfs(root.left)
   
  	def insert(self, root, val):
      	if not root:
        		return TreeNode(val)
        if val < root.val:
          	root.left = self.insert(root.left, val)
        elif val > root.val:
          	root.right = self.insert(root.right, val)
        return root
      

if __name__ == '__main__': 
    # Let us create following BST  
    #         10  
    #       /     \  
    #       5     20  
    #    /   \       \  
    #   3     8   11    30
    #  / 
    # 2
    root = None
    s = Solution()
    root = s.insert(root, 10)  
    s.insert(root, 5) 
    s.insert(root, 3)
    s.insert(root, 8)
    s.insert(root, 2)
    s.insert(root, 20)  
    s.insert(root, 30)  
    s.insert(root, 40)  
    s.insert(root, 50)  
    s.FindSecondLargest(root)
```

