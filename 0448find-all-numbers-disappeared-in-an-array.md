Given an array of integers where 1 ≤ a[i] ≤ *n* (*n* = size of array), some elements appear twice and others appear once.

Find all the elements of [1, *n*] inclusive that do not appear in this array.

Could you do it without extra space and in O(*n*) runtime? You may assume the returned list does not count as extra space.

**Example:**

```
Input:
[4,3,2,7,8,2,3,1]

Output:
[5,6]
```

## 1 hashmap

是,我知道这个不符合常数空间复杂度的要求,但是菜有什么办法呢😭

```c++
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        unordered_map<int,int> mp;
        for(int i = 0; i < nums.size(); ++i)
        {
            ++mp[nums[i]];
        }
        vector<int> vc;
        for(int i = 1; i < nums.size() + 1; ++i)
        {
            if(mp[i] == 0)
                vc.push_back(i);
        }
        return vc;
    }
};
```

## 2 原地标记

本题的关键是**如何储存出现的元素的信息**,观察解法1hashmap方法发现额外构造了一个映射. 

但是因为这次出现的元素的范围在1-n之间, 所以可以利用数组本身储存出现信息. 元素1出现就在数组第0个元素上做个标记, ..., 元素n出现就在数组第n-1个元素上做个标记

**重点是如何做这个标记**, 直接标记为-1或0是不行的. 

> 例如, [4,3,2,7,8,2,3,1] 当读取第0个元素为4时, 假如在第3个元素上标记为-1或0, 这时如果继续向后读到第3个元素,发现第3个元素的7已经被覆盖了, 无法还原原来nums[3]的信息.

所以需要一个标记后也可以还原数组信息的方法. 这样的方法不止一种, 可以取负数, nums[i] = -abs(nums[i]), 当需要读到nums[i]的值的时候直接取绝对值即可还原出来原来nums[i]的值, 也可以在nums[i]上加上n, nums[i] += n, 需要nums原来的信息时候直接取模即可

下面假设使用的是取负数的方法. 

遍历nums所有元素标记数组后, 再遍历一遍检查是否存在没有被标记为负数的元素, 如果nums[i]仍然为正数, 那就说明数组中肯定没有i+1!!!

```c++
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        for(int i = 0; i < nums.size(); ++i)
        {
            int index = abs(nums[i]) - 1;
            nums[index] = - abs(nums[index]);
        }
        vector<int> vc;
        for(int i = 0; i < nums.size(); ++i)
        {
           if(nums[i] > 0)
               vc.push_back(i + 1);
        }
        return vc;
    }
};
```

