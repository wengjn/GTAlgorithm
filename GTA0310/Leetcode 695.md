## 695. Max Area of Island

### GTA tag
![img](../gta.png)

### AC record for the day
![img](./leetcode0310.png)

### Runtime Screenshot
![img](./runtime0310.png)

### Problem

*Medium*
[leetcode link here](https://leetcode.com/problems/max-area-of-island/)

Given a non-empty 2D array grid of 0's and 1's, an island is a group of 1's (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

Find the maximum area of an island in the given 2D array. (If there is no island, the maximum area is 0.)

**Example 1:**

```
[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]
 Given the above grid, return 6. Note the answer is not 11, because the island must be connected 4-directionally.
```

**Example 2:**

```
[[0,0,0,0,0,0,0,0]]
Given the above grid, return 0.
Note: The length of each dimension in the given grid does not exceed 50.
```

### Solution

- We want to get the max area of connected 1s in the grid.
- If we are on 1, DFS from this point to get the area, any 1s that directed connected will be counted into the area. Once it is counted, marked it as 0. This 0 is now working as a visited node and also ensure we don't count it more than once. In this way, we don't need another visited[m][n] grid to keep track of the visited island. 

### Complexity Analysis

- Time complexity: O(N), N is the total elements in the grid.
- Space complexity: O(1)

### Code

```c#
public class Solution {
    public int MaxAreaOfIsland(int[][] grid) {
        if (grid == null || grid.Length == 0 || grid[0].Length == 0) return 0;
        int ans = 0;
        
        for (int i=0; i<grid.Length; i++) {
            for (int j=0; j<grid[0].Length; j++) {
                if (grid[i][j] == 1) {
                    var area = DFS(grid, i, j);
                    ans = Math.Max(ans, area);
                }
            }
        }
        
        return ans;
    }
    
    public int DFS(int[][] grid, int i, int j) {
        if (i < 0 || j < 0 || i >= grid.Length || j >= grid[0].Length || grid[i][j] == 0) return 0;
        
        grid[i][j] = 0;
        
        return 1 + DFS(grid, i-1, j) +
                   DFS(grid, i+1, j) +
                   DFS(grid, i, j-1) +
                   DFS(grid, i, j+1);
    }
}
```