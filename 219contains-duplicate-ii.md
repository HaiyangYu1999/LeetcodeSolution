Given an array of integers and an integer *k*, find out whether there are two distinct indices *i* and *j* in the array such that **nums[i] = nums[j]** and the **absolute** difference between *i* and *j* is at most *k*.

**Example 1:**

```
Input: nums = [1,2,3,1], k = 3
Output: true
```

**Example 2:**

```
Input: nums = [1,0,1,1], k = 1
Output: true
```

**Example 3:**

```
Input: nums = [1,2,3,1,2,3], k = 2
Output: false
```

## 1 hash map1

使用一个从int映射到vector\<int\>的map，i从0遍历到n-1，

>  如果在map中出现与nums[i]相同的键，则当前的下标和vector中最后一个下标对比，若两者之差小于等于k，则返回true，否则把最新的下标i加入到vector中

> 如果在map中没有出现与nums[i]相同的键，则直接在map中insert {nums[i], vector{i}}

```c++
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        unordered_map<int, vector<int>> mp;
        for(int i = 0; i < nums.size(); ++i)
        {
            unordered_map<int, vector<int>>::iterator it = mp.find(nums[i]);
            bool isFound_nums_i = (it != mp.end());
            if(isFound_nums_i)
            {
                int last_index = it -> second.back();
                if(i - last_index <= k) 
                    return true;
                else
                {
                    it -> second.push_back(i);
                }
            }
            else
            {
                mp.insert({nums[i], vector<int>{i}});
            }
        }
        return false;
    }
};
```

## 2 hash map2

发现方法1并不需要一个vector作为值, 因为在迭代过程中只需要比较最后一个出现的index和当前的i的大小关系. 所以设<key, value>为<int, int>，如果出现了相同的值，并且i和前一个index的差大于k，则直接把前一个index替换为i即可。

```c++
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        unordered_map<int, int> mp;
        for(int i = 0; i < nums.size(); ++i)
        {
            auto it = mp.find(nums[i]);
            bool isFound_nums_i = (it != mp.end());
            if(isFound_nums_i)
            {
                int last_index = it -> second;
                if(i - last_index <= k) 
                    return true;
                else
                {
                    it -> second = i;
                }
            }
            else
            {
                mp.insert({nums[i], i});
            }
        }
        return false;
    }
};
```

