Given an array, rotate the array to the right by *k* steps, where *k* is non-negative.

**Follow up:**

- Try to come up as many solutions as you can, there are at least 3 different ways to solve this problem.
- Could you do it in-place with O(1) extra space?

 

**Example 1:**

```
Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]
```

**Example 2:**

```
Input: nums = [-1,-100,3,99], k = 2
Output: [3,99,-1,-100]
Explanation: 
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]
```

 

**Constraints:**

- `1 <= nums.length <= 2 * 10^4`
- It's guaranteed that `nums[i]` fits in a 32 bit-signed integer.
- `k >= 0`

## 1 暴力法

以下均假设k < nums.size()，否则令 k %= nums.size()即可，因为对数组进行k = nums.size()次rotate数组保持不变

先备份后k个值, 再从前部将后k个值插入nums首部，再截取掉最后k个值，即为所求。但是这样需要O(2k)的空间复杂度

```c++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        if(k == 0) return;
        k = k % nums.size();
        int origin_size = nums.size();
        vector<int>::iterator shift_begin = nums.end() - k;
        vector<int> new_vc(shift_begin, nums.end());
        nums.insert(nums.begin(), new_vc.begin(), new_vc.end());
        nums.resize(origin_size);
    }
};
```

## 2 数组翻转

注意到这样一个事实，我们假设数组a的前nums.size() - k个元素组成的子数组为a1, 后k个元素组成的子数组a2，则将a1,a2翻转后变为数组[a1',a2']，再将这个数组翻转变为[a2',a1']这个新数组即为所求！

举例说明一下, nums.size() = 10, k = 4

则划分为两个子数组[0,1,2,3,4,5,**6,7,8,9**].(加粗的是一个不加粗的是另一个)

对这两个数组分别翻转，变为[5,4,3,2,1,0,**9,8,7,6**]

再对整体的数组翻转[**6,7,8,9**,0,1,2,3,4,5]就得到了正确答案.并且空间占用为O(1)。不得不感叹想到这个解法的人的聪明！

```c++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        if(k == 0) return;
        k = k % nums.size();
        reverse(nums,0,nums.size() - k - 1);
        reverse(nums,nums.size() - k, nums.size()-1);
        reverse(nums,0,nums.size() - 1);
    }
private:
    void reverse(vector<int>& nums, int begin, int end)
    {
        while(begin < end)
        {
            swap(nums[begin], nums[end]);
            ++begin;
            --end;
        }
    }
};
```

