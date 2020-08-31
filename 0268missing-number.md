Given an array containing *n* distinct numbers taken from `0, 1, 2, ..., n`, find the one that is missing from the array.

**Example 1:**

```
Input: [3,0,1]
Output: 2
```

**Example 2:**

```
Input: [9,6,4,2,3,5,7,0,1]
Output: 8
```

**Note**:
Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?

## 1 暴力法

使用一个hash set储存数组的所有元素，再查找从0到n哪个元素没有在set中，这样做时间和空间复杂度为O(n)

```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        unordered_set<int> s(nums.begin(), nums.end());
        for(int i = 0; i < nums.size() + 1; ++i)
        {
            bool notFound = (s.find(i) == s.end());
            if(notFound)
                return i;
        }
        return -1;
    }
};
```

## 2 求sum

假如一个都不缺的话，那么这个数组的和一定为n*(n+1)/2,

先计算这个数组的和sum，那么n*(n+1)/2 - sum一定为缺少的元素！

```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n = nums.size();
        int sum = accumulate(nums.begin(),nums.end(),0);
        return n * (n + 1) /2 - sum;
    }
};
```

## 3 异或

先考虑异或的性质  a\^b=b\^a  (a\^b)\^c = a\^(b^c) a\^b\^b = a

即满足交换律和结合律，并且**一个数异或两次相同的数等于不异或**

可以利用这一点，找到缺失的那个值。

对于变量n = 0，n先异或从0到n的所有值，再异或数组中的所有值

假如0到n的任意一个数，在数组中出现过，那么根据交换律和异或两次相同的数等于不异或

那么这个数就相当于被消去了，只有数组中不存在的那个数只被异或了一次。

消去后只剩n = 0 ^ j = j 其中j是数组中消失的数，返回n即可

```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n = 0;
        for(int i = 0; i <= nums.size(); ++i)
            n ^= i;
        for(int i = 0; i < nums.size(); ++i)
            n ^= nums[i];
        return n;
    }
};
```

