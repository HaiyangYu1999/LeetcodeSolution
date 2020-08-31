Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

For example, given the following triangle

```
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```

The minimum path sum from top to bottom is `11` (i.e., **2** + **3** + **5** + **1** = 11).

**Note:**

Bonus point if you are able to do this using only *O*(*n*) extra space, where *n* is the total number of rows in the triangle.

## 1 从上到下计算

由于求top到第k层的最短距离只依赖于top到第k-1层的距离,

创建一个vector shortest 储存top到第k层每一个元素的距离, 计算k+1层时直接覆盖shortest. 最后取shortest中的最小值

此方法需要考虑很多的边界条件 (我改正heap-overflow的错误至少花了30分钟),

+ 创建两个变量tmp_j和tmp_jPlus1保存将要被覆盖的变量，所以triangle[k]至少要有2个元素，否则内存错误。这也就意味着triangle.size() = 0或1时要单独讨论。

+ 对于第k层的第0个元素，其最短距离只与第k-1层第0个元素有关，不需要比较两个相邻的值，所以应该单独求解

+ 对于第k层的倒数第二个元素，最短距离由比较第k-1层的倒数第二个元素和倒数第一个元素得出，但是得出结果之后不能更新tmp_j和tmp_jPlus1，因为此时的tmp_jPlus1已经是shortest中最后一个元素了，再+1会内存错误

+ 对于第k层的最后一个元素，要使用push_back的方式添加，而不是修改shortest.back()，因为上一层的shortest元素比这一层的少1，所以要+1

```c++
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int n = triangle.size();
        vector<int> shortest;
        if(n == 0)
            return 0;
        shortest.push_back(triangle[0][0]);
        if(n == 1)
            return triangle[0][0];
        
        //makesure shortest has at least 2 elements
        shortest[0] = triangle[0][0] + triangle[1][0];
        shortest.push_back(triangle[0][0] + triangle[1][1]);
        
        for(int i = 2; i < n; ++i)
        {
            int tmp_j = shortest[0];     //backup variable
            int tmp_jPlus1 = shortest[1];   //why shortest has at least 2 elements
            shortest[0] = tmp_j + triangle[i][0]; //shortest[i][0] must come from shortest[i-1][0]
            for(int j = 1; j < triangle[i].size() - 2; ++j)
            {
                shortest[j] = min(tmp_j,tmp_jPlus1) + triangle[i][j];
                tmp_j = tmp_jPlus1;
                tmp_jPlus1 = shortest[j + 1];
            }
            //secondly last one cannot operate j+1
            int second_last_index = triangle[i].size() - 2;
            shortest[second_last_index] = min(tmp_j,tmp_jPlus1) + triangle[i][second_last_index];
            //shortest[i][i] must come from shortest[i-1][i-1]
            int last = tmp_jPlus1 + triangle[i].back(); 
            shortest.push_back(last);
        }
        
        return *min_element(shortest.begin(),shortest.end());
    }
};
```



  ## 2 从下到上计算

**从上到下的最短距离等于从下到上的最短距离!**

`dist_triangle[i]\[j]`的距离等于`min(dist_triangle[i+1][j], dist_triangle[i+1][j+1]) + triangle[i][j]`

i从triangle.size()-2遍历到0， 不需要考虑边界条件，内存错误数组溢出的问题，也不需要考虑新建vector储存最短距离，直接可以覆盖到triangle上，使用O(1)的extra space。

只需要考虑triangle.size() == 0的特殊条件

```c++
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        if(triangle.size() == 0)
            return 0;
        for(int i = triangle.size() - 2; i >= 0; --i)
        {
            for(int j = 0; j <= i; ++j)
            {
                triangle[i][j] = min(triangle[i+1][j],triangle[i+1][j+1]) + triangle[i][j];
            }
        }
        return triangle[0][0];
    }
};
```

