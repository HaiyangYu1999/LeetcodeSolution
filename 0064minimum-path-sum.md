Given a *m* x *n* grid filled with non-negative numbers, find a path from top left to bottom right which *minimizes* the sum of all numbers along its path.

**Note:** You can only move either down or right at any point in time.

**Example:**

```
Input:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
Output: 7
Explanation: Because the path 1→3→1→1→1 minimizes the sum.
```

## 1 动态规划1

`dist[i][j] = min(dist[i][j + 1], dist[i + 1][j]) + grid[i][j]`

```c++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        vector<vector<int>> dist(grid.size(), vector<int>(grid.front().size(), -1));
        return minPathSum(dist, grid, 0, 0);
    }
private:
    int minPathSum(vector<vector<int>>& dist, const vector<vector<int>>& grid, int i, int j, int delimiterMark = -1)
    {
        if(i == grid.size() - 1 && j == grid.front().size() - 1)
            return grid[i][j];
        if(!isLocationValid(grid, i, j))
            return INT_MAX;
        if(dist[i][j] != delimiterMark)
            return dist[i][j];
        int right = minPathSum(dist, grid, i, j+1);
        int down = minPathSum(dist, grid, i+1, j);
        dist[i][j] = min(right, down) + grid[i][j];
        return dist[i][j];
    }
    bool isLocationValid(const vector<vector<int>>& grid, int i, int j)
    {
        return i < grid.size() && j < grid.front().size();
    }
};
```

## 2 动态规划2

对于这个题, 可以直接将dist信息储存在grid中, grid遍历过的储存的就是dist, 没有遍历过的还是储存的原来的值. 这样不需要new一个二维数组. 

依次计算(i,j)到(0,0)的距离, 返回grid[m-1]\[n-1]

```c++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        const int m = grid.size();
        const int n = grid.front().size();
        for(int i = 1; i < m; ++i)
            grid[i][0] += grid[i-1][0];
        for(int j = 1; j < n; ++j)
            grid[0][j] += grid[0][j-1];
        for(int i = 1; i < m; ++i)
        {
            for(int j = 1; j < n; ++j)
            {
                grid[i][j] += min(grid[i-1][j], grid[i][j-1]);
            }
        }
        return grid[m-1][n-1];
    }   
};
```



