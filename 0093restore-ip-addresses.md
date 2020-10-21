Given a string `s` containing only digits, return all possible valid IP addresses that can be obtained from `s`. You can return them in **any** order.

A **valid IP address** consists of exactly four integers, each integer is between `0` and `255`, separated by single dots and cannot have leading zeros. For example, "0.1.2.201" and "192.168.1.1" are **valid** IP addresses and "0.011.255.245", "192.168.1.312" and "192.168@1.1" are **invalid** IP addresses. 

 

**Example 1:**

```
Input: s = "25525511135"
Output: ["255.255.11.135","255.255.111.35"]
```

**Example 2:**

```
Input: s = "0000"
Output: ["0.0.0.0"]
```

**Example 3:**

```
Input: s = "1111"
Output: ["1.1.1.1"]
```

**Example 4:**

```
Input: s = "010010"
Output: ["0.10.0.10","0.100.1.0"]
```

**Example 5:**

```
Input: s = "101023"
Output: ["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]
```

 

**Constraints:**

- `0 <= s.length <= 3000`
- `s` consists of digits only.

## 1 DFS

每一个结点可以选择截取的方法只有 3 种：截 1 位、截 2 位、截 3 位，因此每一个结点可以生长出的分支最多只有 3 条分支；

 假设当前的ip地址已经找到了前i个值, 还剩4-i个. 这里假设ip地址总共有4个值, 每个值都是0到255.

注意剪枝. 当剩余的字符串长度小于4-i时或者剩余字符串长度大于3*(4-i)时, 直接返回.因为无论如何后面都凑不出合法的ip地址了

当出现大于255的数时, 直接返回. 当出现2位数或3位数并且是0开头时, 直接返回.

```java
class Solution {
    public List<String> restoreIpAddresses(String s) {
        if(s == null || s.length() < 4 || s.length() > 4 * 3)
            return new ArrayList<String>();
        List<List<String>> ans = new ArrayList<>();
        List<String> curr = new ArrayList<>();
        restoreIPAddress(ans, curr, s, 0, s.length());
        List<String> ans1 = new ArrayList<>();
        for(List<String> i : ans)
        {
            String tmp = i.get(0) + "." + i.get(1) + "." + i.get(2) + "." + i.get(3);
            ans1.add(tmp);
        }
        return ans1;
    }
    private void restoreIPAddress(List<List<String>> ans, List<String> curr, String s, int begin, int end)
    {
        if(begin == end && curr.size() == 4)
        {
            ans.add(new ArrayList<>(curr));
            return;
        }
        int currSize = curr.size();
        int s_len = end - begin;
        if(s_len < 4 - currSize || s_len > (4 - currSize) * 3)
            return;
        // get 1-len ip
        if(end < begin + 1)
            return;
        String tmp = s.substring(begin, begin + 1);
        curr.add(tmp);
        restoreIPAddress(ans, curr, s, begin + 1, end);
        curr.remove(curr.size() - 1);
        // get 2-len ip
        if(end < begin + 2)
            return;
        tmp = s.substring(begin, begin + 2);
        if(tmp.charAt(0) == '0')
            return;
        curr.add(tmp);
        restoreIPAddress(ans, curr, s, begin + 2, end);
        curr.remove(curr.size() - 1);
        // get 3-len ip
        if(end < begin + 3)
            return;
        tmp = s.substring(begin, begin + 3);
        if(tmp.charAt(0) == '0')
            return;
        if(Integer.parseInt(tmp) > 255)
            return;
        curr.add(tmp);
        restoreIPAddress(ans, curr, s, begin + 3, end);
        curr.remove(curr.size() - 1);
    }
}
```

## 2 动态规划

**我本以为动态规划应该比DFS快的, 没想到反而比DFS慢, 并且标准答案用的就是DFS!!**

dp[i] 存储所有s从0到i的字串可能组成的ip地址前缀.

例如 s = "25525511135" 那么

> `dp[0] = [[2]]`
>
> `dp[1] = [[25], [2, 5]]`
>
> `dp[2] = [[255], [25, 5], [2, 55], [2, 5, 5]]`

所以对于dp[i], 可以直接从dp[i-1]得到. 

对于所有的dp[i-1]的元素, 可以选择在后面添加一个新的8位ip地址, 例如`[2, 5] -> [2, 5, 5]`. 但要保证size < 4

也可以选择append在最后一个8位ip地址后面, 例如 `[2, 5] -> [2, 55]`. 但要保证值小于255并且不能以0开头.

最后将`dp[s.length() - 1]`转化成`List<String>`即可

```java
class Solution {
    public List<String> restoreIpAddresses(String s) {
        if(s == null || s.length() < 4 || s.length() > 4 * 3)
            return new ArrayList<String>();
        List<List<String>>[] dp = new List[s.length()];
        dp[0] = new ArrayList<List<String>>();
        List<String> dp0 = new ArrayList<>(Arrays.asList("" + s.charAt(0)));
        dp[0].add(dp0);
        for(int i = 1; i < s.length(); ++i)
        {
            dp[i] = new ArrayList<List<String>>();
            for(List<String> list : dp[i-1])
            {
                // [2.5] -> [2.5.5]
                if(list.size() < 4)
                {
                    List<String> newList = new ArrayList<>(list);
                    newList.add("" + s.charAt(i));
                    dp[i].add(newList);
                }
                // [2.5] -> [2.55]
                List<String> newList1 = new ArrayList<>(list);
                // check if the last element valid after append s.charAt(i)
                String lastString = newList1.remove(newList1.size() - 1);
                if(lastString.charAt(0) == '0')
                    continue;
                lastString = lastString + s.charAt(i);
                if(Integer.parseInt(lastString) > 255)
                    continue;
                newList1.add(lastString);
                dp[i].add(newList1);
            }
        }
        List<String> ans = stringListToIp(dp[s.length() - 1]);
        return ans;
    }
    private List<String> stringListToIp(List<List<String>> a)
    {
        List<String> ans = new ArrayList<>();
        for(List<String> i : a)
        {
            if(i.size() != 4)
                continue;
            String tmp = i.get(0) + "." + i.get(1) + "." + i.get(2) + "." + i.get(3);
            ans.add(tmp);
        }
        return ans;
    }
}
```

