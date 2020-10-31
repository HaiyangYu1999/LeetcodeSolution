Given a list of non-negative integers `nums`, arrange them such that they form the largest number.

**Note:** The result may be very large, so you need to return a string instead of an integer.

 

**Example 1:**

```
Input: nums = [10,2]
Output: "210"
```

**Example 2:**

```
Input: nums = [3,30,34,5,9]
Output: "9534330"
```

**Example 3:**

```
Input: nums = [1]
Output: "1"
```

**Example 4:**

```
Input: nums = [10]
Output: "10"
```

 

**Constraints:**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 109`

## 自定义排序

很明显的发现, 字典序越大的数, 越应该放前面. 例如[9, 88, 777]肯定是988777最大

但是, 有一些特殊情况需要处理. 比如前缀相同的字符串的排序

例如 [34, 343]. 这样因为343 34 < 34 343 所以34应该放前面

但是这个例子[32, 323]. 就变成了32 323 < 323 32 所以323应该放在前面

**所以问题转变成了, 相同前缀的字符串, 哪个应该放前面呢?**

**所以应该判断 a + b 和 b + a的大小.** 

**如果a+b大, a就放到前面. 反之b放在前面.**

上面的判断方式我一开始没想到, 自己想的方法是按位判断的. 又麻烦又难以理解. 还是太菜了😭😭

```java
class Solution {
    public String largestNumber(int[] nums) {
        String[] strings = new String[nums.length];
        for(int i = 0; i < nums.length; ++i)
        {
            strings[i] = nums[i] + "";
        }
        Comparator<String> cmp = new Comparator<String>() {
            @Override
            public int compare(String a, String b) {
                String s1 = a + b;
                String s2 = b + a;
                if(s1.compareTo(s2) > 0)
                {
                    return 1;
                }
                else if(s1.compareTo(s2) == 0)
                {
                    return 0;
                }
                else
                {
                    return -1;
                }
            }
        };
        Arrays.sort(strings, cmp);
        StringBuilder sb = new StringBuilder();
        for(int i = strings.length - 1; i > -1; --i)
        {
            sb.append(strings[i]);
        }
        String ans = sb.toString();
        if(ans.length() < 1 || ans.charAt(0) == '0')
            return "0";
        return ans.toString();
    }
    
}
```



