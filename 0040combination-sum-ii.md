Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sums to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:**

- All numbers (including `target`) will be positive integers.
- The solution set must not contain duplicate combinations.

**Example 1:**

```
Input: candidates = [10,1,2,7,6,1,5], target = 8,
A solution set is:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```

**Example 2:**

```
Input: candidates = [2,5,2,1,2], target = 5,
A solution set is:
[
  [1,2,2],
  [5]
]
```

## 1 动态规划1

和leetcode39一样，只不过这一次对于最近加上的元素位于index j，后续的寻找要从j+1开始，这样就能避免一个index使用多次。

但是，这个方法只能保证相同下标的index不能使用多次，对于不同下标的相同值，仍需要排序去重。

example 1 中就有可能出现[1, 7]和[7, 1]两种解，因为candidates里有2个1

```c++
class Solution {
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        auto vectors = getVectorsWithoutDuplication(candidates,target,-1);
        for(auto& i : vectors)
            sort(i.begin(), i.end());
        sort(vectors.begin(), vectors.end());
        auto end = unique(vectors.begin(), vectors.end());
        return vector<vector<int>>(vectors.begin(), end);
    }
    vector<vector<int>> getVectorsWithoutDuplication(const vector<int>& candidates, int target,int begin_index)
    {
        vector<vector<int>> vectors;
        if(target <= 0)
        {
            return vectors;
        }
        for(int i = begin_index + 1; i < candidates.size(); ++i)
        {
            auto vectorsEqualTarget_i = getVectorsWithoutDuplication(candidates, target - candidates[i], i);
            if(target - candidates[i] == 0)
                vectors.push_back(vector<int>({candidates[i]}));
            for(auto& vc : vectorsEqualTarget_i)
            {
                vc.push_back(candidates[i]);
                vectors.push_back(vc);
            }
        }
        return vectors;
    }
};
```

## 2 动态规划2

动态规划1重复的根本原因是因为，如果有两个index i > j，并且candidates[i] == candidates[j]，所以所有的和为target-candidates[i]的集合与所有和为target-candidates[j]的集合相等。那么getVectorsWithoutDuplication(candidates, target - candidates[i], i) 一定是 getVectorsWithoutDuplication(candidates, target - candidates[j], j) 的子集！

这时只需要不计算j产生的所有和为target-candidates[j]的数组就可以了(candidates[i] == candidates[i - 1])。

但是,要注意条件i > begin_index + 1 ,如果没有这个条件，会落下{1,1,6,7} target=8中{1,1,6}的解

```c++
class Solution {

public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(),candidates.end());
        auto vectors = getVectorsWithoutDuplication(candidates,target,-1);

        return vectors;
    }
    vector<vector<int>> getVectorsWithoutDuplication(const vector<int>& candidates, int target,int begin_index)
    {
        vector<vector<int>> vectors;
        if(target <= 0)
        {
            return vectors;
        }
        for(int i = begin_index + 1; i < candidates.size(); ++i)
        {
            if(i > begin_index + 1 && candidates[i] == candidates[i - 1]) continue;
            auto vectorsEqualTarget_i = getVectorsWithoutDuplication(candidates, target - candidates[i], i);
            if(target - candidates[i] == 0)
                vectors.push_back(vector<int>({candidates[i]}));
            for(auto& vc : vectorsEqualTarget_i)
            {
                vc.push_back(candidates[i]);
                vectors.push_back(vc);
            }
        }
        return vectors;
    }
};
```

