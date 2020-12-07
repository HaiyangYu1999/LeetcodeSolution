Given four lists A, B, C, D of integer values, compute how many tuples `(i, j, k, l)` there are such that `A[i] + B[j] + C[k] + D[l]` is zero.

To make problem a bit easier, all A, B, C, D have same length of N where 0 ≤ N ≤ 500. All integers are in the range of -228 to 228 - 1 and the result is guaranteed to be at most 231 - 1.

**Example:**

```
Input:
A = [ 1, 2]
B = [-2,-1]
C = [-1, 2]
D = [ 0, 2]

Output:
2

Explanation:
The two tuples are:
1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0
```

## 1. O(n^3)解法

这个O(n^3)的算法纯粹是被leetcode 15和leetcode 18给带跑偏了. 在15和18题中, 用的是双指针法, 3Sum为O(n^2)复杂度, 4Sum为O(n^3)复杂度. 我以为这个题也是最快O(n^3), 所以想都没想就用双指针做了. 

双指针做的时候还碰到了一个问题, [-1,-1]和[1,1]这两个数组和为0的一共有4组, 双指针做不到找到全部的4组. 所以再增设变量ALen, BLen, 当指针(i, j)满足`A[i] + B[j] == 0`的时候, 找出所有和`A[i]`相等的元素数量ALen, 和`B[j]`相等的元素数量BLen, 所以满足条件的组合就有`ALen * BLen`个

```java
class Solution {
    public int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
        int ans = 0;
        Arrays.sort(A);
        Arrays.sort(B);
        for(int c : C){
            for(int d : D){
                int tar = -(c + d);
                ans += twoSumCount(A, B, tar);
            }
        }
        return ans;
    }
    private int twoSumCount(int[] A, int[] B, int tar){
        //assume A and B are sorted
        int count = 0;
        
        int indexA = 0;
        int indexB = B.length - 1;
        
        while(indexA < A.length && indexB >= 0){
            if(A[indexA] + B[indexB] < tar){
                ++indexA;
            }
            else if(A[indexA] + B[indexB] > tar){
                --indexB;
            }
            else{
                int ALen = 0;
                int BLen = 0;
                for(int j = indexA; j < A.length; ++j){
                    if(A[j] == A[indexA]){
                        ++ALen;
                    }
                }
                for(int j = indexB; j >= 0; --j){
                    if(B[j] == B[indexB]){
                        ++BLen;
                    }
                }
                count += ALen * BLen;
                
                indexA += ALen;
                indexB -= BLen;
            }
        }
        return count;
    }
}
```

## 2. O(n^2)解法

但这个题还是有O(n^2)解法的. 就是两两求和.

先把A[]和B[]求和的结果放到一个HashMap中, 然后遍历C[]和D[], 找出-(c+d)在HashMap中的元素即可.

但是问题来了, 为什么Leetcode 18不能用这种方法呢?  

我的观点是因为18题中只有一个数组`numbers[]`, 如果把`A[], B[], C[], D[]`都赋值为`numbers[]`的话, 就会出现重复. 

```java
class Solution {
    public int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int a : A){
            for(int b : B){
                map.put(a + b, map.getOrDefault(a + b, 0) + 1);
            }
        }
        int ans = 0;
        for(int c : C){
            for(int d : D){
                ans += map.getOrDefault(-(c + d), 0);
            }
        }
        return ans;
    }
}
```

