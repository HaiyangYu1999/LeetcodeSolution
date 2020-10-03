Given a **non-empty** array of integers, every element appears *twice* except for one. Find that single one.

**Note:**

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

**Example 1:**

```
Input: [2,2,1]
Output: 1
```

**Example 2:**

```
Input: [4,1,2,1,2]
Output: 4
```

## 异或

这题和leetcode 268一样. 利用一个数异或某个数两次, 等于没异或.

```java
class Solution {
    public int singleNumber(int[] nums) {
        int a = 0;
        for(int i : nums)
            a ^= i;
        return a;
    }
}
```

