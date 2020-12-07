Given an integer array `nums`, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once. You can return the answer in **any order**.

**Follow up:** Your algorithm should run in linear runtime complexity. Could you implement it using only constant space complexity?

 

**Example 1:**

```
Input: nums = [1,2,1,3,2,5]
Output: [3,5]
Explanation:  [5, 3] is also a valid answer.
```

**Example 2:**

```
Input: nums = [-1,0]
Output: [-1,0]
```

**Example 3:**

```
Input: nums = [0,1]
Output: [1,0]
```

 

**Constraints:**

- `1 <= nums.length <= 30000`
-  Each integer in `nums` will appear twice, only two integers will appear once.

## 1 暴力

这题我也不会, 只能暴力哈希表了

```java
class Solution {
    public int[] singleNumber(int[] nums) {
        int[] a = new int[2];
        List<Integer> list = new ArrayList<>();
        Map<Integer, Integer> map = new HashMap<>();
        for(int i : nums)
        {
            map.putIfAbsent(i,0);
            map.put(i,map.get(i) + 1);
        }
        for(Map.Entry<Integer, Integer> entry : map.entrySet())
        {
            if(entry.getValue() == 1)
                list.add(entry.getKey());
        }
        a[0] = list.get(0);
        a[1] = list.get(1);
        return a;
    }
}
```

## 2 位运算

首先还是老样子异或, 异或最后的结果是diff = a^b, a和b为两个出现一次的数. diff的每一位为1的位就表明a和b在这一位上不同. 然后执行diff = diff & (-diff). 这一步是为了求出diff从右到左第一个不是0的位.

例如, diff = 14 = 0b1110. 那么经过运算后diff = 0b010. 即从右到左第一个不是0的是第2位.

那么, 重点来了, **a和b其中必有一个第二位是1, 另一个第二位是0!**

所以可以根据这个条件, 和diff做与运算为0的放一类, 和diff做与运算为1的放一类, 这样, a和b一定会在不同的类里面!!

再异或即可.

想出这个方法的人真的是天才!!!

```java
class Solution {
    public int[] singleNumber(int[] nums) {
        int xor = 0;
        for(int i : nums)
            xor ^= i;
        int diff = xor & (-xor);
        int a = 0;
        for(int i : nums)
            if((i & diff) != 0)
                a ^= i;
        int b = xor ^ a;
        return new int[]{a,b};
    }
}
```

