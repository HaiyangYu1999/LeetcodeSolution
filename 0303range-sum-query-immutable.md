Given an integer array `nums`, find the sum of the elements between indices `i` and `j` `(i ≤ j)`, inclusive.

Implement the `NumArray` class:

- `NumArray(int[] nums)` Initializes the object with the integer array `nums`.
- `int sumRange(int i, int j)` Return the sum of the elements of the `nums` array in the range `[i, j]` inclusive (i.e., `sum(nums[i], nums[i + 1], ... , nums[j])`)

 

**Example 1:**

```
Input
["NumArray", "sumRange", "sumRange", "sumRange"]
[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
Output
[null, 1, -1, -3]

Explanation
NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
numArray.sumRange(0, 2); // return 1 ((-2) + 0 + 3)
numArray.sumRange(2, 5); // return -1 (3 + (-5) + 2 + (-1)) 
numArray.sumRange(0, 5); // return -3 ((-2) + 0 + 3 + (-5) + 2 + (-1))
```

 

**Constraints:**

- `0 <= nums.length <= 104`
- `-105 <= nums[i] <= 105`
- `0 <= i <= j < nums.length`
- At most `104` calls will be made to `sumRange`.

## 动态规划

储存一个数组`accumulateSums`, 保存从0到i的和. 计算的时候通过`accumulateSums`来返回结果

```java
class NumArray {
    int[] accumulateSums;
    public NumArray(int[] nums) {
        accumulateSums = new int[nums.length];
        int sum = 0;
        for(int i = 0; i != nums.length; ++i)
        {
            sum += nums[i];
            accumulateSums[i] = sum;
        }
    }
    
    public int sumRange(int i, int j) {
        if(i > j)
            throw new RuntimeException("indexException");
        else
        {
            if(i == 0)
                return accumulateSums[j];
            else
            {
                return accumulateSums[j] - accumulateSums[i - 1];
            }
        }
        
    }
}

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * int param_1 = obj.sumRange(i,j);
 */
```

