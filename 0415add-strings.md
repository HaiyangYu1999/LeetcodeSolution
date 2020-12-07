Given two non-negative integers `num1` and `num2` represented as string, return the sum of `num1` and `num2`.

**Note:**

1. The length of both `num1` and `num2` is < 5100.
2. Both `num1` and `num2` contains only digits `0-9`.
3. Both `num1` and `num2` does not contain any leading zero.
4. You **must not use any built-in BigInteger library** or **convert the inputs to integer** directly.

## 大整数加法

按位相加即可, 使用`char[]`保存结果.

这里要注意`char[]`转化为`String`的方法. 可以用`String(char[] chars)`将整个数组转化为String. 但是这种方法不好, 因为我们可能需要将`char[]`前面的`0`或`'0'`去掉后再转换成String. 

可以使用`String(char value[], int offset, int count)`来转换, 将value数组的第offset个元素开始, 总共count个元素的这一个子数组转为String. 可以完美解决我们的问题

```java
class Solution {
    public String addStrings(String num1, String num2) {
        if(num1 == null || num2 == null){
            throw new NullPointerException();
        }
        int maxLen = Math.max(num1.length(), num2.length());
        char[] res = new char[maxLen + 1];
        int i = num1.length() - 1;
        int j = num2.length() - 1;
        int carry = 0;
        int remainder = 0;
        int k = res.length - 1;
        while(i >= 0 || j >= 0){
            int a1 = 0;
            int a2 = 0;
            if(i < 0){
                a1 = 0;
                a2 = num2.charAt(j) - '0';
            }else if(j < 0){
                a1 = num1.charAt(i) - '0';
                a2 = 0;
            }else{
                a1 = num1.charAt(i) - '0';
                a2 = num2.charAt(j) - '0';
            }
            int sum = a1 + a2 + carry;
            carry = sum / 10;
            remainder = sum % 10;
            res[k--] = (char) (remainder + '0');
            --i;
            --j;
        }
        if(carry > 0){
            res[k--] = (char) (carry + '0');
        }
        int s = 0;
        while(s < res.length - 1){
            if(res[s] == 0 || res[s] == '0'){
                ++s;
            }
            else{
                break;
            }
        }
        return new String(res,s, res.length - s);
    }
}
```