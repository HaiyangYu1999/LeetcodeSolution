A robot is located at the top-left corner of a *m* x *n* grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

![img](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)
Above is a 7 x 3 grid. How many possible unique paths are there?

 

**Example 1:**

```
Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Right -> Down
2. Right -> Down -> Right
3. Down -> Right -> Right
```

**Example 2:**

```
Input: m = 7, n = 3
Output: 28
```

 

**Constraints:**

- `1 <= m, n <= 100`
- It's guaranteed that the answer will be less than or equal to `2 * 10 ^ 9`.

## 1 动态规划

显然是从(i, j)到终点的可能路径数量等于 (i + 1, j)到终点的可能路径数 加上 (i, j + 1)到终点的可能路径数. 直接动态规划即可

时间复杂度m*n

```c++
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> grid(m, vector<int>(n,-1));
        return uniquePaths(grid, 0, 0);
    }
private:
    int uniquePaths(vector<vector<int>>& grid, int i, int j, int delimiterMark = -1)
    {
        if(i == grid.size() - 1 && j == grid.front().size() - 1)
            return 1;
        if(!isLocationValid(grid,i,j))
            return 0;
        if(grid[i][j] != delimiterMark)
            return grid[i][j];
        int right = uniquePaths(grid, i, j + 1);
        int down = uniquePaths(grid, i + 1, j);
        grid[i][j] = right + down;
        return grid[i][j];
    }
    bool isLocationValid(const vector<vector<int>>& grid, int i, int j)
    {
        return i < grid.size() && j < grid.front().size();
    }
};
```

## 2 排列组合

因为机器最后的目标是右下角，向下几步，向右几步都是固定的，

所以一共需要向下走m - 1步, 向右走n - 1步, 总共走m + n - 2步. 所以在这m + n - 2步里面挑m - 1步向下即可.

​                                                                                  $C_{m+n-2}^{m-1} = \frac{(m+n-2)!}{(m-1)!(n-1)!}$

时间复杂度O(m + n). **虽然这题一看就是动态规划, 但是被动态规划限制了思维, 没有更进一步考虑复杂度更低的算法, 不得不佩服大佬们敏锐的思维! 😞😞**



```c++
class Solution {
public:
    int uniquePaths(int m, int n) {
        return round(tgamma(m+n-1) / (tgamma(m) * tgamma(n)));
    }
};
```

