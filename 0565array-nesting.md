A zero-indexed array A of length N contains all integers from 0 to N-1. Find and return the longest length of set S, where S[i] = {A[i], A[A[i]], A[A[A[i]]], ... } subjected to the rule below.

Suppose the first element in S starts with the selection of element A[i] of index = i, the next element in S should be A[A[i]], and then A[A[A[i]]]… By that analogy, we stop adding right before a duplicate element occurs in S.

 

**Example 1:**

```
Input: A = [5,4,0,3,1,6,2]
Output: 4
Explanation: 
A[0] = 5, A[1] = 4, A[2] = 0, A[3] = 3, A[4] = 1, A[5] = 6, A[6] = 2.

One of the longest S[K]:
S[0] = {A[0], A[5], A[6], A[2]} = {5, 6, 2, 0}
```

 

**Note:**

1. N is an integer within the range [1, 20,000].
2. The elements of A are all distinct.
3. Each element of A is an integer within the range [0, N-1].

## 1 set + 原地标记

构造一个set储存被遍历过的值.

i从0到size - 1遍历, 当nums[i] 被标记过时直接跳过. 

当nums[i]未被标记过, 从nums[i]开始, 遵循顺序nums[i] -> nums[nums[i]] -> nums[nums[nums[i]]] -> ... 把每一个元素标记, 并放入set, 直到某一个值与set里面的值重复. 最后统计set的size并clear这个set. 取各个set中最大的size返回即可. 

时间复杂度O(n) 空间复杂度O(k) k是返回值

```c++
class Solution {
public:
    int arrayNesting(vector<int>& nums) {
        const int N = nums.size();
        unordered_set<int> s;
        int maxLen = 0;
        for(int i = 0; i < nums.size(); ++i)
        {
            if(!isMarked(nums[i]))
            {
                int tmp = nums[i];
                nums[i] = mark(nums[i], N);
                while(true)
                {
                    if(s.find(tmp) == s.end())
                    {
                        s.insert(tmp);
                        nums[tmp] = mark(nums[tmp], N);
                        tmp = undoMark(nums[tmp], N);
                    }
                    else
                    {
                        maxLen = max(maxLen, static_cast<int>(s.size()));
                        s.clear();
                        break;
                    }
                }
            }
            else
                continue;
        }
        return maxLen;
    }
private:
    int mark(int a, int N)
    {
        return (a < 0) ? a : a - N;
    }
    int undoMark(int a, int N)
    {
        return (a < 0) ? a + N : a;
    }
    int isMarked(int a)
    {
        return a < 0;
    }
};
```

## 2 原地标记

通过对1的观察, 发现不需要构造一个set来判断是否重复地计算一个元素. 只需要一个数count来记录这次遍历的环的大小即可. 对于每一个没有被标记的nums[i], 还是从nums[i]开始, 遵循顺序nums[i] -> nums[nums[i]] -> nums[nums[nums[i]]] -> ... 把每一个元素标记, 并且**++count**. 直到某个元素已经被标记了, 返回各个count的最大值, 并清空count. 

时间复杂度O(n) 空间复杂度O(1)

```c++
class Solution {
public:
    int arrayNesting(vector<int>& nums) {
        const int N = nums.size();
        int maxLen = 0;
        for(int i = 0; i < nums.size(); ++i)
        {
            if(!isMarked(nums[i]))
            {
                int maxLocal = 0;
                int tmp = i;
                while(true)
                {
                    if(!isMarked(nums[tmp]))
                    {
                        ++maxLocal;
                        nums[tmp] = mark(nums[tmp], N);
                        tmp = undoMark(nums[tmp], N);
                    }
                    else
                    {
                        maxLen = max(maxLen, maxLocal);
                        break;
                    }
                }
            }
            else
                continue;
        }
        return maxLen;
    }
private:
    int mark(int a, int N)
    {
        return (a < 0) ? a : a - N;
    }
    int undoMark(int a, int N)
    {
        return (a < 0) ? a + N : a;
    }
    int isMarked(int a)
    {
        return a < 0;
    }
};
```

