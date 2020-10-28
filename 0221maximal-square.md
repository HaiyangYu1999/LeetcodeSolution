Given a 2D binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.

**Example:**

```
Input: 

1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0

Output: 4
```

## 动态规划

第一次做这种二维数组最大矩阵的题还真没什么思路. 甚至连暴力可能都写不出来. 看了网上的解析之后才有思路. 希望以后这样的题会做吧.

`dp[i][j]`表示以坐标(i, j)为右下角元素的最大正方形.

那么显然如果`matrix[i][j] = '0'`, 那么就有`dp[i][j] = 0`. 

如果`matrix[i][j] = '1'`, 但是`matrix[i-1][j-1]`超出数组边界或者`matrix[i-1][j-1] = '0'` 就有`dp[i][j] = 1`

如果`matrix[i][j] = '1'` 并且`matrix[i-1][j-1]`没有数组边界, 我们假设`dp[i-1][j-1] = m`, 即在(i,j)的左上方有一个边长为m的正方形. 

为了以(i, j)也可以组成正方形, 左上角有一个m的正方形还不够, 还要左侧的矩形A域上方的矩形B也是1. 如图所示

```
m m m m m B
m m m m m B
m m m m m B
m m m m m B
m m m m m B
A A A A A 1  最后一个1的坐标为(i,j)
```

矩行A和矩行B只要有0的中断就不行. 只要中断, 正方形的size到这里就截止了

```
m m m m m 1
m m m m m 0
m m m m m 1
m m m m m 1
m m m m m 1
1 1 0 1 1 1
```

例如, 左侧的矩阵A在距离(i, j)为2的地方截止了. 即使上方的矩阵截止的地方为3, 左上方的正方形边长为5也没有用.

只取最小值2. 再加上本身的(i,j), 所以`dp[i][j] = 3`.

```java
class Solution {
    public int maximalSquare(char[][] matrix) {
        int n1 = matrix.length;
        if(n1 == 0)
            return 0;
        int n2 = matrix[0].length;
        int[][] dp = new int[n1][n2];
        for(int i = 0; i < n1; ++i)
        {
            for(int j = 0; j < n2; ++j)
            {
                char tmp = matrix[i][j];
                if(tmp == '0')
                    dp[i][j] = 0;
                else
                {
                    if(!isValid(matrix, i-1, j-1))
                    {
                        dp[i][j] = 1;
                    }
                    else
                    {
                        int m = dp[i-1][j-1];
                        int leftBarSize = 0;
                        for(int s = 1; s <= m; ++s)
                        {
                            if(matrix[i][j-s] == '1')
                                ++leftBarSize;
                            else
                                break;
                        }
                        int aboveBarSize = 0;
                        for(int s = 1; s <= m; ++s)
                        {
                            if(matrix[i-s][j] == '1')
                                ++aboveBarSize;
                            else
                                break;
                        }
                        int size = Math.min(aboveBarSize, leftBarSize);
                        dp[i][j] = size + 1;
                    }
                }
            }
        }
        int maxSize = 0;
        for(int i = 0; i < n1; ++i)
        {
            for(int j = 0; j < n2; ++j)
            {
                maxSize = Math.max(maxSize, dp[i][j]);
            }
        }
        return maxSize * maxSize;
    }
    private boolean isValid(char[][] matrix, int i, int j)
    {
        int n1 = matrix.length;
        int n2 = matrix[0].length;
        return i > -1 && j > -1 && i < n1 && j < n2;
    }
}
```

