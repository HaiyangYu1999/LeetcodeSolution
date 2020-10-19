Given two integers *n* and *k*, return all possible combinations of *k* numbers out of 1 ... *n*.

You may return the answer in **any order**.

 

**Example 1:**

```
Input: n = 4, k = 2
Output:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

**Example 2:**

```
Input: n = 1, k = 1
Output: [[1]]
```

 

**Constraints:**

- `1 <= n <= 20`
- `1 <= k <= n`

## 递归

容易得到, 从1到n选择k个数的所有方式等于从2到n选择k个数的所有方式A加上从2到n选择k-1个数的所有方式B. 其中, B中的所有list都要add上1. 那么A并B就得到了从1到n选择k个数的所有方式

```java
class Solution {
    public List<List<Integer>> combine(int n, int k) {
        return combine(1, n, k);
    }
    private List<List<Integer>> combine(int begin, int end, int k)
    {
        //assert(k >= 1)
        List<List<Integer>> lists = new ArrayList<>();
        if(end - begin + 1 < k)
            return lists; //you must return [] not [[]]!
        if(k == 1)
        {
            for(int i = begin; i <= end; ++i)
            {
                List<Integer> oneElemList = new ArrayList<>();
                oneElemList.add(i);
                lists.add(oneElemList);
            }
            return lists;
        }
        else
        {
            List<List<Integer>> listsWithoutBegin = combine(begin + 1, end, k);
            List<List<Integer>> listsWithBegin = combine(begin + 1, end, k - 1);
            for(List<Integer> list : listsWithBegin)
                list.add(begin);
            listsWithoutBegin.addAll(listsWithBegin);
            return listsWithoutBegin;
        }
    }
}
```

