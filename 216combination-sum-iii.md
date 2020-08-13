Find all possible combinations of ***k*** numbers that add up to a number ***n***, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.

**Note:**

- All numbers will be positive integers.
- The solution set must not contain duplicate combinations.

**Example 1:**

```
Input: k = 3, n = 7
Output: [[1,2,4]]
```

**Example 2:**

```
Input: k = 3, n = 9
Output: [[1,2,6], [1,3,5], [2,3,4]]
```

## dfs

beginNumber从1到9依次判断

+ 若含有beginNumber, 则需寻找所有的长度为k-1, 和为n-beginNumber的vector, 再从所有满足条件的vector上push_back(gebinNumber).
+  若不含有beginNumber, 则需要寻找所有长度为k, 和为n的vector

两个vector取并集即为所得

```c++
class Solution {
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        return combinationSum3(1,k,n);
    }
private:
    vector<vector<int>> combinationSum3(int beginNumber, int k, int n)
    {
        if(k == 0 && n == 0)
            return vector<vector<int>>({vector<int>()});
        if(n < 0 || beginNumber > 9)
            return vector<vector<int>>();
        vector<vector<int>> containsBeginNumber = combinationSum3(beginNumber + 1, k - 1, n - beginNumber);
        for(auto& i : containsBeginNumber)
        {
            i.push_back(beginNumber);
        }
        vector<vector<int>> notContainsBeginNumber = combinationSum3(beginNumber + 1, k, n);
        containsBeginNumber.insert(containsBeginNumber.begin(),notContainsBeginNumber.begin(), notContainsBeginNumber.end());
        return containsBeginNumber;
    }
};
```

## dfs2

贴一个我觉得更清晰的代码

```c++
void dfs(int k, int sum, int idx, vector<int>& cur, vector<vector<int>>& ans)
{
	if (k == 0 && sum == 0) {
		ans.push_back(cur);
		return;
	}
	if (k <= 0 || sum <= 0) {
		return;
	}
	
	if (idx > 9) {
		return;
	}
	
	cur.push_back(idx);
	dfs(k - 1, sum - idx, idx + 1, cur, ans); // 选择1
	cur.pop_back();

	dfs(k, sum, idx + 1, cur, ans); // 不选择1
}

vector<vector<int>> combinationSum3(int k, int n) {
	vector<vector<int>> ans;
	vector<int> cur;
	dfs(k, n, 1, cur, ans);
	
	return ans;
}

作者：62E64ArP6T
链接：https://leetcode-cn.com/problems/combination-sum-iii/solution/2chong-di-gui-hui-su-de-bi-jiao-by-62e64arp6t/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



