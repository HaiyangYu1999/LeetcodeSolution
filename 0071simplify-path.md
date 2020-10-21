Given an **absolute path** for a file (Unix-style), simplify it. Or in other words, convert it to the **canonical path**.

In a UNIX-style file system, a period `.` refers to the current directory. Furthermore, a double period `..` moves the directory up a level.

Note that the returned canonical path must always begin with a slash `/`, and there must be only a single slash `/` between two directory names. The last directory name (if it exists) **must not** end with a trailing `/`. Also, the canonical path must be the **shortest** string representing the absolute path.

 

**Example 1:**

```
Input: "/home/"
Output: "/home"
Explanation: Note that there is no trailing slash after the last directory name.
```

**Example 2:**

```
Input: "/../"
Output: "/"
Explanation: Going one level up from the root directory is a no-op, as the root level is the highest level you can go.
```

**Example 3:**

```
Input: "/home//foo/"
Output: "/home/foo"
Explanation: In the canonical path, multiple consecutive slashes are replaced by a single one.
```

**Example 4:**

```
Input: "/a/./b/../../c/"
Output: "/c"
```

**Example 5:**

```
Input: "/a/../../b/../c//.//"
Output: "/c"
```

**Example 6:**

```
Input: "/a//b////c/d//././/.."
Output: "/a/b/c"
```

## 1 利用StringBuilder

首先用split方法分割"/"得到字符串数组strings

首先建立一个空的stringbuilder sb

下面依次处理strings里面的字符串

对于`"."`和`""`, 直接跳过不做处理.

对于一般的字符串s, 如果sb为空或者sb最后一个字符不是"/", 就要append上"/s", 反之, 只需要append上"s"

对于`".."`, 寻找sb中**最后一个**"/"的位置lastSlashIndex, 如果lastSlashIndex == -1, 就跳过. 否则, 删除lastSlashIndex到末尾的所有字符.

最后返回sb.toString(). 如果sb.toString() 为`""`, 就返回`"/"`

```java
class Solution {
    public String simplifyPath(String path) {
        if(path == null || "".equals(path))
            return path;
        StringBuilder sb = new StringBuilder();
        String[] strings = path.split("/");
        for(String string : strings)
        {
            if("".equals(string) || ".".equals(string))
                continue;
            else if("..".equals(string))
            {
                int lastSlashIndex = sb.lastIndexOf("/");
                if(lastSlashIndex < 0)
                {
                    continue;
                }
                else
                {
                    sb.delete(lastSlashIndex, sb.length());
                }
            }
            else
            {
                if(sb.length() == 0 || sb.charAt(sb.length() - 1) != '/')
                    sb.append("/");
                sb.append(string);
            }
        }
        String ans = sb.toString();
        return "".equals(ans) ? "/" : ans;
    }
}
```

## 2 利用list

上面的过程完全可以转化成list. 并且在碰到`".."`的情况时效率更高(因为不用找最后一个"/"了). 但是在字符串拼接上string会比stringbuilder慢

```java
class Solution {
    public String simplifyPath(String path) {
        if(path == null || "".equals(path))
            return path;
        List<String> list = new ArrayList<>();
        String[] strings = path.split("/");
        for(String string : strings)
        {
            if("".equals(string) || ".".equals(string))
                continue;
            else if("..".equals(string))
            {
                if(list.isEmpty())
                    continue;
                else
                    list.remove(list.size() - 1);
            }
            else
            {
                list.add(string);
            }
        }
        if(list.isEmpty())
            return "/";
        StringBuilder sb = new StringBuilder();
        for(String i : list)
        {
            sb.append("/");
            sb.append(i);
        }
        return sb.toString();
    }
}
```

