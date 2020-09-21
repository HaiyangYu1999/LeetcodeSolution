Write an efficient algorithm that searches for a value in an *m* x *n* matrix. This matrix has the following properties:

- Integers in each row are sorted from left to right.
- The first integer of each row is greater than the last integer of the previous row.

**Example 1:**

```
Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3
Output: true
```

**Example 2:**

```
Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13
Output: false
```

## 1 2次二分查找

先二分查找target所在的行号row, 然后在row中二分查找target. 

对于第一次二分查找, 若target大于mid行中的最后一个元素, 说明在mid行之后, 若小于mid行中的第0个元素, 说明在mid行之前, 否则就是在mid行里

```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if(matrix.empty() || matrix.front().empty())
            return false;
        const int m = matrix.size();
        const int n = matrix.front().size();
        int begin = 0;
        int end = m - 1;
        int row = -1;
        if(target < matrix[0][0] || target > matrix[m-1][n-1])
            return false;
        while(begin <= end)
        {
            int mid = begin + (end - begin) / 2;
            if(target < matrix[mid][0])
            {
                end = mid - 1;
            }
            else if(target > matrix[mid][n-1])
            {
                begin = mid + 1;
            }
            else
            {
                row = mid;
                break;
            }
        }
        if(row == -1)
            return false;
        else
            return binary_search(matrix[row].begin(), matrix[row].end(), target);
    }
};
```

## 2 1次二分查找

将矩阵想象成一个长的数组, 对这个数组用二分查找. 只不过在寻址的时候要变换一下

**长数组中第i个元素为矩阵中 第 i / n 行 第 i % n 列的元素 (n是矩阵列数)**

是**"计算机科学领域的任何问题都可以通过增加一个间接的中间层来解决"**的思想的应用 在这个题中, 中间层即为函数get1DValue(), 给调用的人一种假象, 用的就是一维数组

```c++
int get1DValue(const vector<vector<int>>& matrix, int index){
    return matrix[index / matrix.front().size()][index % matrix.front().size()];
}
```



```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if(matrix.empty() || matrix.front().empty())
            return false;
        if(target < matrix.front().front() || target > matrix.back().back())
            return false;
        const int m = matrix.size();
        const int n = matrix.front().size();
        int begin = 0;
        int end = m * n - 1;
        while(begin <= end)
        {
            int mid = begin + (end - begin) / 2;
            int midValue = get1DValue(matrix, mid);
            if(target > midValue)
            {
                begin = mid + 1;
            }
            else if(target < midValue)
            {
                end = mid - 1;
            }
            else
                return true;
        }
        return false;
    }
private:
    int get1DValue(const vector<vector<int>>& matrix, int index)
    {
        return matrix[index / matrix.front().size()][index % matrix.front().size()];
    }
};
```

