Given an array of integers, find if the array contains any duplicates.

Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.

**Example 1:**

```
Input: [1,2,3,1]
Output: true
```

**Example 2:**

```
Input: [1,2,3,4]
Output: false
```

**Example 3:**

```
Input: [1,1,1,3,3,4,3,2,4,2]
Output: true
```

## hash set

i从1遍历到nums.size() - 1, 使用一个hash set 储存已经出现过的元素, 如果再次出现返回true.

```c++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        unordered_set<int> st;
        for(int i = 0; i < nums.size(); ++i)
        {
            bool isFound = (st.find(nums[i]) != st.end());
            if(isFound) return true;
            else
            {
                st.insert(nums[i]);
            }
        }
        return false;
    }
};
```

