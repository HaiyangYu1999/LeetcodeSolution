Given a **non-empty** array of integers, every element appears *three* times except for one, which appears exactly once. Find that single one.

**Note:**

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

**Example 1:**

```
Input: [2,2,3,2]
Output: 3
```

**Example 2:**

```
Input: [0,1,0,1,0,1,99]
Output: 99
```

## 1 哈希表

想不到空间复杂度O(1)的算法了.....

只能hashmap了

```java
class Solution {
    public int singleNumber(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int i : nums)
        {
            map.putIfAbsent(i,0);
            map.put(i,map.get(i) + 1);
        }
        for(Map.Entry<Integer, Integer> entry : map.entrySet())
        {
            if(entry.getValue() == 1)
                return entry.getKey();
        }
        return -1;
    }
}
```

## 2 位运算

思路来源于https://leetcode-cn.com/problems/single-number-ii/solution/zhi-chu-xian-yi-ci-de-shu-zi-ii-by-leetcode/

所以假如数组是这样的[1,1,1,2,2,2,3,3,3,4], 自然是可以求出只出现一次的元素.

但是, 如果数组是这样的[1,2,3,4,1,2,3,1,2,3], 第一次将1传进去, seen_once置为1, seen_twice置为0, 当下一次将2传入, 会不会打乱保存了1只出现了一次的信息的变量seen_one? 所以我们要证明, 这个算法与元素顺序无关. 如果无关的话, 就可以视为对数组[1,1,1,2,2,2,3,3,3,4]进行计算, 显然会得到正确的答案4.

我们假设seen_once的初始值第i位为0, seen_twice的初始值第i位为0. 第一个元素第i位为a, 第二个元素第i位为b. (a, b = 0 or 1)

那么先计算a, 再计算b的结果为:

> `seen_once = a`
>
> `seen_two = (~a)&(0^a) = 0`  
>
> `seen_once =a^b`
>
> `seen_two = ~(a^b)&b` 

先计算b, 后计算a的结果为`(a^b)`和`~(a^b)&a`两者无论a, b取任何值(0或1)都是相等的.

同理, 可以计算出

+ seen_once的初始值第i位为0, seen_twice的初始值第i位为1,第一个元素第i位为a, 第二个元素第i位为b
+ seen_once的初始值第i位为1, seen_twice的初始值第i位为0,第一个元素第i位为a, 第二个元素第i位为b
+ seen_once的初始值第i位为1, seen_twice的初始值第i位为1,第一个元素第i位为a, 第二个元素第i位为b

这三种情况下, 先计算第一个元素和先计算第二个元素都是相等的. 因为i的任意性, 所以整个的值seen_one, seen_two也和计算顺序无关.

这就保证了[1,2,3,4,1,2,3,1,2,3]这样的数组可以通过交换, 变成[1,1,1,2,2,2,3,3,3,4]. 后者的正确性显然.

```java
class Solution {
    public int singleNumber(int[] nums) {
        int one = 0;
        int two = 0;
        for(int i : nums)
        {
            one = (~two) & (one ^ i);
            two = (~one) & (two ^ i);
        }
        return one;
    }
}
```

