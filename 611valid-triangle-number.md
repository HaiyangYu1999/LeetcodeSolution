Given an array consists of non-negative integers, your task is to count the number of triplets chosen from the array that can make triangles if we take them as side lengths of a triangle.

**Example 1:**

```
Input: [2,2,3,4]
Output: 3
Explanation:
Valid combinations are: 
2,3,4 (using the first 2)
2,3,4 (using the second 2)
2,2,3
```



**Note:**

1. The length of the given array won't exceed 1000.
2. The integers in the given array are in the range of [0, 1000].

## 1 二分查找

先排序, 确定任意前2个边, 再统计第3个边的可能的个数. 最后加起来

O(n^2logn)复杂度. 确定前两个边n^2, 找第三个边的数量logn

```c++
class Solution {
public:
    int triangleNumber(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        int ans = 0;
        for(int i = 0; i < nums.size(); ++i)
        {
            for(int j = i + 1; j < nums.size(); ++j)
            {
                if(j == nums.size() - 1)
                    continue;
                vector<int>::iterator it = lower_bound(nums.begin() + j + 1, nums.end(), nums[i] + nums[j]);
                int k = it - nums.begin() - 1;
                int count = k - j;
                ans += count;
            }
        }
        return ans;
    }
};
```

## 2 改进的查找

观察1, 发现每次确定一个i,j后, 都要从[j+1, end)中查找最后一个满足nums[i] + nums[j] < nums[k]的k

但是, 随着j的迭代, 上面的范围可以逐渐缩小.

假设某一次找到了相应的k, 下一次j=j+1的迭代时, 新的k肯定是在原来的k的后面. 利用这一点可以复杂度变为n^2.(对于每一个固定的i, j和k都会以**不后退**的方式前进到end, 所以固定i后每一次复杂度为O(n))

同时还要剔除nums中的0, 因为含有0的三个边不可能组成三角形

```c++
class Solution {
public:
    int triangleNumber(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        int ans = 0;
        for(int i = 0; i < nums.size(); ++i)
        {
            if(nums[i] == 0)
                continue;
            int k = i + 2;
            for(int j = i + 1; j < nums.size(); ++j)
            {
                while(k != nums.size() && nums[i] + nums[j] > nums[k])
                    ++k;
                int count = k - j - 1;
                ans += count;
            }
        }
        return ans;
    }
};
```

