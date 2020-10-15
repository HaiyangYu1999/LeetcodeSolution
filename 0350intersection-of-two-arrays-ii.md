Given two arrays, write a function to compute their intersection.

**Example 1:**

```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2,2]
```

**Example 2:**

```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [4,9]
```

**Note:**

- Each element in the result should appear as many times as it shows in both arrays.
- The result can be in any order.

**Follow up:**

- What if the given array is already sorted? How would you optimize your algorithm?
- What if *nums1*'s size is small compared to *nums2*'s size? Which algorithm is better?
- What if elements of *nums2* are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?

## 哈希表

先用哈希表储存某个数组的信息, 然后遍历另一个数组, 得到交集, 也能用排序做, 但是时间复杂度更高.空间复杂度一样, 都是O(m + n)

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        Map<Integer, Integer> map1 = new HashMap<>();
        for(int i : nums1)
        {
            map1.putIfAbsent(i,0);
            map1.put(i, map1.get(i) + 1);
        }
        List<Integer> ans = new ArrayList<>();
        for(int i : nums2)
        {
            if(map1.get(i) != null && map1.get(i) != 0)
            {
                map1.put(i, map1.get(i) - 1);
                ans.add(i);
            }
        }
        int[] ret = new int[ans.size()];
        int k = 0;
        for(int i : ans)
            ret[k++] = i;
        return ret;
    }
}
```

