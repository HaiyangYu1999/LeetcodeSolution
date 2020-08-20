You are given an *n* x *n* 2D matrix representing an image.

Rotate the image by 90 degrees (clockwise).

**Note:**

You have to rotate the image [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm), which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.

**Example 1:**

```
Given input matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

rotate the input matrix in-place such that it becomes:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```

**Example 2:**

```
Given input matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

rotate the input matrix in-place such that it becomes:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]
```

## 原地移动

因为不让使用额外空间, 所以只能原地变换.

考虑m[i]\[j]顺时针旋转90°的变换

m[i]\[j] -->m[j]\[n-1-i]-->m[n-1-i]\[n-1-j]-->m[n-1-j]\[i]-->m[i]\[j]

所以只需要旋转一个元素就可以达到将这一个元素对应的4个位置的元素都旋转90的目的

例如  i = 1, j = 0. 只需要按照以下顺序操作一次, 对应的4个位置都会改变

```c++
				int tmp = m[i][j];
                m[i][j] = m[n-1-j][i];
                m[n-1-j][i] = m[n-1-i][n-1-j];
                m[n-1-i][n-1-j] = m[j][n-1-i];
                m[j][n-1-i] = tmp;
```

m[0]\[1] = 1, 10 <-- 1, 12 <-- 10, 13 <-- 12, 1 <-- 13 

[ 5, **1**, 9,11],
[ 2, 4, 8,**10**],
[**13**, 3, 6, 7],
[15,14,**12**,16]

所以只需要对整个矩阵的1/4的元素执行上述操作.如下图的黑体.当对5,1,9,4全部进行完旋转操作后

[ **5, 1, 9**,11],
[ 2, **4**, 8,10],
[13, 3, 6, 7],
[15,14,12,16]

```c++
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        vector<vector<int>>& m = matrix;
        if(m.empty())
            return;
        if(m.size() != m.front().size())
            throw string("InputInvalid");
        int n = m.size();
        int j_begin = 0;
        int j_end = n - 2;
        for(int i = 0; i < n; ++i)
        {
            if(j_begin > j_end)
                break;
            for(int j = j_begin; j <= j_end; ++j)
            {
                int tmp = m[i][j];
                m[i][j] = m[n-1-j][i];
                m[n-1-j][i] = m[n-1-i][n-1-j];
                m[n-1-i][n-1-j] = m[j][n-1-i];
                m[j][n-1-i] = tmp;
            }
            ++j_begin;
            --j_end;
        }
    }
};
```

