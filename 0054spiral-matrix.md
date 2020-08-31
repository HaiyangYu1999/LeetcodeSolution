Given a matrix of *m* x *n* elements (*m* rows, *n* columns), return all elements of the matrix in spiral order.

**Example 1:**

```
Input:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
Output: [1,2,3,6,9,8,7,4,5]
```

**Example 2:**

```
Input:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
```

## 螺旋遍历

首先要对已经遍历过的元素做个标记INT_MIN(一开始想用-1做标记, 但是给的test cases里有-1这个元素).

对于某个location{i,j}, 分别检查左边,右边, 上边,下边的元素是不是还没有遍历过并且没有超出边界.

  [ 1,  2,  **3**,  4],
  [ 5,  6,  7,  8],
  [ **9**, 10,11,**12**],
  [13,**14**,15,16]


1. 如果一个元素的右边和下边都没有遍历 接下来要先遍历右边 对于上图元素3,接下来要遍历4
2. 如果一个元素的下边和左边都没有遍历 接下来要先遍历下边 对于上图元素12,接下来要遍历16
3. 如果一个元素的左边和上边都没有遍历 接下来要先遍历左边 对于上图元素14, 接下来要遍历13
4. 如果一个元素的上边和右边都没有遍历 接下来要先遍历上边 对于上图元素9, 接下来要遍历5
5. 其他情况, 对应角上的元素, 例如4,16,13 只有一个方向没有遍历过. (4只能往下走, 16只能往左走, 13只能往上走) 那就遍历这个方向.



```c++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> vc;
        if(matrix.empty() || matrix.front().empty())
            return vc;
        
        pair<int,int> pr = {0,0};
        vc.push_back(matrix[0][0]);
        matrix[0][0] = INT_MIN;
        
        while(hasNextPair(matrix,pr))
        {
            getNextPair(matrix,pr);
            vc.push_back(matrix[pr.first][pr.second]);
            matrix[pr.first][pr.second] = INT_MIN;
        }
        return vc;
    }
private:
    bool hasNextPair(const vector<vector<int>>& m, const pair<int,int>& pr)
    {
        return isRightAvailable(m,pr) || isDownAvailable(m,pr) || isLeftAvailable(m,pr) || isUpAvailable(m,pr);
    }
    
    void getNextPair(const vector<vector<int>>& m, pair<int,int>& pr)
    {
        if(isRightAvailable(m,pr) && isDownAvailable(m,pr))
        {
            ++pr.second;
        }
        else if(isDownAvailable(m,pr) && isLeftAvailable(m,pr))
        {
            ++pr.first;
        }
        else if(isLeftAvailable(m,pr) && isUpAvailable(m,pr))
        {
            --pr.second;
        }
        else if(isUpAvailable(m,pr) && isRightAvailable(m,pr))
        {
            --pr.first;
        }
        else if(isRightAvailable(m,pr))
        {
            ++pr.second;
        }
        else if(isDownAvailable(m,pr))
        {
            ++pr.first;
        }
        else if(isLeftAvailable(m,pr))
        {
            --pr.second;
        }
        else if(isUpAvailable(m,pr))
        {
            --pr.first;
        }
    }
    
    bool isRightAvailable(const vector<vector<int>>& m, const pair<int,int>& pr)
    {
        int i = pr.first;
        int j = pr.second;
        return (j + 1 < m.front().size()) && (m[i][j+1] != INT_MIN);
    }
    bool isDownAvailable(const vector<vector<int>>& m, const pair<int,int>& pr)
    {
        int i = pr.first;
        int j = pr.second;
        return (i + 1 < m.size()) && (m[i+1][j] != INT_MIN);
    }
    bool isLeftAvailable(const vector<vector<int>>& m, const pair<int,int>& pr)
    {
        int i = pr.first;
        int j = pr.second;
        return (j - 1  >= 0) && (m[i][j-1] != INT_MIN);
    }
    bool isUpAvailable(const vector<vector<int>>& m, const pair<int,int>& pr)
    {
        int i = pr.first;
        int j = pr.second;
        return (i - 1 >= 0) && (m[i-1][j] != INT_MIN);
    }
};
```

