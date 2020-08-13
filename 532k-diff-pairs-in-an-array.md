Given an array of integers and an integer **k**, you need to find the number of **unique** k-diff pairs in the array. Here a **k-diff** pair is defined as an integer pair (i, j), where **i** and **j** are both numbers in the array and their [absolute difference](https://en.wikipedia.org/wiki/Absolute_difference) is **k**.

**Example 1:**

```
Input: [3, 1, 4, 1, 5], k = 2
Output: 2
Explanation: There are two 2-diff pairs in the array, (1, 3) and (3, 5).Although we have two 1s in the input, we should only return the number of unique pairs.
```



**Example 2:**

```
Input:[1, 2, 3, 4, 5], k = 1
Output: 4
Explanation: There are four 1-diff pairs in the array, (1, 2), (2, 3), (3, 4) and (4, 5).
```



**Example 3:**

```
Input: [1, 3, 1, 5, 4], k = 0
Output: 1
Explanation: There is one 0-diff pair in the array, (1, 1).
```



**Note:**

1. The pairs (i, j) and (j, i) count as the same pair.
2. The length of the array won't exceed 10,000.
3. All the integers in the given input belong to the range: [-1e7, 1e7].

## 分类讨论

当k = 0时, 问题等价于寻找数组中出现次数大于等于2次的元素个数, 建立一个hashmap统计出现次数即可.O(n)

当k < 0时, 我本以为k=-1和k=1的结果应该一样, 毕竟集合`{(i, j): k == i - j}`和集合`{(i, j): -k == i - j}`的元素个数相同,但是这个题的坑在于**k<0时, 应该返回结果是0, 而example并未说明!**

当k>0时, 同一个元素只需要考虑一次,不需要考虑这个元素出现了几次(出现1次和出现多次没有区别). 所以先进行**排序**和去重

设置双指针, i=0,j=1, tmp = nums[j] - nums[i], 

+ 若tmp == k,则说明已经找到一组解, 再令++j, ++i去寻找下一个可能的解

+ 若tmp<k 说明需要++j, 以获得更大的差值nums[j] - nums[i]
+ 若tmp>k 说明需要++i, 以获得更小的差值nums[j] - nums[i]

```c++
class Solution {
public:
    int findPairs(vector<int>& nums, int k) {
        if(nums.size() < 2 || k < 0)
            return 0;
        if(k == 0)
        {
            int res = 0;
            unordered_map<int, int> mp;
            for(int i :nums)
                ++mp[i];
            for(auto& i : mp)
            {
                if(i.second > 1)
                    ++res;
            }
            return res;
        }
        else
        {
            int res = 0;
            sort(nums.begin(), nums.end());
            vector<int>::iterator new_end = unique(nums.begin(), nums.end());
            nums.erase(new_end, nums.end());
            int i = 0; 
            int j = 1;
            while(j < nums.size())
            {
                int tmp = nums[j] - nums[i];
                if(tmp == k)
                {
                    ++res;
                    ++j;
                    ++i;
                }
                if(tmp < k)
                    ++j;
                if(tmp > k)
                    ++i;
            }
            return res;
        }
        return -1;
    }
};
```

