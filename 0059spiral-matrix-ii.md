Given a positive integer *n*, generate a square matrix filled with elements from 1 to *n*2 in spiral order.

**Example:**

```
Input: 3
Output:
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]
```

## 解

思路和leetcode54一模一样

都先要根据规则找到下一个位置. 具体参考leetcode54

```c++
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> vvc(n,vector<int>(n,INT_MIN));
        int a = 1;
        pair<int,int> location = {0,0};
        vvc[0][0] = a++;
        while(hasNextPair(vvc,location))
        {
            getNextPair(vvc, location);
            vvc[location.first][location.second] = a++;
        }
        return vvc;
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
        return (j + 1 < m.front().size()) && (m[i][j+1] == INT_MIN);
    }
    bool isDownAvailable(const vector<vector<int>>& m, const pair<int,int>& pr)
    {
        int i = pr.first;
        int j = pr.second;
        return (i + 1 < m.size()) && (m[i+1][j] == INT_MIN);
    }
    bool isLeftAvailable(const vector<vector<int>>& m, const pair<int,int>& pr)
    {
        int i = pr.first;
        int j = pr.second;
        return (j - 1  >= 0) && (m[i][j-1] == INT_MIN);
    }
    bool isUpAvailable(const vector<vector<int>>& m, const pair<int,int>& pr)
    {
        int i = pr.first;
        int j = pr.second;
        return (i - 1 >= 0) && (m[i-1][j] == INT_MIN);
    }
};
```

