---
description: >-
  DP is an algorithmic technique for solving an optimization problem by breaking
  it down into simpler subproblems and the optimal solution to the main problem
  is dependent on subproblems.
---

# Dynamic Programing \(DP\)

Source: [https://www.educative.io/courses/grokking-dynamic-programming-patterns-for-coding-interviews/m2G1pAq0OO0](https://www.educative.io/courses/grokking-dynamic-programming-patterns-for-coding-interviews/m2G1pAq0OO0)

**509 Fibonacci Number** 

{% embed url="https://leetcode-cn.com/problems/fibonacci-number/submissions/" caption="Leetcode 509" %}

```python
# Solution 1: recursion 
# TC: O(2^N) - N is the depth of recursion
# SC: O(N) - N is the stack generated 
 
class Solution:
    def fib(self, N: int) -> int:
        if N == 0: 
            return 0 
        if N == 1: 
            return 1 
        return self.fib(N-1) + self.fib(N-2)
```

```python
# Solution 2: DP-topDown
# Problem in recursion: repeate calculation
# Solution: recursive memorization of the subproblem result
# TC: O(N) traverse all dp to save result 
# SC: O(N)

class Solution:
    def fib(self, N: int) -> int:
        self.dp = [-1 for x in range(N+1)]
        return self.memorize(N)
        
    def memorize(self, n):
        if n < 2: return n # base case 
        if self.dp[n] >= 0: # prunning, already calculated
            return self.dp[n]
        self.dp[n] = self.memorize(n-1) + self.memorize(n-2)
        return self.dp[n]
```

```python
# Solution 4: DP-bottomUp
# tabulation: solve all the subproblems first, as filling up an n-dimensional table, opposite of memorizaiton 
# every Fibonacci number is the sumer of the two proceeding numbers 
# TC: O(N) SC: O(N)
class Solution:
    def fib(self, N: int) -> int:
        dp = [0,1]
        for i in range(2, N+1):
            dp.append(dp[i-1] + dp[i-2])
        return dp[N]
```

