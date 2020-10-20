Given two words (*beginWord* and *endWord*), and a dictionary's word list, find the length of shortest transformation sequence from *beginWord* to *endWord*, such that:

1. Only one letter can be changed at a time.
2. Each transformed word must exist in the word list.

**Note:**

- Return 0 if there is no such transformation sequence.
- All words have the same length.
- All words contain only lowercase alphabetic characters.
- You may assume no duplicates in the word list.
- You may assume *beginWord* and *endWord* are non-empty and are not the same.

**Example 1:**

```
Input:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

Output: 5

Explanation: As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
return its length 5.
```

**Example 2:**

```
Input:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

Output: 0

Explanation: The endWord "cog" is not in wordList, therefore no possible transformation.
```

## 1 暴力

暴力地构造一个邻接矩阵图, 用BFS求最短距离.

时间复杂度是O(m*n^2) m是单词长度， n是wordList长度

```java
class Solution {
    public List<String> list;
    public int[][] dist;
    public boolean[] isTraversed;
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        init(beginWord, endWord, wordList);
        int shortest = 0;
        Queue<Integer> q = new ArrayDeque<>();
        q.offer(0);
        isTraversed[0] = true;
        while(!q.isEmpty())
        {
            int currSize = q.size();
            while(currSize != 0)
            {
                int curr = q.poll();
                --currSize;
                for(int i = 0; i < list.size(); ++i)
                {
                    if(dist[curr][i] == 1 && !isTraversed[i])
                    {
                        if(list.get(i).equals(endWord))
                            return shortest + 2;
                        q.offer(i);
                        isTraversed[i] = true;
                    }
                }
            }
            ++shortest;
        }
        return 0;

    }
    private void init(String beginWord, String endWord, List<String> wordList)
    {
        list = new ArrayList<>();
        list.add(beginWord);
        list.addAll(wordList);
        dist = new int[list.size()][list.size()];
        isTraversed = new boolean[list.size()];
        for(int i = 0; i < list.size(); ++i)
        {
            for(int j = 0; j < list.size(); ++j)
            {
                dist[i][j] = (i > j) ? dist[j][i] : getDist(i,j);
            }
        }
    }
    private int getDist(int i, int j)
    {
        if(i == j)
            return 0;
        String s1 = list.get(i);
        String s2 = list.get(j);
        int dist = 0;
        for(int k = 0; k < s1.length(); ++k)
        {
            if(s1.charAt(k) != s2.charAt(k))
                ++dist;
        }
        return dist;
    }
}
```

## 2 一次字符变化 + BFS

来源 https://leetcode-cn.com/problems/word-ladder/solution/dan-ci-jie-long-by-leetcode/

首先要构造通用状态到所有单词的映射map. 例如, dog的通用状态就有3个, `*og, d*g, do*`. 如果两个单词有一个相同的通用状态, 那么这两个单词就是相通的. 例如dog, dig 有相同的通用状态d*g

而map就存储了任意一个给定的通用状态都对应了哪些单词.

例如, `["hot","dot","dog","lot","log","cog"]`就对应了下面的map

```
d*g -> dog 
c*g -> cog 
ho* -> hot 
*og -> log cog dog 
h*t -> hot 
lo* -> lot log 
l*t -> lot 
l*g -> log 
do* -> dot dog 
*ot -> lot dot hot 
d*t -> dot 
co* -> cog 
```

那么, 我们从beginWord = "hit"开始, 首先将元组(hit, 1)放入队列,

取出(hit,1), 找到hit所有的通用状态, `*it, h*t, hi*`, 发现只有`h*t`在map里面, 就把`h*t`对应的所有单词都加入到队列里面, 同时, distance要加1, 即(hot, 2)加入到队列中, 并且将hot标记为已经遍历过的. 这时, 队列中只有(hot,2)

然后取出(hot, 2), 找到hot所有通用状态, `*ot, h*t, ho*`. 对应的所有单词为`lot, dot, hot, hot, hot`. 这时hot已经遍历过了, 就只把(lot, 3), (dot,3)加入队列, 并设置lot和dot为已经遍历过的.

以此类推, 直到取出的值等于endWord. 返回元组第二个元素.

```java
class Solution {
    public Map<String, Set<String>> map;
    public Map<String, Boolean> isVisited = new HashMap<>();;
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        init(wordList);
        Queue<Pair<String, Integer>> q = new ArrayDeque<>();
        q.offer(new Pair<String, Integer>(beginWord, 1));
        isVisited.put(beginWord, true);
        while(!q.isEmpty())
        {
            Pair<String, Integer> pair = q.poll();
            String s = pair.getKey();
            if(endWord.equals(s))
            {
                return pair.getValue();
            }
            for(int i = 0; i < s.length(); ++i)
            {
                String wildCard_s = s.substring(0,i) + "*" + s.substring(i+1, s.length());
                Set<String> adjacentStrings = map.get(wildCard_s);
                if(adjacentStrings == null)
                    continue;
                for(String diff1char_s : adjacentStrings)
                {
                    if(isVisited.get(diff1char_s) != null)
                    {
                        //string has already been traversed 
                        continue;
                    }
                    q.offer(new Pair<String, Integer>(diff1char_s, pair.getValue() + 1));
                    isVisited.put(diff1char_s, true);
                }
            }
        }
        return 0;
    }
    
    private void init(List<String> wordList)
    {
        map = new HashMap<>();
        for(String str : wordList)
        {
            for(int i = 0; i < str.length(); ++i)
            {
                String wildCard_s = str.substring(0,i) + "*" + str.substring(i + 1, str.length());
                map.putIfAbsent(wildCard_s, new HashSet<String>());
                map.get(wildCard_s).add(str);
            }
        }
    }
}
```

## 3 双向BFS

为了解决BFS搜索的越来越多, 使用双向BFS搜索.

这时要建立两个队列qBegin, qEnd用来遍历了. 一个从头开始遍历, 一个从尾开始遍历.

同时, 为了标记哪个节点已经被遍历过了, 要创建2个map, beginVisited和endVisited. 如果某个节点从头开始遍历过了, 就把他放到beginVisit里面, 对应的值为到beginWord的距离. 对于endVisited同理.

同时要写一个辅助函数`int visitWord(Queue<Pair<String, Integer>> currQ, Map<String, Integer> currMap, Map<String, Integer> anotherMap)`

这个函数的作用是 以当前的currQ为队列, 从队列中取出来一个值, 这个值的所有通用状态对应的字符串(即所有变换一个字符可以得到的字符串)记为集合S, 如果S里面至少有一个值在anotherMap里面, 就说明当前的遍历首位相接了. 直接返回两端的距离之和就可以了.

如果S里面的值都不在anotherMap里, 说明还没到可以和另外一个遍历相接. 就按照普通的BFS更新currQ和currMap. 返回-1.

`visitWord(qBegin, beginVisited, endVisited);`和`visitWord(qEnd, endVisited, beginVisited);`在一个循环里先后调用, 往中间凑.

```java
class Solution {
    public Map<String, Set<String>> map;
    public Map<String, Integer> beginVisited = new HashMap<>();
    public Map<String, Integer> endVisited = new HashMap<>();
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        if(!wordList.contains(endWord))
            return 0;
        init(wordList);
        Queue<Pair<String, Integer>> qBegin = new ArrayDeque<>();
        Queue<Pair<String, Integer>> qEnd = new ArrayDeque<>();
        qBegin.offer(new Pair<String, Integer>(beginWord, 1));
        beginVisited.put(beginWord,1);
        qEnd.offer(new Pair<String, Integer>(endWord, 1));
        endVisited.put(endWord,1);
        while(!qBegin.isEmpty() && !qEnd.isEmpty())
        {
            int ans = visitWord(qBegin, beginVisited, endVisited);
            if(ans > -1)
            {
                return ans;
            }
            ans = visitWord(qEnd, endVisited, beginVisited);
            if(ans > -1)
            {
                return ans;
            }
        }
        return 0;
    }

    private void init(List<String> wordList)
    {
        map = new HashMap<>();
        for(String str : wordList)
        {
            for(int i = 0; i < str.length(); ++i)
            {
                String wildCard_s = str.substring(0,i) + "*" + str.substring(i + 1, str.length());
                map.putIfAbsent(wildCard_s, new HashSet<String>());
                map.get(wildCard_s).add(str);
            }
        }
    }

    private int visitWord(Queue<Pair<String, Integer>> currQ, Map<String, Integer> currMap, Map<String, Integer> anotherMap)
    {
        Pair<String, Integer> pair = currQ.poll();
        String str = pair.getKey();
        Integer value = pair.getValue();
        for(int i = 0; i < str.length(); ++i)
        {
            String wildCard_s = str.substring(0,i) + "*" + str.substring(i+1, str.length());
            if(map.get(wildCard_s) == null)
                continue;
            for(String diff1char_s : map.get(wildCard_s))
            {
                if(anotherMap.containsKey(diff1char_s))
                {
                    return anotherMap.get(diff1char_s) + value;
                }
                if(!currMap.containsKey(diff1char_s))
                {
                    currMap.put(diff1char_s, value + 1);
                    currQ.offer(new Pair<String, Integer>(diff1char_s, value + 1));
                }
            }
        }
        return -1;
    }
}
```

