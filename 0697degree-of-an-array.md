Given a non-empty array of non-negative integers `nums`, the **degree** of this array is defined as the maximum frequency of any one of its elements.

Your task is to find the smallest possible length of a (contiguous) subarray of `nums`, that has the same degree as `nums`.

 

**Example 1:**

```
Input: nums = [1,2,2,3,1]
Output: 2
Explanation: 
The input array has a degree of 2 because both elements 1 and 2 appear twice.
Of the subarrays that have the same degree:
[1, 2, 2, 3, 1], [1, 2, 2, 3], [2, 2, 3, 1], [1, 2, 2], [2, 2, 3], [2, 2]
The shortest length is 2. So return 2.
```

**Example 2:**

```
Input: nums = [1,2,2,3,1,4,2]
Output: 6
Explanation: 
The degree is 3 because the element 2 is repeated 3 times.
So [2,2,3,1,4,2] is the shortest subarray, therefore returning 6.
```

 

**Constraints:**

- `nums.length` will be between 1 and 50,000.
- `nums[i]` will be an integer between 0 and 49,999.

## 1 暴力算法

先创建个map找到数组的degree(1次遍历), 再找到所有的出现次数为degree的元素(1次遍历, 因为出现次数最多的元素可能不止一个. 假设有k个元素的出现次数最多). 对于每个出现次数最多的元素找到第一次出现的位置和最后一次出现的位置, 由这两个位置计算长度(k次遍历). 再从k个长度里面返回最小的长度.  一共需要k+2次遍历. 

但是看完网上各种答案之后, 感觉并没有更好的思路. 所有的思路都是找到所有出现次数为degree的元素, 再从这些元素中计算每个元素的最小长度, 最后取最小值. 有些方法虽然降低了遍历的次数但增加了空间复杂度. 各有利弊吧. 目前没有找到时间或空间低于O(n)的方法

```c++
class Solution {
public:
    int findShortestSubArray(vector<int>& nums) {
        if(nums.size() == 0 || nums.size() == 1)
            return nums.size();
        unordered_map<int, int> mp;
        for(auto& i : nums)
        {
            ++mp[i];
        }
        int maxFrequency = 0;
        for(auto& i : mp)
        {
            if(i.second > maxFrequency)
            {
                maxFrequency = i.second;
            }
        }
        
        vector<int> frequencyNumbers;
        for(auto& i : mp)
        {
            if(i.second == maxFrequency)
                frequencyNumbers.push_back(i.first);
        }
        
        vector<int> len;
        for(auto frequencyNum : frequencyNumbers)
        {
            int begin = 0;
            while(begin < nums.size())
            {
                if(nums[begin] == frequencyNum)
                    break;
                ++begin;
            }
            int end = nums.size() - 1;
            while(end > -1)
            {
                if(nums[end] == frequencyNum)
                    break;
                --end;
            }
            len.push_back(end - begin + 1);
        }
        return *min_element(len.begin(), len.end());
    }
};
```

