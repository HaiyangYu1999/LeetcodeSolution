Given a **non-empty** array of integers, return the **third** maximum number in this array. If it does not exist, return the maximum number. The time complexity must be in O(n).

**Example 1:**

```
Input: [3, 2, 1]

Output: 1

Explanation: The third maximum is 1.
```



**Example 2:**

```
Input: [1, 2]

Output: 2

Explanation: The third maximum does not exist, so the maximum (2) is returned instead.
```



**Example 3:**

```
Input: [2, 2, 3, 1]

Output: 1

Explanation: Note that the third maximum here means the third maximum distinct number.
Both numbers with value 2 are both considered as second maximum.
```



## 优先队列

这个题思路很简单,但是有坑的地方

首先说思路,求第k大的数通常是构造k个元素的优先队列,这里由于优先队列里的元素不能重复, 所以手动用vector构造.

+ 第1步创建一个3个元素的优先队列, 并且都赋值为INT_MIN. 这时vector为递增排列
+ 第2步 对于每个元素nums[i], 和vector最前面的元素vector[0]比较,如果大于这个元素,并且和后两个元素不重复,就将vector[0] 替换为nums[i], 同时排序vector
+ 第3步 当遍历完nums[i]时，vector[0]即为第3大的元素。注意如果vector[0] 为INT_MIN，那么说明没有第三大的元素 e.g. {2,2,2,2,4,4}, 这时根据题意要返回最大的元素vector[2].

这个思路的复杂度低O(n), 也只需要常数的空间复杂度。

**但是，这个题坑的地方是，给的测试数组中包含INT_MIN = -2147483648 !!!**，上面的用INT_min作为最小值的策略就失效了。（**[1,2,-2147483648]**这样的数组不会得出正确结果 -2147483648）

所以，就要设置一个新的值表示所有的int的最小值，和-2147483648区分开。

> 有的做法为将int转换为long long，在设置 long long LONG_MIN = (long long)INT_MIN - 1，这是一种解决方法

我使用的方法是创建类型 

```c++
pair<int,bool> real_min {INT_MIN, false}
```

数组中的值nums[i]定义为 

```c++
pair<int,bool> fake {nums[i], false}
```

并重新定义大小关系 `{INT_MIN, false} < {INT_MIN, true}`。 这样即使nums中有INT_MIN也能正确区分是真的最小值还是只是-2147483648了

```c++
class Solution {
public:
    int thirdMax(vector<int>& nums) {
        if(nums.size() == 1)
            return nums[0];
        if(nums.size() == 2)
            return max(nums[0],nums[1]);
        pair<int,bool> pr = {INT_MIN, false};
        vector<pair<int,bool>> top3(3,pr);
        for(int i = 0; i < nums.size(); ++i)
        {
            pair<int,bool> tmp = {nums[i],true};
            if(cmp_pair(tmp,top3[0]) > 0 && cmp_pair(tmp,top3[1]) != 0 && cmp_pair(tmp,top3[2]) != 0)
            {
                top3[0] = tmp;
                sort(top3.begin(), top3.end(), cmp);
                cout<<"i="<<i<<endl;
                cout<<top3[0].first<<" "<<top3[1].first<<" "<<top3[2].first<<" "<<endl;
                cout<<top3[0].second<<" "<<top3[1].second<<" "<<top3[2].second<<" "<<endl;
            }
        }
        if(top3[0] == pr)
            return top3[2].first;
        return top3[0].first;
    }
private:
    int cmp_pair(const pair<int,bool>& a, const pair<int,bool>& b)
    {
        if(a.first != b.first)
        {
            return a.first > b.first ? 1 : -1;
        }
        else
        {
            if(a.second == false && b.second == true)
                return -1;
            if(a.second == true && b.second ==false)
                return 1;
        }
        return 0;
    }
    static bool cmp(const pair<int,bool>& a, const pair<int,bool>& b)
    {
        if(a.first != b.first)
            return a.first < b.first;
        else
        {
            return (a.second == false) && (b.second ==true);
        }

    }
};
```



