Given a set of *non-overlapping* intervals, insert a new interval into the intervals (merge if necessary).

You may assume that the intervals were initially sorted according to their start times.

**Example 1:**

```
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
```

**Example 2:**

```
Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
```

**NOTE:** input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

## 1 懒人做法

直接参考leetcode 56 的做法, pushback新的区间后直接调用leetcode56的merge方法.

但是复杂度高, 要排序 nlogn

```c++
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        intervals.push_back(newInterval);
        return merge(intervals);
    }
private:
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
    static bool cmp(const vector<int>& intervalA, const vector<int>& intervalB)
    {
        if(intervalA.front() != intervalB.front())
            return intervalA.front() < intervalB.front();
        else
            return intervalA.back() < intervalB.back();
    }
};
```

## 2 扫描

因为intervals已经是按照左端点排序的了, 所以从左到右找到newInterval的左端点在的区间intervals[left_index]和右端点在的区间intervals[right_index]. 然后合并即可

这里对于左右端点设置两个变量, 所在的区间i 和是否在区间外面（左边的外面）

例如intervals = [[1,3],[6,9]], newInterval = [2,5]，左端点2所在的区间为第0个区间，并且不是在区间0外面. 右端点5所在的区间为第1个区间，并且在区间1外面

对于index小于left_index的区间和index大于right_index的区间， 直接放到结果中

left_index到right_index之间的区间则要分类讨论(还要注意left_index和right_index是否越界)

若right不在区间中, 则要添加两个区间[ min(intervals[left_index].front(), left), right ] 和 intervals[right_index]. 反之， 只添加一个区间 [ min(intervals[left_index].front(), left), right ]

> 例如intervals = [[1,3],[6,9]], newInterval = [2,5] 要添加 区间 [1,5] 和 [6,9]
>
> 例如intervals = [[1,3],[6,9]], newInterval = [2,7] 要添加 区间[1,7] 
>
> 因为 无论 newInterval.back()是5还是7, 都归类到第1个区间[6,9], 但是一个在第1个区间左边,一个在第1个区间中间





```c++
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        vector<vector<int>> vvc;
        if(intervals.empty())
        {
            vvc.push_back(newInterval);
            return vvc;
        }
        int left = newInterval.front();
        int right = newInterval.back();
        bool isLeftBeyondInterval = true;
        bool isRightBeyondInterval = true;
        int left_index = intervals.size();
        int right_index = intervals.size();
        for(int i = 0; i < intervals.size(); ++i)
        {
            if(left < intervals[i].front())
            {
                isLeftBeyondInterval = true;
                left_index = i;
                break;
            }
            else if(left <= intervals[i].back())
            {
                isLeftBeyondInterval = false;
                left_index = i;
                break;
            }
        }
        for(int i = 0; i < intervals.size(); ++i)
        {
            if(right < intervals[i].front())
            {
                isRightBeyondInterval = true;
                right_index = i;
                break;
            }
            else if(right <= intervals[i].back())
            {
                isRightBeyondInterval = false;
                right_index = i;
                break;
            }
        }
        
        for(int i = 0; i < intervals.size(); ++i)
        {
            if(i < left_index)
            {
                vvc.push_back(intervals[i]);
            }
            
        }
        if(isRightBeyondInterval)
        {
            if(left_index < intervals.size())
                vvc.push_back({min(intervals[left_index].front(), left), right});
            if(right_index < intervals.size())
                vvc.push_back(intervals[right_index]);
            if(left_index == intervals.size())
                vvc.push_back({left, right});
        }
        else
        {
            vvc.push_back({min(intervals[left_index].front(), left), intervals[right_index].back()});
        }
        for(int i = 0; i < intervals.size(); ++i)
        {
            if(i > right_index)
            {
                vvc.push_back(intervals[i]);
            }
            
        }
        return vvc;
    }
};
```

