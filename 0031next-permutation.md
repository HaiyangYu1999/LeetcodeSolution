Implement **next permutation**, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be **[in-place](http://en.wikipedia.org/wiki/In-place_algorithm)** and use only constant extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.

```
1,2,3` → `1,3,2`
`3,2,1` → `1,2,3`
`1,1,5` → `1,5,1
```



## 寻找数组最后的几个递减的元素

最大的排列是满足降序
最小的排列是满足升序
因此寻找后一个排列的操作是:
    1. 从后遍历，寻找第一个升序的数，即 nums[k - 1] < nums[k]
    2. 在[k, end]中寻找比nums[k - 1]大的最小值
    3. 交换第二步找到的最小值和nums[k - 1]
    4. 将[k, end]中的内容reverse, 因为之前[k, end]中的内容是满足降序的，如果不	reverse不是紧挨着的下一个排列。

```c++
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        if(nums.empty()) return;
        int index = nums.size() - 1;
        while(index > 0 && nums[index] <= nums[index - 1])
        {
            --index;
        }
        bool isDescending = (index == 0);
        if(isDescending)
        {
            //use reverse iterator reverse the descending array
            reverse(nums.begin(), nums.end());
            return;
        }
        else
        {
            int first_descend_element = nums[index - 1];

            int begin = index;
            int end = nums.size() - 1;
            int upper = descUpperBound(nums,begin,end,first_descend_element);
            swap(nums[index - 1],nums[upper]);

            reverse(nums.begin()+begin, nums.end());
        }
    }
private:
    int descUpperBound(const vector<int>& nums,int i,int j,int target)
{
    while(i <= j)
    {
        int mid = i + (j - i) / 2;
        if(nums[mid] > target)
        {
            i = mid + 1;
        }
        else
        {
            j = mid - 1;
        }
    }
    return j;
}
};
```

