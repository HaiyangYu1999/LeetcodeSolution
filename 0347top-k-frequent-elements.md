Given a non-empty array of integers, return the ***k\*** most frequent elements.

**Example 1:**

```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

**Example 2:**

```
Input: nums = [1], k = 1
Output: [1]
```

**Note:**

- You may assume *k* is always valid, 1 ≤ *k* ≤ number of unique elements.
- Your algorithm's time complexity **must be** better than O(*n* log *n*), where *n* is the array's size.
- It's guaranteed that the answer is unique, in other words the set of the top k frequent elements is unique.
- You can return the answer in any order.

## 1. 暴力

使用一个hashmap存储每个数字出现的次数, 按次数排序, 取前k个.

时间复杂度为O(max(n, m*logm)) 其中n为数组长度, m为数组中不同元素的总个数

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int i : nums)
        {
            map.putIfAbsent(i,0);
            map.put(i, map.get(i) + 1);
        }
        Map.Entry<Integer, Integer>[] arr = (Map.Entry<Integer, Integer>[]) new Map.Entry[map.size()];
        int s = 0;
        for(Map.Entry<Integer, Integer> entry : map.entrySet())
        {
            arr[s++] = entry;
        }
        Arrays.sort(arr, (entry1, entry2) -> {
            return entry2.getValue() - entry1.getValue();
        });
        int[] ans = new int[k];
        for(int i = 0; i < k; ++i)
        {
            ans[i] = arr[i].getKey();
        }
        return ans;
    }
}
```

## 2. 优先队列

和1的思路差不多, 都要先用hashmap. 但是之后可以用优先队列完成选取前k个的操作. 时间复杂度可降为O(max(n, m*logk)) 其中n为数组长度, m为数组中不同元素的总个数.

**这里还想到了用快速排序的partition思想求的第k大的数, 可以将复杂度降为O(n). 但是这是行不通的. 因为需要求全部的第1大, 第2大, ... 第k大. 而不是仅仅求出第k大的数就可以的.**

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int i : nums)
        {
            map.putIfAbsent(i,0);
            map.put(i, map.get(i) + 1);
        }
        PriorityQueue<Map.Entry<Integer, Integer>> pq = new PriorityQueue<>((entry1, entry2) -> entry1.getValue() - entry2.getValue());
        for(Map.Entry<Integer, Integer> entry : map.entrySet())
        {
            if(pq.size() < k)
            {
                pq.offer(entry);
            }
            else
            {
                Map.Entry<Integer, Integer> top = pq.peek();
                if(top.getValue() < entry.getValue())
                {
                    pq.poll();
                    pq.offer(entry);
                }
            }
        }
        int[] ans = new int[k];
        int s = 0;
        for(Map.Entry<Integer, Integer> entry : pq)
        {
            ans[s++] = entry.getKey();
        }
        return ans;
    }
}
```

## 3.桶排序

**因为待排序的值都在0到n这个范围内, 所以可以用桶排序.** 复杂度O(n)

这也提醒了我们当排序的范围不大的时候, 可以用这种非比较排序算法.

**平常基本上不用桶排序, 所以很难想到这个方法. 所以还是要多练习, 多总结.**

再选取前k个即可. 

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int i : nums)
        {
            map.put(i, map.getOrDefault(i, 0) + 1);
        }
        List<Integer>[] bucket = (List<Integer>[]) new List[nums.length + 1];
        for(int i = 0; i != bucket.length; ++i)
        {
            bucket[i] = new ArrayList<Integer>();
        }
        for(Map.Entry<Integer, Integer> entry : map.entrySet())
        {
            bucket[entry.getValue()].add(entry.getKey());
        }
        int[] ans = new int[k];
        int s = 0;
        for(int i = bucket.length - 1; i != -1; --i)
        {
            for(int j : bucket[i])
            {
                if(s == k)
                    break;
                ans[s++] = j;
            }
        }
        return ans;
        
    }
}
```



