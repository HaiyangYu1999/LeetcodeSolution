Given an integer array of size *n*, find all elements that appear more than `⌊ n/3 ⌋` times.

**Note:** The algorithm should run in linear time and in O(1) space.

**Example 1:**

```
Input: [3,2,3]
Output: [3]
```

**Example 2:**

```
Input: [1,1,1,3,3,2,2,2]
Output: [1,2]
```

## 1 错误的摩尔投票

因为超过n/3的值只能是1个或2个. 

若**已经找到一个超过n/3的值A**, 可以先"剔除"这个值, 然后在剩下的值里面通过摩尔投票 (摩尔投票参考leetcode169)寻找超过nn/2的值B, (其中nn为n - count(A); )

若找到的B的个数大于n/3, 那么返回[A, B]

找到的B的个数小于n/3, 那么返回[A]

```c++
for(int i = 0; i < nums.size(); ++i)  //相当于"剔除"了A
{   
	if(nums[i] == A)
		continue;
    doSometingElse();
}
```

**但关键是如何找到一个超过n/3的值A, 我的所有尝试都失败了**. 本来想在摩尔投票上做一个修改找超过n/3的元素, 即一个元素通过2个不同的元素消去,

```c++
int x = nums.front();
int count_x = 2;
for(int i = 1; i < nums.size(); ++i)
{
    if(nums[i] == x)
        count_x += 2;
    else
    {
        count_x -= 1;
    }
    if(count_x < 0)
    {
        x = nums[i];
        count_x = 2;
    }
}
```

**但是这样是不行的**, 反例[1,3,3,4].

所以只能用hashmap了 (再次流下了菜的泪水😭😭😭)

```c++
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {
        vector<int> vc;
        unordered_map<int, int> mp;
        for(int i : nums)
            ++mp[i];
        for(const auto& i : mp)
            if(i.second > nums.size() / 3)
                vc.push_back(i.first);
        return vc;
    }
};
```

## 2 正确的摩尔投票

思路是, 对于三个互不同的值, 三三消去. 最后剩下的, 就是超过n/3的元素

思路很简单, 但是代码上有细节需要注意.

首先设置两个candidate和count记录出现的值和个数

```c++
int candidate1 = nums[0];
int candidate2 = nums[0];
int count1 = 0;
int count2 = 0;
```

+ 若nums[i] == candidate1 或nums[i] == candidate2, 则相应的count++. **然后conitune**
+ 若count1 == 0,  要及时地更换相应的candidate1并设置count为1, candidaite2不变, 若count2 == 0,  要及时地更换相应的candidate2并设置count为1, candidaite1不变. 这样就保证了只要有一个count是0, 那么另外一个count不会受三者不相等的影响导致count--**. 这一点很重要, 之前写n/2的摩尔投票的时候都是nums[i]和candidiate不相等的时候count直接减1, 然后检查count是否小于0, 如果小于0, 再更新candidate和count. 但是这个例子中不行! 反例[1 1 1 3 3 2 2 2], 如果先判断不相等都减1的话, 前面的3个1会被第3个元素3抵消1个, 抵消后的1就达不到n/3的标准了**
+ 最后检查两个candidate是否真的大于n/3.

```c++
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {
        vector<int> vc;
        if(nums.size() < 2)
            return nums;    //nums:[] => [], nums:[a] => [a]
        int candidate1 = nums[0];
        int candidate2 = nums[0];
        int count1 = 0;
        int count2 = 0;
        for(int i = 0; i < nums.size(); ++i)
        {
            if(nums[i] == candidate1)
                ++count1;
            else if(nums[i] == candidate2)
                ++count2;
            else if(count1 == 0)
            {
                candidate1 = nums[i];
                count1 = 1;
            }
            else if(count2 == 0)
            {
                candidate2 = nums[i];
                count2 = 1;
            }
            else
            {
                --count1;
                --count2;
            }
        }
        if(count(nums.begin(), nums.end(), candidate1) > nums.size() / 3)
            vc.push_back(candidate1);
        if(candidate2 != candidate1 && count(nums.begin(), nums.end(), candidate2) > nums.size() / 3)
            vc.push_back(candidate2);
        return vc;
    }
};
```

