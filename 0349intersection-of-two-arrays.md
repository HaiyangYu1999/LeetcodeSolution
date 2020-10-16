Given two arrays, write a function to compute their intersection.

**Example 1:**

```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]
```

**Example 2:**

```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
```

**Note:**

- Each element in the result must be unique.
- The result can be in any order.

## 双hashset

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> set1 = new HashSet<>();
        for(int i : nums1)
            set1.add(i);
        Set<Integer> set2 = new HashSet<>();
        for(int i : nums2){
            if(set1.contains(i)){
                set2.add(i);
            }
        }
        int[] ans = new int[set2.size()];
        int k = 0;
        for(int i : set2)
            ans[k++] = i;
        return ans;
    }
}
```

