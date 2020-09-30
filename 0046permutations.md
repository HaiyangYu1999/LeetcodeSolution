Given a collection of **distinct** integers, return all possible permutations.

**Example:**

```
Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

## DFS

DFS即可. 使用一个boolean数组来标记nums[i]是否已经出现在list里面了

```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
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

