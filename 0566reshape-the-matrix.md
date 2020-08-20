In MATLAB, there is a very useful function called 'reshape', which can reshape a matrix into a new one with different size but keep its original data.

You're given a matrix represented by a two-dimensional array, and two **positive** integers **r** and **c** representing the **row** number and **column** number of the wanted reshaped matrix, respectively.

The reshaped matrix need to be filled with all the elements of the original matrix in the same **row-traversing** order as they were.

If the 'reshape' operation with given parameters is possible and legal, output the new reshaped matrix; Otherwise, output the original matrix.

**Example 1:**

```
Input: 
nums = 
[[1,2],
 [3,4]]
r = 1, c = 4
Output: 
[[1,2,3,4]]
Explanation:The row-traversing of nums is [1,2,3,4]. The new reshaped matrix is a 1 * 4 matrix, fill it row by row by using the previous list.
```



**Example 2:**

```
Input: 
nums = 
[[1,2],
 [3,4]]
r = 2, c = 4
Output: 
[[1,2],
 [3,4]]
Explanation:There is no way to reshape a 2 * 2 matrix to a 2 * 4 matrix. So output the original matrix.
```



**Note:**

1. The height and width of the given matrix is in range [1, 100].
2. The given r and c are all positive.

## 构造二维数组赋值

没什么难的, 没学过数据结构和算法的也能做出来, 注意要先判断给的r*c和nums的元素数量是否相同

```c++
class Solution {
public:
    vector<vector<int>> matrixReshape(vector<vector<int>>& nums, int r, int c) {
        if(nums.size() == 0)
            return nums;
        int count = nums.size() * nums.front().size();
        if(count != r * c)
        {
            return nums;
        }
        vector<int> tmp(c,0);
        vector<vector<int>> vvc(r,tmp);
        int new_i = 0;
        int new_j = 0;
        for(const vector<int>& i : nums)
        {
            for(int j : i)
            {
                vvc[new_i][new_j] = j;
                ++new_j;
                if(new_j == c)
                {
                    new_j = 0;
                    ++new_i;
                }
            }
        }
        return vvc;
    }
};
```

