Given an array of integers, 1 ≤ a[i] ≤ *n* (*n* = size of array), some elements appear **twice** and others appear **once**.

Find all the elements that appear **twice** in this array.

Could you do it without extra space and in O(*n*) runtime?



**Example:**

```
Input:
[4,3,2,7,8,2,3,1]

Output:
[2,3]
```

## 1 暴力

想不出来O(1)空间的了

```c++
class Solution {
public:
    vector<int> findDuplicates(vector<int>& nums) {
        unordered_map<int, int> mp;
        vector<int> vc;
        for(int i : nums)
            ++mp[i];
        for(auto& i : mp)
            if(i.second == 2)
                vc.push_back(i.first);
        return vc;
    }
};
```

## 2 原地标记

思路借鉴于: [hou-yong-sheng](https://leetcode-cn.com/problems/find-all-duplicates-in-an-array/solution/c-qiao-miao-zi-xing-ha-xi-by-hou-yong-sheng/) (leetcode-cn)

利用nums[i] 属于 [1,n]的特点, 每一个值都可以对应一个下标. 这里使用值i对应i-1下标

**如果读到一个值i, 就把nums[i-1]的值置为负数. 如果再次读到i时, 检查nums[i-1]是不是为负数， 如果是的话, 说明前面肯定出现过i, 重复!!**

```c++
class Solution {
public:
    vector<int> findDuplicates(vector<int>& nums) {
        vector<int> vc;
        for(int i : nums)
        {
            i = abs(i);
            if(nums[i-1] < 0)
                vc.push_back(i);
            else
                nums[i-1] = -nums[i-1];
        }
        return vc;
    }
};
```

