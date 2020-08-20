Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.

 

**Example 1:**

```
Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

**Example 2:**

```
Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
```

 

**Constraints:**

- `1 <= nums.length <= 3 * 10^4`
- `0 <= nums[i][j] <= 10^5`

## 贪心

创建变量int maxDist = 0储存目前阶段能jump到的最远的距离 一开始最远只能到0

对于i从0到nums.size() - 1

若i > maxDist 说明无论如何也不可能到达i， 所以不可能到达last one 返回false

若i <= maxDist 说明可以到达i， 这时要更新maxDist, 看看在节点i的帮助下能不能跳更远， 在i开始跳的最远距离为i + nums[i] 并更新maxDist = max(maxDist, i + nums[i])

最后，若i完整的从0遍历到size - 1，说明可以到达，返回true

```c++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int maxDist = 0;
        for(int i = 0; i < nums.size(); ++i)
        {
            if(maxDist < i)
                return false;
            maxDist=max(maxDist, i + nums[i]);
        }
        return true;
    }
};
```

