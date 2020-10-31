Find the **k**th largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

**Example 1:**

```
Input: [3,2,1,5,6,4] and k = 2
Output: 5
```

**Example 2:**

```
Input: [3,2,3,1,2,4,5,5,6] and k = 4
Output: 4
```

**Note:**
You may assume k is always valid, 1 ≤ k ≤ array's length.

## 1 优先队列

维护一个k个元素组成的优先队列(小根堆), 如果遍历的元素i比优先队列里最小的元素大, 就poll掉最小的元素, 再把i加入到队列中.

最后, 优先队列中最小的元素即为第k大的元素.

复杂度 O(n * logk)

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for(int i : nums)
        {
            if(pq.size() < k)
            {
                pq.offer(i);
            }
            else
            {
                int tmp = pq.peek();
                if(i > tmp)
                {
                    pq.poll();
                    pq.offer(i);
                }
            }
        }
        return pq.peek();
    }
}
```

## 2 快速排序的思想

利用快速排序的思想, 随机选取一个哨兵sentinel = nums[begin]

然后将其partition两部分, 比sentinel小的在数组左边, 比sentinel大的在数组右边.

这样, 我们就能求出sentinel在数组中的排名是第`end - j + 1`大的元素了. j为sentinel经过partition后最终的位置

如果k == sentinel_rank, 那么直接返回sentinel即可.

如果k > sentinel_rank, 那么说明第k大的数在sentinel左边的子数组中, 从nums[begin], 到nums[j - 1]中寻找第k - j大的元素即可

如果k < sentinel_rank, 那么说明第k大的数在sentinel右边的子数组中, 从nums[j+1]到nums[end]中寻找第k大的数即可

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        return kth(nums, 0, nums.length - 1, k);
    }
    private int kth(int[] nums, int begin, int end, int k)
    {
        int sentinel = nums[begin];
        int i = begin;
        int j = end;
        while(true)
        {
            while(i < end && nums[i] <= sentinel)
                ++i;
            while(j > begin && nums[j] >= sentinel)
                --j;
            if(i >= j)
            {
                //swap nums[begin] and nums[j]
                int tmp = nums[j];
                nums[j] = nums[begin];
                nums[begin] = tmp;
                break;
            }
            //swap nums[i] and nums[j]
            int tmp = nums[j];
            nums[j] = nums[i];
            nums[i] = tmp;
        }
        int sentinel_rank = end - j + 1;
        if(sentinel_rank == k)
        {
            return sentinel;
        }
        else if(sentinel_rank < k)
        {
            return kth(nums, begin, j - 1, k - sentinel_rank);
        }
        else
        {
            return kth(nums, j + 1, end, k);
        }
    }
}
```

