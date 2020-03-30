# BFS

**994. Rotton Orange** 

```python
class Solution:
    def orangesRotting(self, grid: List[List[int]]) -> int:
        # corner case 
        # TC: O(m * n) SC: O(m *n)
        if not grid or not grid[0]: 
            return 0 
        m, n, time = len(grid), len(grid[0]), 0 
        directions = [(-1,0), (1,0), (0,-1),(0,1)]
        q = collections.deque()
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 2:
                    q.append((i, j, time))
        while q:
            x, y, time = q.popleft()
            for d in directions:
                x_new, y_new = x + d[0], y + d[1]
                if 0 <= x_new < m and 0<= y_new < n and grid[x_new][y_new] == 1:
                    grid[x_new][y_new] = 2 
                    q.append((x_new, y_new, time + 1))
        
        # evalute the rest of 1 
        for row in grid:
            if 1 in row:
                return -1
        return time 
            
    
```

