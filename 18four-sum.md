Given an array `nums` of *n* integers and an integer `target`, are there elements *a*, *b*, *c*, and *d* in `nums` such that *a* + *b* + *c* + *d* = `target`? Find all unique quadruplets in the array which gives the sum of `target`.

**Note:**

The solution set must not contain duplicate quadruplets.

**Example:**

```
Given array nums = [1, 0, -1, 0, -2, 2], and target = 0.

A solution set is:
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```

## 双指针

完全参考leetcode15 3sum的双指针法,只不过在外面又加了一层循环

```c++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> vvc;
        if(nums.size() < 4)
            return vvc;
        for(int i = 0; i < nums.size(); ++i)
        {
            if(i > 0 && nums[i] == nums[i - 1])
                continue;
            int a = nums[i];
            for(int j = i + 1; j < nums.size(); ++j)
            {
                if(j > i + 1 && nums[j] == nums[j - 1])
                    continue;
                int b = nums[j];
                int k = j + 1;
                int m = nums.size() - 1;
                while(k < m)
                {
                    if(k > j + 1 && nums[k] == nums[k - 1])
                    {
                        ++k;
                        continue;
                    }
                    if(m < nums.size() - 1 && nums[m] == nums[m+1])
                    {
                        --m;
                        continue;
                    }
                    int c = nums[k];
                    int d = nums[m];
                    int sum = a + b + c + d;
                    if(sum == target)
                    {
                        vector<int> tmp = {a,b,c,d};
                        vvc.push_back(tmp);
                        ++k;
                        --m;
                    }
                    else if(sum < target)
                        ++k;
                    else
                        --m;
                }
            }
        }
        return vvc;
    }
};
```

