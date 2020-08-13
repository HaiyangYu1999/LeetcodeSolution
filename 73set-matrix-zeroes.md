Given a *m* x *n* matrix, if an element is 0, set its entire row and column to 0. Do it [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm).

**Example 1:**

```
Input: 
[
  [1,1,1],
  [1,0,1],
  [1,1,1]
]
Output: 
[
  [1,0,1],
  [0,0,0],
  [1,0,1]
]
```

**Example 2:**

```
Input: 
[
  [0,1,2,0],
  [3,4,5,2],
  [1,3,1,5]
]
Output: 
[
  [0,0,0,0],
  [0,4,5,0],
  [0,3,1,0]
]
```

**Follow up:**

- A straight forward solution using O(*m**n*) space is probably a bad idea.
- A simple improvement uses O(*m* + *n*) space, but still not the best solution.
- Could you devise a constant space solution?

## 1 原地标记(失败)

本来打算用原地标记的. 某个元素加上(max-min+1)后为标记, 标记后的元素减去(max-min+1)可以还原为原来的元素.

但是测试案例中有INT_MIN和INT_MAX, 这样会溢出.

**我也想改进一下原地标记的方法,但我实在想不出来既兼容INT_MAX, INT_MIN, 又只有常数空间占用的方法了😭😭😭**

```c++
// This method failed when encounters [[-4,-2147483648,6,-7,0],[-8,6,-8,-6,0],[2147483647,2,-9,-6,-10]]
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int max0 = INT_MIN;
        int min0 = INT_MAX;
        for(const auto& i : matrix)
        {
            int max_tmp = *max_element(i.cbegin(), i.cend());
            int min_tmp = *min_element(i.cbegin(), i.cend());
            max0 = max(max0, max_tmp);
            min0 = min(min0, min_tmp);
        }
               
        for(int i = 0; i < matrix.size(); ++i)
            for(int j = 0; j < matrix.front().size(); ++j)
                if(undoMark(matrix[i][j], min0, max0) == 0)
                    markMatrix(matrix, i, j, min0, max0);
        
        for(int i = 0; i < matrix.size(); ++i)
            for(int j = 0; j < matrix.front().size(); ++j)
                if(isMarked(matrix[i][j], min0, max0))
                    matrix[i][j] = 0;
            
    }
private:
    int mark(int value, const int& min0, const int& max0)
    {
        if(value < min0)
            throw string("MinMax Input Error!");
        if(value > max0)
            return value;
        const long long diff = max0 - min0 + 1;
        return value + diff;
    }
    int undoMark(int value, const int& min0, const int& max0)
    {
        if(value < min0)
            throw string("MinMax Input Error!");
        if(value <= max0)
            return value;
        const long long diff = max0 - min0 + 1;
        return value - diff;
    }
    void markMatrix(vector<vector<int>>& matrix, int row, int column, int min0, int max0)
    {
        for(int i = 0; i < matrix.size(); ++i)
            matrix[i][column] = mark(matrix[i][column], min0, max0);
        for(int j = 0; j < matrix.front().size(); ++j)
            matrix[row][j] = mark(matrix[row][j], min0, max0);
    }
    bool isMarked(int value, int min0, int max0)
    {
        return value > max0;
    }
};
```

## 2 原地标记2 

考虑利用matrix的第0行和第0列来标记矩阵的某一行或某一列是否出现过0

1. 首先创建两个变量`ifrow0has0` 和`ifcolumn0has0` 并检查第0行和第0列是否含有0, 如果是, 将相应的值更新为true.(这时第0行和第0列的信息储存在这两个变量中了, 然后在上面标记含有0的列或含有0的行不会产生影响)
2. 从第1行第1列开始遍历整个矩阵, 若第i行第j列含有0, 则将第0行第j个元素标记为0, 和第i行第0个元素标记为0(意味着接下来第i行所有元素和第j列所有元素都要被归0)
3. 根据`ifrow0has0` 和`ifcolumn0has0` 判断是否将第0行的全部元素归0或将第0列的元素归0

```c++
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        bool ifrow0has0 = false;
        bool ifcolumn0has0 = false;
        for(int i = 0; i < matrix.front().size(); ++i)
        {
            if(matrix[0][i] == 0)
            {
                ifrow0has0 = true;
                break;
            }
        }
        for(int j = 0; j < matrix.size(); ++j)
        {
            if(matrix[j][0] == 0)
            {
                ifcolumn0has0 = true;
                break;
            }
        }
        
        for(int i = 1; i < matrix.size(); ++i)
        {
            for(int j = 1; j < matrix.front().size(); ++j)
            {
                if(matrix[i][j] == 0)
                {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        
        for(int i = 1; i < matrix.size(); ++i)
        {
            if(matrix[i][0] == 0)
            {
                for(int j = 1; j < matrix.front().size(); ++j)
                    matrix[i][j] = 0;
            }
        }
        
        for(int j = 1; j <matrix.front().size(); ++j)
        {
            if(matrix[0][j] == 0)
            {
                for(int i = 1; i < matrix.size(); ++i)
                    matrix[i][j] = 0;
            }
        }
        
        if(ifrow0has0)
        {
            for(int i = 0; i < matrix.front().size(); ++i)
                matrix[0][i] = 0;
        }
        if(ifcolumn0has0)
        {
            for(int i = 0; i < matrix.size(); ++i)
                matrix[i][0] = 0;
        }
    }
};
```

