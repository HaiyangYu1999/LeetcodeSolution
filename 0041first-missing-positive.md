Given an unsorted integer array, find the smallest missing positive integer.

**Example 1:**

```
Input: [1,2,0]
Output: 3
```

**Example 2:**

```
Input: [3,4,-1,1]
Output: 2
```

**Example 3:**

```
Input: [7,8,9,11,12]
Output: 1
```

**Follow up:**

Your algorithm should run in *O*(*n*) time and uses constant extra space.

## 1 原地修改数组

首先, 利用双指针遍历一遍数组, 把大于0的元素放在数组左边, 小于等于0的放在数组右边.

所以我们只需要考虑左边大于0的数组, 范围记为[0, right]

然后, 在0到right这个子数组中原地标记, 如果发现了元素i, 就把nums[i-1]置为负, 如果元素超过了right + 1, 就跳过. (因为数组中一共只有right + 1个正数, 所以第一个缺失的肯定在0到right + 1中间!)

然后, 从0开始寻找没有被标记为负数的值, 如果nums[i]没有标记为负数, 说明数组中没有出现过元素i+1, 即找到了第一个消失的正数i+1. 如果从0到right所有的nums[i]都被标记了负数, 说明1到right + 1这些元素都有, 下一个缺失的元素自然是right + 2.

```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        while(left <= right)
        {
            while(left < nums.length && nums[left] > 0)
                ++left;
            while(right > -1 && nums[right] <= 0)
                --right;
            if(left > right)
                break;
            else
            {
                int tmp = nums[right];
                nums[right] = nums[left];
                nums[left] = tmp;
            }
        }
        
        for(int i = 0; i <= right; ++i)
        {
            int val = Math.abs(nums[i]);
            if(val <= right + 1)
                nums[val-1] = -Math.abs(nums[val-1]);
        }
        for(int i = 0; i <= right; ++i)
        {
            if(nums[i] > 0)
                return i + 1;
        }
        return right + 2;
    }
}
```

## 2 原地修改数组

对于上面的方法1, 我们发现根本不用把大于0的元素和小于等于0的元素左右分类. 只需要把所有小于等于0的元素都置为n+1, 然后对整个数组原地修改即可. 这种方法复杂度不变, 但更简洁.

```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        final int n = nums.length;
        for(int i = 0; i < n; ++i)
        {
            if(nums[i] <= 0)
                nums[i] = n + 1;
        }
        
        for(int i = 0; i < n; ++i)
        {
            int val = Math.abs(nums[i]);
            if(val <= n)
                nums[val-1] = -Math.abs(nums[val-1]);
        }
        for(int i = 0; i < n; ++i)
        {
            if(nums[i] > 0)
                return i + 1;
        }
        return n + 1;
    }
}
```



