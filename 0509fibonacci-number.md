The **Fibonacci numbers**, commonly denoted `F(n)` form a sequence, called the **Fibonacci sequence**, such that each number is the sum of the two preceding ones, starting from `0` and `1`. That is,

```
F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), for N > 1.
```

Given `N`, calculate `F(N)`.

## 直接计算

没什么难的, 直接计算吧, 这都不会的话重学编程语言

```c++
class Solution {
public:
    int fib(int N) {
        if(N == 0 || N == 1)
            return N;
        int f_Nminus2 = 0;
        int f_Nminus1 = 1;
        int ans = 0;
        for(int i = 1; i < N; ++i)
        {
            ans = f_Nminus2 + f_Nminus1;
            f_Nminus2 = f_Nminus1;
            f_Nminus1 = ans;
        }
        return ans;
    }
};
```

