Write an algorithm to determine if a number `n` is "happy".

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it **loops endlessly in a cycle** which does not include 1. Those numbers for which this process **ends in 1** are happy numbers.

Return True if `n` is a happy number, and False if not.

**Example:** 

```
Input: 19
Output: true
Explanation: 
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```

## 1 hashset

这题的算法很简单, 按照要求一次次的模拟即可.

重点是判断是不是最后以1结束. 不是以1结束肯定是个链式循环. 要判断有没有这样的循环, 可以用快慢指针, 也可以利用一个集合存储遍历过的元素, 如果再次计算出了遍历过的元素, 那么肯定在循环中. 

```java
class Solution {
    public boolean isHappy(int n) {
        if(n <= 0)
            return false;
        Set<Integer> set = new HashSet<>();
        
        do
        {
            set.add(n);
            n = getDigitSquareSum(n);
            if(n == 1)
                return true;
        }while(!set.contains(n));
        return false;
    }
    private int getDigitSquareSum(int n)
    {
        int sum = 0;
        while(n > 0)
        {
            int tmp = n % 10;
            sum += tmp * tmp;
            n /= 10;
        }
        return sum;
    }
}
```

## 2 快慢指针

写完快慢指针竟然发现代码比用set还要少. 一开始还以为用快慢指针肯定复杂. 

看来以后判断是否有环这种问题还是快慢指针好用.

```java
class Solution {
    public boolean isHappy(int n) {
        if(n <= 0)
            return false;
        int fast = n;
        int slow = n;
        while(true)
        {
            fast = getDigitSquareSum(getDigitSquareSum(fast));
            slow = getDigitSquareSum(slow);
            if(fast == 1 || slow == 1){
                return true;
            }
            else if(fast == slow){
                return false;
            }
        }
    }
    private int getDigitSquareSum(int n)
    {
        int sum = 0;
        while(n > 0)
        {
            int tmp = n % 10;
            sum += tmp * tmp;
            n /= 10;
        }
        return sum;
    }
}
```

