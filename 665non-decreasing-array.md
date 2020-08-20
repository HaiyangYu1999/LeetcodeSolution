Given an array `nums` with `n` integers, your task is to check if it could become non-decreasing by modifying **at most** `1` element.

We define an array is non-decreasing if `nums[i] <= nums[i + 1]` holds for every `i` (0-based) such that `(0 <= i <= n - 2)`.

 

**Example 1:**

```
Input: nums = [4,2,3]
Output: true
Explanation: You could modify the first 4 to 1 to get a non-decreasing array.
```

**Example 2:**

```
Input: nums = [4,2,1]
Output: false
Explanation: You can't get a non-decreasing array by modify at most one element.
```

 

**Constraints:**

- `1 <= n <= 10 ^ 4`
- `- 10 ^ 5 <= nums[i] <= 10 ^ 5`

## 分类讨论

首先找到数组中第一个递减元素的索引first_descending_index = the first i that enable `nums[i] < nums[i-1]`.  i from 1 to nums.size() - 1

+ 如果first_descending_index == nums.size(), 那么这个数组本来就是递增的. 返回true
+ 如果first_descending_index == nums.size() - 1, 那么这个数组除了最后一个元素之外是递增的, 将最后一个元素置为INT_MAX也能满足要求. 返回true

下面为简洁起见将first_descending_index记为fdi. fdi必定在数组内部(不是第一个和最后一个位置). 考虑fdi相邻的两个元素, 并且已知`nums[fdi] < nums[fdi - 1]`

+ 若 `nums[fdi + 1] < nums[fdi]`, 则有`nums[fdi + 1] < nums[fdi] < nums[fdi - 1]`. 例如[4, 3, 2] 此时无论如何也不能做到修改一个元素后将数组变为ascending的. 返回false
+ 若`nums[fdi + 1] > nums[fdi - 1]`, 则有`nums[fdi] < nums[fdi - 1] < nums[fdi + 1]`.  例如[4,3,7] 这时将nums[fdi]修改为`nums[fdi] = nums[fdi - 1]`, 即可使得在fdi的邻域保持ascending. 但我们还不知道fdi后面的元素是不是为递增的(如果不是, 就至少要修改2次). 所以要判断修改之后的数组是不是递增的. 如果不是, 说明1次的修改是不能将数组变为ascending的. 返回false
+ 若`nums[fdi + 1] >= nums[fdi] && nums[fdi + 1] <= nums[fdi - 1]`, 即nums[fdi] < nums[fdi + 1] < nums[fdi - 1]. 例如[6, 2, 3]. **此时只能修改nums[fdi - 1]`nums[fdi - 1] = nums[fdi]`. (修改nums[fdi+1]或nums[fdi]都不能使这三个元素变为ascending.  [6,2,3]中只能修改6)**. 但是这样会造成前面已经ascending的元素disordered. 在[1,6,2,3]中将4修改为2是可行的. 但是在[4,6,2,3]中将6修改为2是不可行的. 所以还是要判断修改之后的元素是不是递增的.如果不是,返回false

```c++
class Solution {
public:
    bool checkPossibility(vector<int>& nums) {
        if(nums.size() == 0 || nums.size() == 1)
            return true;
        int i;
        for(i = 1; i < nums.size(); ++i)
        {
            if(nums[i] < nums[i-1])
                break;
        }
        const int first_descending_index = i;
        if(first_descending_index == nums.size() || first_descending_index == nums.size() - 1)
            return true;
        const int& fdi = first_descending_index;
        
        if(nums[fdi + 1] < nums[fdi])
        {
            return false;
        }
        else if(nums[fdi + 1] > nums[fdi - 1])
        {
            nums[fdi] = nums[fdi - 1];
            return isAscending(nums);
        }
        else if(nums[fdi + 1] >= nums[fdi] && nums[fdi + 1] <= nums[fdi - 1])
        {
            nums[fdi - 1] = nums[fdi];
            return isAscending(nums);
        }
        else
        {
            throw string("In no case you could enter this clause; some logical errors must occur before!");
        }
    }
private:
    bool isAscending(const vector<int>& nums)
    {
        if(nums.empty())
            return true;
        for(int i = 1; i < nums.size(); ++i)
        {
            if(nums[i] < nums[i-1])
                return false;
        }
        return true;
    }
};
```

