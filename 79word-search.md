Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example:**

```
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

Given word = "ABCCED", return true.
Given word = "SEE", return true.
Given word = "ABCB", return false.
```

 

**Constraints:**

- `board` and `word` consists only of lowercase and uppercase English letters.
- `1 <= board.length <= 200`
- `1 <= board[i].length <= 200`
- `1 <= word.length <= 10^3`

## 1 暴力递归(超时)

对于一个字符串, 和一个开始位置, 若开始位置与字符串第0个字符相等(不相等返回false), 则判断开始位置的四周是否和字符串第1个字符相等(不相等返回false), 再判断开始位置四周的四周是不是和第2个位置相等, 依次类推. 如果一直到字符串最后都有相等的, 那就返回true.同时要创建一个isMarked数组防止一个元素被纳入两次.

**但是这种方法复杂度很高. 每一次字符相等后需要判断接下来前后左右4个位置的字符和下一个字符是否相等. 最坏条件下复杂度为O(4\^n), n为字符串长度 **

```c++
class Solution {
public:
    bool exist(vector<vector<char>>& board, string word) {
        vector<vector<bool>> isMarked(board.size(),vector<bool>(board.front().size(),false));
        for(int i = 0; i < board.size(); ++i)
        {
            for(int j = 0; j < board.front().size(); ++j)
            {
                bool tmp = exist(pair<int,int>({i,j}), isMarked, board, word.begin(), word.end());
                if(tmp) return true;
            }
        }
        return false;
    }
private:
    bool exist(const pair<int, int>& beginLocation, vector<vector<bool>> isMarked, const vector<vector<char>>& board, string::iterator begin, string::iterator end)
    {
        if(begin == end) return true;  // empty string always matches
        const int i = beginLocation.first;
        const int j = beginLocation.second;
        if(isOutOfBoundary(board, i, j)) return false;
        // use Array isMarked avoid using one element two times
        if(isMarked[i][j] || board[i][j] != *begin) return false;
        if(board[i][j] == *begin)
        {
            isMarked[i][j] = true;
            bool isLeftAvailable = exist(pair<int, int>({i, j - 1}), isMarked, board, begin + 1, end);
            bool isRightAvailable = exist(pair<int, int>({i, j + 1}), isMarked, board, begin + 1, end);
            bool isUpAvailable = exist(pair<int, int>({i - 1, j}), isMarked, board, begin + 1, end);
            bool isDownAvailable = exist(pair<int, int>({i + 1, j}), isMarked, board, begin + 1, end);
            return isLeftAvailable || isRightAvailable || isUpAvailable || isDownAvailable;
        }
        else
            throw string("In no case you could enter this block. Logic Error!");
    }
    inline bool isOutOfBoundary(const vector<vector<char>>& board, int i, int j)
    {
        return i >= board.size() || i < 0 || j >= board.front().size() || j < 0;
    }
};
```

## 2 递归2

一开始超时我还以为是算法的问题, 想了好久找一个最坏时间复杂度低于O(4^n)的算法, 没想出来. 后来看了答案之后发现这样做是对的, 只不过递归的时候要注意细节, 尽可能的减少时间复杂度.

**第一个细节就是一定要剪枝剪的彻底!**

在递归1中, 是这样递归的

```c++
bool isLeftAvailable = exist(pair<int, int>({i, j - 1}), isMarked, board, begin + 1, end);
bool isRightAvailable = exist(pair<int, int>({i, j + 1}), isMarked, board, begin + 1, end);
bool isUpAvailable = exist(pair<int, int>({i - 1, j}), isMarked, board, begin + 1, end);
bool isDownAvailable = exist(pair<int, int>({i + 1, j}), isMarked, board, begin + 1, end);
return isLeftAvailable || isRightAvailable || isUpAvailable || isDownAvailable;
```

这样就意味着, 如果算出来isLeftAvailable是可行的, 下面还会继续算isRightAvailable, isUpAvailable, isDownAvailable. 但是可以直接返回true即可. 不需要再算后面三个了. 未改进时最坏的情况下要多花3倍的时间!

```c++
bool isLeftAvailable = exist(pair<int, int>({i, j - 1}), isMarked, board, begin + 1, end);
if(isLeftAvailable) return true;
bool isRightAvailable = exist(pair<int, int>({i, j + 1}), isMarked, board, begin + 1, end);
if(isRightAvailable) return true;
bool isUpAvailable = exist(pair<int, int>({i - 1, j}), isMarked, board, begin + 1, end);
if(isUpAvailable) return true;
bool isDownAvailable = exist(pair<int, int>({i + 1, j}), isMarked, board, begin + 1, end);
if(isDownAvailable) return true;
return false;
```

**第二个细节就是一定要传引用, 不要传值!**

在递归1中是这样设计递归函数的                                                                                         ⬇

```c++
bool exist(const pair<int, int>& beginLocation, vector<vector<bool>> isMarked, const vector<vector<char>>& board, string::iterator begin, string::iterator end)
```

递归2                                                                                                                                       ⬇

```c++
bool exist(const pair<int, int>& beginLocation, vector<vector<bool>>& isMarked, const vector<vector<char>>& board, string::iterator begin, string::iterator end)
```

不断的复制isMarked非常耗费时间. 

当时想的是因为每个函数都可能对isMarked修改, 为了不让不同的递归之间产生影响, 索性就传值了. 但是这么做费时间.

**实际上传引用也能使不同的递归之间不产生影响, 只需要递归完恢复原来的值即可**

```c++
isMarked[i][j] = true;
bool isLeftAvailable = exist(pair<int, int>({i, j - 1}), isMarked, board, begin + 1, end);
if(isLeftAvailable) return true;
bool isRightAvailable = exist(pair<int, int>({i, j + 1}), isMarked, board, begin + 1, end);
if(isRightAvailable) return true;
bool isUpAvailable = exist(pair<int, int>({i - 1, j}), isMarked, board, begin + 1, end);
if(isUpAvailable) return true;
bool isDownAvailable = exist(pair<int, int>({i + 1, j}), isMarked, board, begin + 1, end);
if(isDownAvailable) return true;
isMarked[i][j] = false;
return false;  // <-- restore array isMarked
```

即在返回false之前将遍历过的位置恢复为false, 这样上一个递归不会影响下一个递归.

这两个细节缺一不可!!! 少一个都会超时

```c++
class Solution {
public:
    bool exist(vector<vector<char>>& board, string word) {
        vector<vector<bool>> isMarked(board.size(),vector<bool>(board.front().size(),false));
        for(int i = 0; i < board.size(); ++i)
        {
            for(int j = 0; j < board.front().size(); ++j)
            {
                bool tmp = exist(pair<int,int>({i,j}), isMarked, board, word.begin(), word.end());
                if(tmp) return true;
            }
        }
        return false;
    }
private:
    bool exist(const pair<int, int>& beginLocation, vector<vector<bool>>& isMarked, const vector<vector<char>>& board, string::iterator begin, string::iterator end)
    {
        if(begin == end) return true;  // empty string always matches
        const int i = beginLocation.first;
        const int j = beginLocation.second;
        if(isOutOfBoundary(board, i, j)) return false;
        // use Array isMarked avoid using one element two times
        if(isMarked[i][j] || board[i][j] != *begin) return false;
        if(board[i][j] == *begin)
        {
            isMarked[i][j] = true;
            bool isLeftAvailable = exist(pair<int, int>({i, j - 1}), isMarked, board, begin + 1, end);
            if(isLeftAvailable) return true;
            bool isRightAvailable = exist(pair<int, int>({i, j + 1}), isMarked, board, begin + 1, end);
            if(isRightAvailable) return true;
            bool isUpAvailable = exist(pair<int, int>({i - 1, j}), isMarked, board, begin + 1, end);
            if(isUpAvailable) return true;
            bool isDownAvailable = exist(pair<int, int>({i + 1, j}), isMarked, board, begin + 1, end);
            if(isDownAvailable) return true;
            isMarked[i][j] = false;
            return false;
        }
        else
            throw string("In no case you could enter this block. Logic Error!");
    }
    inline bool isOutOfBoundary(const vector<vector<char>>& board, int i, int j)
    {
        return i >= board.size() || i < 0 || j >= board.front().size() || j < 0;
    }
};
```

