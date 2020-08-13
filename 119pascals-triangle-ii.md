Given a non-negative index *k* where *k* ≤ 33, return the *k*th index row of the Pascal's triangle.

Note that the row index starts from 0.

![img](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)
In Pascal's triangle, each number is the sum of the two numbers directly above it.

**Example:**

```
Input: 3
Output: [1,3,3,1]
```

**Follow up:**

Could you optimize your algorithm to use only *O*(*k*) extra space?

## 解

时间复杂度仍需要O(n\^2),注意到每一行的vector只需要由上一行来计算，和更上面的元素无关

所以当有第k行时，直接在第k行的基础上计算并覆盖第k+1行

要注意提前拷贝出来要被覆盖的第k行的值，并且注意最后加上1

```c++
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        int k = rowIndex;
        if(k == 0)
            return vector<int>({1});
        if(k == 1)
            return vector<int>({1,1});
        vector<int> vc({1,2,1});
        for(int i = 2; i < k; ++i)
        {
            int tmp_jMinus1 = vc[0];   //copy to prevent being covered
            int tmp_j = vc[1];
            for(int j = 1; j < i; ++j)
            {
                int tmp_val = tmp_jMinus1 + tmp_j;
                tmp_jMinus1 = tmp_j;
                tmp_j = vc[j+1];        //copy to prevent being covered
                vc[j] = tmp_val;      //covering the value
            }
            vc.back() = tmp_jMinus1 + tmp_j;   //change the second last value
            vc.push_back(1);                   //add the last one
        }
        return vc;
    }
};
```

