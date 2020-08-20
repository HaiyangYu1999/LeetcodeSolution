Given a non-negative integer *numRows*, generate the first *numRows* of Pascal's triangle.

![img](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)
In Pascal's triangle, each number is the sum of the two numbers directly above it.

**Example:**

```
Input: 5
Output:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

## 暴力算法

直接构造即可 复杂度O(n\^2)

注意numRows = 0, 1, 2的特殊情况

```c++
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> vvc;
        if(numRows == 0) return vvc;
        vvc.push_back(vector<int>({1}));
        if(numRows == 1) return vvc;
        vvc.push_back(vector<int>({1,1}));
        if(numRows == 2) return vvc;
        
        assert(numRows > 2);
        
        for(int j = 3; j <= numRows; ++j)
        {
            vector<int> tmp;
            tmp.push_back(1);
            for(int i = 0; i < j - 2; ++i)
            {
                int a = vvc.back()[i] + vvc.back()[i + 1];
                tmp.push_back(a);
            }
            tmp.push_back(1);
            vvc.push_back(tmp);
        }
        return vvc;
        
    }
};
```

