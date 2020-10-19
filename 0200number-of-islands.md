Given an `m x n` 2d `grid` map of `'1'`s (land) and `'0'`s (water), return *the number of islands*.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

 

**Example 1:**

```
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
```

**Example 2:**

```
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
```

 

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 300`
- `grid[i][j]` is `'0'` or `'1'`.

## 深度优先

肯定是挨个遍历二维数组的元素值了.

对于每一个遍历的位置(i, j), 如果该位置是1, 那么说明找到了一个岛屿, cnt自加, 并且把和这个1相连的所有1都置为2. 表示这个岛屿已经遍历过了, 接下来不应该再被计算一次. 用深度优先遍历所有与这个1相连的1即可.

```java
class Solution {
    public int numIslands(char[][] grid) {
        int cnt = 0;
        for(int i = 0; i < grid.length; ++i)
        {
            for(int j = 0; j < grid[0].length; ++j)
            {
                if(grid[i][j] == '1')
                {
                    ++cnt;
                    traverseIsland(grid, i, j);
                }
            }
        }
        return cnt;
    }
    private void traverseIsland(char[][] grid, int i, int j)
    {
        if(!isLocationValid(grid, i, j))
            return;
        if(grid[i][j] == '2')
            return;
        if(grid[i][j] == '1')
        {
            grid[i][j] = '2';
            traverseIsland(grid, i - 1, j);
            traverseIsland(grid, i + 1, j);
            traverseIsland(grid, i, j - 1);
            traverseIsland(grid, i, j + 1);
        }
    }
    private boolean isLocationValid(char[][] grid, int i, int j)
    {
        int m = grid.length;
        int n = grid[0].length;
        return i > -1 && i < m && j > -1 && j < n;
    }
}
```



