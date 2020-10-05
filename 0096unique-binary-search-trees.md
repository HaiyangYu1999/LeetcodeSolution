Given *n*, how many structurally unique **BST's** (binary search trees) that store values 1 ... *n*?

**Example:**

```
Input: 3
Output: 5
Explanation:
Given n = 3, there are a total of 5 unique BST's:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

 

**Constraints:**

- `1 <= n <= 19`

## 动态规划

令f(i)为把1,2,3,...i放到BST中的所有不同方法数量.

假设要把1,2,3,4...,n这些数放到BST中, 可以从1到n中任意选一个点作为root. 不妨设选择i为root. 那么左子树肯定是1到i-1的这些值, 所以左子树有f(i-1)种可能性. 右子树的值肯定是从i+1到n的这些值, 这些值组成的BST子树的可能数量等于从1到n-i这些值组成的BST子树的可能数量. 所以, 当选择i为root时, 有f(i-1)*f(n-i)种可能. i从1遍历到n, 相加就能得到结果.

```java
class Solution {
    public int numTrees(int n) {
        if(n == 0) return 1;
        if(n == 1) return 1;
        if(n == 2) return 2;
        int[] dp = new int[n + 1];
        dp[0] = 1;
        dp[1] = 1;
        dp[2] = 2;
        for(int i = 3; i < n + 1; ++i)
        {
            for(int j = 0; j < i; ++j)
            {
                dp[i] += dp[j] * dp[i-j-1];
            }
        }
        return dp[n];
    }
    
}
```





