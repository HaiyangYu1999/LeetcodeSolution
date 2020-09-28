Validate if a given string can be interpreted as a decimal number.

Some examples:
`"0"` => `true`
`" 0.1 "` => `true`
`"abc"` => `false`
`"1 a"` => `false`
`"2e10"` => `true`
`" -90e3  "` => `true`
`" 1e"` => `false`
`"e3"` => `false`
`" 6e-1"` => `true`
`" 99e2.5 "` => `false`
`"53.5e93"` => `true`
`" --6 "` => `false`
`"-+3"` => `false`
`"95a54e53"` => `false`

**Note:** It is intended for the problem statement to be ambiguous. You should gather all requirements up front before implementing one. However, here is a list of characters that can be in a valid decimal number:

- Numbers 0-9
- Exponent - "e"
- Positive/negative sign - "+"/"-"
- Decimal point - "."

Of course, the context of these characters also matters in the input.

**Update (2015-02-10):**
The signature of the `C++` function had been updated. If you still see your function signature accepts a `const char *` argument, please click the reload button to reset your code definition.

## DFA

自己不会做, 借鉴于https://leetcode-cn.com/problems/valid-number/solution/biao-qu-dong-fa-by-user8973/

```java
class Solution {
    private static final int S0 = 0;  // begin state
    private static final int S1 = 1;  // digits before 'e' and '.', after head '+/-'
    private static final int S2 = 2; // state to absorb the first "+/-" before 'e'
    private static final int S3 = 3; // state that absorb a "."
    private static final int S4 = 4; // digits after '.'
    private static final int S5 = 5; // state that absorb 'e'
    private static final int S6 = 6; // digits after 'e'
    private static final int S7 = 7; // state that absorb '+/-' after 'e'
    private static final int Serr = -1; //error state
    private Set<Integer> validSet = new HashSet<>();
    private int[][] tran = new int[][]{
            {1,2,3,-1},
            {1,-1,4,5},
            {1,-1,3,-1},
            {4,-1,-1,-1},
            {4,-1,-1,5},
            {6,7,-1,-1},
            {6,-1,-1,-1},
            {6,-1,-1,-1}};
    public Solution()
    {
        validSet.add(S0);
        validSet.add(S1);
        validSet.add(S4);
        validSet.add(S6);
    }
    private int make(char ch)
    {
        if(Character.isDigit(ch))
            return 0;
        else if(ch == '+' || ch == '-')
            return 1;
        else if(ch == '.')
            return 2;
        else if(ch == 'e')
            return 3;
        else
            return -1;
    }
    public boolean isNumber(String s) {
        s = s.trim();
        if(s.isEmpty())
            return false;
        if(s.charAt(0) == '.')
            return beginWithDot(s);
        int state = S0;
        for(int i = 0; i < s.length(); ++i)
        {
            char ch = s.charAt(i);
            int curr = make(ch);
            if(curr == -1)
                return false;
            state = tran[state][curr];
            if(state == -1)
                return false;
        }
        return validSet.contains(state);
    }
    private boolean beginWithDot(String s)
    {
        if(s.length() == 1)
            return false;
        else if(s.charAt(1) == 'e')
            return false;
        else
        {
            int state = S3;
            for(int i = 1; i < s.length(); ++i)
            {
                char ch = s.charAt(i);
                int curr = make(ch);
                if(curr == -1)
                    return false;
                state = tran[state][curr];
                if(state == -1)
                    return false;
            }
            return validSet.contains(state);
        }
    }
}
```

