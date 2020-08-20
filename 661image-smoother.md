Given a 2D integer matrix M representing the gray scale of an image, you need to design a smoother to make the gray scale of each cell becomes the average gray scale (rounding down) of all the 8 surrounding cells and itself. If a cell has less than 8 surrounding cells, then use as many as you can.

**Example 1:**

```
Input:
[[1,1,1],
 [1,0,1],
 [1,1,1]]
Output:
[[0, 0, 0],
 [0, 0, 0],
 [0, 0, 0]]
Explanation:
For the point (0,0), (0,2), (2,0), (2,2): floor(3/4) = floor(0.75) = 0
For the point (0,1), (1,0), (1,2), (2,1): floor(5/6) = floor(0.83333333) = 0
For the point (1,1): floor(8/9) = floor(0.88888889) = 0
```



**Note:**

1. The value in the given matrix is in the range of [0, 255].
2. The length and width of the given matrix are in the range of [1, 150].

## 暴力解法

没什么难的, 判断好是否是边界元素,然后计算就行了

推荐写一个私有方法判断位置是否越界, 要不然if里面的条件太复杂

```c++
class Solution {
public:
    vector<vector<int>> imageSmoother(vector<vector<int>>& M) {
        int m = M.size();
        int n = M.front().size();
        auto mean = M;
        for(int i = 0; i < m; ++i)
        {
            for(int j = 0; j < n; ++j)
            {
                vector<int> tmp;
                if(isLocationValid(i - 1, j - 1, m, n))
                    tmp.push_back(M[i-1][j-1]);
                if(isLocationValid(i - 1, j, m, n))
                    tmp.push_back(M[i-1][j]);
                if(isLocationValid(i - 1, j + 1, m, n))
                    tmp.push_back(M[i-1][j+1]);
                if(isLocationValid(i, j - 1, m, n))
                    tmp.push_back(M[i][j-1]);
                if(isLocationValid(i, j, m, n))
                    tmp.push_back(M[i][j]);
                if(isLocationValid(i, j + 1, m, n))
                    tmp.push_back(M[i][j+1]);
                if(isLocationValid(i + 1, j - 1, m, n))
                    tmp.push_back(M[i+1][j-1]);
                if(isLocationValid(i + 1, j, m, n))
                    tmp.push_back(M[i+1][j]);
                if(isLocationValid(i + 1, j + 1, m, n))
                    tmp.push_back(M[i+1][j+1]);
                mean[i][j] = accumulate(tmp.begin(), tmp.end(), 0) / tmp.size();
            }
        }
        return mean;
    }
private:
    bool isLocationValid(int i, int j, int bound1, int bound2)
    {
        return i>=0 && i < bound1 && j >=0 && j <bound2;
    }
};
```

