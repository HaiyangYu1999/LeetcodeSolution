Given a collection of intervals, merge all overlapping intervals.

**Example 1:**

```
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
```

**Example 2:**

```
Input: [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

**NOTE:** input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

## 排序

懒得写题解了, 思路copy from [https://leetcode-cn.com/problems/merge-intervals/solution/he-bing-qu-jian-by-leetcode-solution/](https://leetcode-cn.com/problems/merge-intervals/solution/he-bing-qu-jian-by-leetcode-solution/)

如果我们按照区间的左端点排序，那么在排完序的列表中，可以合并的区间一定是连续的。

我们用数组 merged 存储最终的答案。

首先，我们将列表中的区间按照左端点升序排序。然后我们将第一个区间加入 merged 数组中，并按顺序依次考虑之后的每个区间：

如果当前区间的左端点在数组 merged 中最后一个区间的右端点之后，那么它们不会重合，我们可以直接将这个区间加入数组 merged 的末尾；

否则，它们重合，我们需要用当前区间的右端点更新数组 merged 中最后一个区间的右端点，将其置为二者的较大值。

代码中用vvc代替merged

```c++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), cmp);
        vector<vector<int>> vvc;
        if(intervals.empty())
            return vvc;
        int left = intervals.front().front();
        int right = intervals.front().back();
        for(int i = 1; i < intervals.size(); ++i)
        {
            auto tmp = intervals[i];
            if(tmp.front() <= right)  // able to merge
            {
                right = max(right, tmp.back());
            }
            else                    // cannot merge
            {
                vvc.push_back(vector<int>({left, right}));
                left = tmp.front();
                right = tmp.back();
            }
        }
        vvc.push_back({left,right});
        return vvc;
    }
private:
    static bool cmp(const vector<int>& intervalA, const vector<int>& intervalB)
    {
        if(intervalA.front() != intervalB.front())
            return intervalA.front() < intervalB.front();
        else
            return intervalA.back() < intervalB.back();
    }
};
```

