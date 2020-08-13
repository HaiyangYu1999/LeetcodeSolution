Given an integer array, you need to find one **continuous subarray** that if you only sort this subarray in ascending order, then the whole array will be sorted in ascending order, too.

You need to find the **shortest** such subarray and output its length.

**Example 1:**

```
Input: [2, 6, 4, 8, 10, 9, 15]
Output: 5
Explanation: You need to sort [6, 4, 8, 10, 9] in ascending order to make the whole array sorted in ascending order.
```



**Note:**

1. Then length of the input array is in range [1, 10,000].
2. The input array may contain duplicates, so ascending order here means **<=**.

## 1 憨批解法

先构造个排好序的数组sortedNums, 再从左到右对照nums和sortNums第一个不一样的元素nums[i]和最后一个不一样的元素nums[j],所以(j - i + 1)即为所求。为了防止nums已经是排好序的导致的i>j，所以 return max(j - i + 1, 0)

**这算法太憨了😂😂😂, 既然已经都花时间和空间排好序了那还求排序需要的最短子数组干什么呢?**

```c++
class Solution {
public:
    int findUnsortedSubarray(vector<int>& nums) {
        vector<int> sortedNums = nums;
        sort(sortedNums.begin(), sortedNums.end());
        int i = 0;
        int j = nums.size() - 1;
        while(i < nums.size())
        {
            if(nums[i] != sortedNums[i])
                break;
            ++i;
        }
        while(j > -1)
        {
            if(nums[j] != sortedNums[j])
                break;
            --j;
        }
        return max(j - i + 1, 0);
    }
};
```

## 2 4次扫描

1. 首先要确定数组中从左到右第一个不是升序的元素和最后一个不是升序的元素, 即第一个使`nums[i] < nums[i - 1]`成立的i first_desc, 和最后一个使`nums[i] < nums[i - 1]`成立的i last_desc. (若找不到的话, 那就说明这个数组本来就是升序的, 返回0)那么在范围[first_desc, last_desc - 1]范围内的元素肯定不是升序排列. 需要移动的范围**至少**是这个区间

> e.g. [0 2 3 4 **1** 6 7 **5** 8]           至少需要移动元素1到5所在的区间, 注意是至少, 因为前面的2 3 4元素虽然是升序但是位置也是不对的  
>
> ​                                                 (元素1要放到他们前面)

2. 当确定first_desc和last_desc后.需要找到first_desc及之后的最小元素min_after_first_desc和last_desc之前的最大元素max_before_last_desc.  **first_desc及之后的最小元素min_after_first_desc的正确位置肯定在[0, first_desc]之间. last_desc之前的最大元素max_before_last_desc的正确位置肯定在[last_desc, nums.size() - 1]之间!!** 因为min_after_first_desc是肯定要小于nums[first_desc]的, 排序的话肯定要排到first_desc前面的某个位置loc1, 这个loc1就是我们最终寻找的index.  max_before_last_desc同理.

3. 找到min_after_first_desc的正确位置loc1, 即**从左到右**遍历0到first_desc - 1的第一个大于min_after_first_desc的元素位置, 找到max_before_last_desc的正确位置loc2, 即**从右到左**遍历nums.size() - 1到last_desc第一个小于max_before_last_desc的元素位置, loc2 - loc1 + 1即为所求.

```c++
class Solution {
public:
    int findUnsortedSubarray(vector<int>& nums) {
        int first_desc = 0;
        int last_desc = 0;
        for(int i = 1; i < nums.size(); ++i)
        {
            if(nums[i] < nums[i - 1])
            {
                first_desc = i;
                break;
            }
        }
        for(int i = nums.size() - 1; i > 0; --i)
        {
            if(nums[i] < nums[i - 1])
            {
                last_desc = i;
                break;
            }
        }
        if(first_desc == 0 && last_desc == 0) return 0;  //ascending array
        int min_after_first_desc = *min_element(nums.begin() + first_desc, nums.end());
        int max_before_last_desc = *max_element(nums.begin(), nums.begin() + last_desc);
        int low = 0;
        int high = nums.size() - 1;
        for(int i = 0; i <= first_desc; ++i)
        {
            if(nums[i] > min_after_first_desc)
            {
                low = i;
                break;
            }
        }
        for(int i = nums.size() - 1; i >= last_desc; --i)
        {
            if(nums[i] < max_before_last_desc)
            {
                high = i;
                break;
            }
        }
        return high - low + 1;
    }
};
```

