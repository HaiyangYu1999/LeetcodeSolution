Given a **set** of candidate numbers (`candidates`) **(without duplicates)** and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sums to `target`.

The **same** repeated number may be chosen from `candidates` unlimited number of times.

**Note:**

- All numbers (including `target`) will be positive integers.
- The solution set must not contain duplicate combinations.

**Example 1:**

```
Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]
```

**Example 2:**

```
Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

 

**Constraints:**

- `1 <= candidates.length <= 30`
- `1 <= candidates[i] <= 200`
- Each element of `candidate` is unique.
- `1 <= target <= 500`

## 1 动态规划1

给定candidates，设target对应的二维数组为arr(tar)

则有arr(tar) 等于对于每一个i，arr(tar - i)返回的二维数组每个数组中再append上i后的二维数组的并。

如果tar-i < 0，返回空的二维数组，如果tar-i == 0，则arr(tar-i)返回[[i]]，这是递归边界。

计算完了后对于每个vector排序，再对vectors排序，去重

这几次排序的复杂度很高。

```c++
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        auto vectors = getVectors(candidates, target);
        //remove the duplicate
        for(auto& vc : vectors)
        {
            sort(vc.begin(),vc.end());
        }
        sort(vectors.begin(),vectors.end());
        auto end = unique(vectors.begin(),vectors.end());
        return vector<vector<int>>(vectors.begin(), end);
    }
private:
    vector<vector<int>> getVectors(const vector<int>& candidates, int target)
    {
        vector<vector<int>> vectors;
        if(target <= 0)
            return vectors;
        for(const auto& i : candidates)
        {
            auto vectorsEqualTargetMinus_i = getVectors(candidates, target - i);
            if(target - i == 0)
            vectors.push_back(vector<int>({i}));
            for(auto& vc : vectorsEqualTargetMinus_i)
            {
                vc.push_back(i);
                vectors.push_back(vc);
            }
        }
        return vectors;
    }
};
```

## 2 动态规划2

对于1的改进，增加一个参数j，代表上一次选择的数的index，每次只挑选j和j后面的数作为可能的选择，这样出来的二维数组为已经排序好的，不需要排序去重。复杂度低

对于candidates = [2,3,6,7], target = 7, 这种方法不会产生[2,2,3] [2,3,2] [3,2,2]的多解情况

```c++
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        return getVectors(candidates, target, 0);
    }
private:
    vector<vector<int>> getVectors(const vector<int>& candidates, int target, int j)
    {
        vector<vector<int>> vectors;
        if(target <= 0)
            return vectors;
        for(int i = j; i < candidates.size(); ++i)
        {
            auto vectorsEqualTargetMinus_i = getVectors(candidates, target - candidates[i],i);
            if(target - candidates[i] == 0)
            	vectors.push_back(vector<int>({candidates[i]}));
            for(auto& vc : vectorsEqualTargetMinus_i)
            {
                vc.push_back(candidates[i]);
                vectors.push_back(vc);
            }
        }
        return vectors;
    }
};
```

