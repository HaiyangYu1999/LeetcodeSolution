Given a collection of numbers that might contain duplicates, return all possible unique permutations.

**Example:**

```
Input: [1,1,2]
Output:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```

## 1 暴力

明显可以当成leetcode 46题来做, 只不过这里要用`Set<List<Integer>>`.去重. 但是太慢了.

```java
class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        Set<List<Integer>> ans = new HashSet<>();
        boolean[] isTraversed = new boolean[nums.length];
        List<Integer> curr = new ArrayList<>();
        permute(ans, curr, nums, isTraversed);
        return new ArrayList<>(ans);
    }
    private void permute(Set<List<Integer>> ans, List<Integer> curr, int[] nums, boolean[] isTraversed)
    {
        if(curr.size() == nums.length)
        {
            ans.add(new ArrayList<Integer>(curr));
            return;
        }
        for(int i = 0; i < nums.length; ++i)
        {
            if(!isTraversed[i])
            {
                curr.add(nums[i]);
                isTraversed[i] = true;
                permute(ans, curr, nums, isTraversed);
                curr.remove(curr.size() - 1);
                isTraversed[i] = false;
            }
        }
    }
}
```

## 2 排序去重

首先先排序.

**当nums[i] == nums[i-1], 并且isTraverse[i-1] = false的时候, 直接跳过nums[i]即可. 否则一定会重!**

>  例如 nums = [1,1,2]
>
> 第一次遍历, 先选择了第1个1, 此时curr = [1], 这时, 第1个1的isTraverse为false, 可以将第2个1假如到curr中变成curr = [1, 1],随后可以计算出符合条件的 curr = [1,1,2], curr = [1,2,1], 他们被加入到结果ans中.
>
> 第二次遍历, 选择了第2个1, 但是有nums[1] == nums[0], 并且isTraverse[0] = false, 应该跳过这一次! 如果不跳过, 就有当前的curr存储了第2个1, curr = [1], 但是第1个1的isTraverse是false, 所以还会把第1个1加入到curr中, 变成curr = [1,1]. 和上面的重复了

```java
class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        Arrays.sort(nums);
        boolean[] isTraversed = new boolean[nums.length];
        List<Integer> curr = new ArrayList<>();
        List<List<Integer>> ans = new ArrayList<>();
        permute(ans, curr, nums, isTraversed);
        return ans;
     }
    private void permute(List<List<Integer>> ans, List<Integer> curr, int[] nums, boolean[] isTraversed)
    {
        if(curr.size() == nums.length)
        {
            ans.add(new ArrayList<Integer>(curr));
            return;
        }
        for(int i = 0; i < nums.length; ++i)
        {
            if(!isTraversed[i])
            {
                if(i > 0 && nums[i] == nums[i-1] && !isTraversed[i-1])
                    continue;
                curr.add(nums[i]);
                isTraversed[i] = true;
                permute(ans, curr, nums, isTraversed);
                curr.remove(curr.size() - 1);
                isTraversed[i] = false;
            }
        }
    }
}
```

