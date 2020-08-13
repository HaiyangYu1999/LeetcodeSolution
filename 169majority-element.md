Given an array of size *n*, find the majority element. The majority element is the element that appears **more than** `⌊ n/2 ⌋` times.

You may assume that the array is non-empty and the majority element always exist in the array.

**Example 1:**

```
Input: [3,2,3]
Output: 3
```

**Example 2:**

```
Input: [2,2,1,1,1,2,2]
Output: 2
```



## 1 暴力法

维护一个hashmap,对应数组中的值和出现的次数, 遍历数组后, 然后找map中出现次数最高的元素的值,

由于创建这个hashmap的复杂度为n，map中找最大的也为n 所以总复杂度为O(n)

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        unordered_map<int, int> mp;
        for(int i : nums)
        {
            if(mp.find(i) == mp.end())
            {
                mp.insert({i,1});
            }
            else
            {
                ++mp.find(i)->second;
            }
        }
        int count = 0;
        int res = 0;
        for(const pair<int,int>& i : mp)
        {
            if(i.second > count)
            {
                count = i.second;
                res = i.first;
            }
        }
        return res;
    }
};
```

## 2 摩尔投票

观察到，如果有一个出现次数大于n/2的数，那么从左到右遍历一遍，只要发现两个不一样的数就划掉这两个数，最后剩下的数一定是最多的数。

所以构造一个值current记录当前的数值，和一个计数器count；如果读到下一个值和current相同，计数器加一，如果和current不同，就令count - 1，当count < 0时，更新current的值为nums[i]，同时将count置为0

遍历完后的current即为所求.虽然时间复杂度也为O(n)，但是只需要O(1)的空间复杂度。而暴力法则需要O(m)的空间复杂度，其中m为数组中不一样的数的个数。

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int current = nums[0];
        int count = 1;
        for(int i = 1; i < nums.size(); ++i)
        {
            if(nums[i] == current)
                ++count;
            else
                -- count;
            if(count < 0)
            {
                current = nums[i];
                count = 0;
            }
        }
        return current;
    }
};
```

