A robot is located at the top-left corner of a *m* x *n* grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

Now consider if some obstacles are added to the grids. How many unique paths would there be?

![img](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

An obstacle and empty space is marked as `1` and `0` respectively in the grid.

**Note:** *m* and *n* will be at most 100.

**Example 1:**

```
Input:
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
Output: 2
Explanation:
There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right
```

## 动态规划

和leetcode 62相同的方法. 只不过要加一个判断是不是obstacle的条件. 若(i,j)是obstacle, 那么(i,j)到终点的可能的路径数为0.

```c++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        vector<vector<int>> paths(obstacleGrid.size(), vector<int>(obstacleGrid.front().size(), -1));
        return uniquePathsWithObstacles(paths, obstacleGrid, 0, 0);
    }
private:
    int uniquePathsWithObstacles(vector<vector<int>>& paths, const vector<vector<int>>& obstacleGrid, int i, int j, int delimiterMark = -1, int obstacleMark = 1)
    {
        if(i == obstacleGrid.size() - 1 && j == obstacleGrid.front().size() - 1)
            return obstacleGrid[i][j] == obstacleMark ? 0 : 1;
        if(!isLocationValid(obstacleGrid, i, j))
            return 0;
        if(paths[i][j] != delimiterMark)
            return paths[i][j];
        int right = uniquePathsWithObstacles(paths, obstacleGrid, i, j+1);
        int down = uniquePathsWithObstacles(paths, obstacleGrid, i+1, j);
        paths[i][j] = right + down;
        return paths[i][j];
    }
    bool isLocationValid(const vector<vector<int>>& obstacleGrid, int i, int j, int delimiterMark = -1, int obstacleMark = 1)
    {
        return i < obstacleGrid.size() && j < obstacleGrid.front().size() && obstacleGrid[i][j] != obstacleMark;
    }
};
```

