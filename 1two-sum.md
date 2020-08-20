Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have ***exactly\*** one solution, and you may not use the *same* element twice.

**Example:**

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

注意，最后要判断返回的下标不能相等

## 1. 暴力求解

```c++
class Solution {
public:
 vector<int> twoSum(vector<int>& nums, int target) 
 {
     int size = nums.size();
     for(int i = 0; i < size; ++i)
     {
         for(int j = i+1; j < size; ++j)
         {
             if(nums[i] + nums[j] == target)
             {
                 vector<int> res = {i,j};
                 return res;
             }
         }
     }
     return {-1, -1};
 }
};
```



嵌套for 寻找nums[i] + nums[j] == target 的{i, j}. 花费O(n^2)的时间复杂度

## 2. 利用hashtable降低时间复杂度

经分析 只需要对于每个i, 在nums中找到值为target - nums[i]的值对应的下标，可以先便利一遍vector构造hashtable，花费O(1)的复杂度查找. 总时间复杂度O(2n)

注意api的使用

insert() 接受pair对象，也可以为初始化列表{key, value}

find(); 接受key_type对象，返回查找结果的迭代器，若查找不到返回尾后迭代器

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) 
    {
        int size = nums.size();
        unordered_map<int,int> mp;
        for(int i = 0; i < size; ++i)
        {
            mp.insert({nums[i],i});
        }
        for(int i = 0; i < size; ++i)
        {
            unordered_map<int, int>::iterator it = mp.find(target - nums[i]);
            if(it != mp.end())
            {
                int j = it->second;
                if(i != j)
                {
                    return {i,j};
                }
            }
        }
        return {-1, -1};
    }
};
```

## 3. 一遍hashtable

将2 中的算法改进，在遍历的过程中构造hashtable，这样只需要遍历一遍，但是第i个元素只能和第1, ..., i-1个元素查找。返回的{i, j}一定有i > j。如果有顺序要求要改正返回值。时间复杂度O(n)

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) 
    {
        int size = nums.size();
        unordered_map<int,int> mp;
        for(int i = 0; i < size; ++i)
        {
            mp.insert({nums[i], i});
            unordered_map<int, int>::iterator it = mp.find(target - nums[i]);
            if(it != mp.end())
            {
                int j = it->second;
                if(i != j)
                {
                    return {i,j};
                }
            }
        }
        return {-1, -1};
    }
};
```

