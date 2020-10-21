Determine if a `9 x 9` Sudoku board is valid. Only the filled cells need to be validated **according to the following rules**:

1. Each row must contain the digits `1-9` without repetition.
2. Each column must contain the digits `1-9` without repetition.
3. Each of the nine `3 x 3` sub-boxes of the grid must contain the digits `1-9` without repetition.

**Note:**

- A Sudoku board (partially filled) could be valid but is not necessarily solvable.
- Only the filled cells need to be validated according to the mentioned rules.

 

**Example 1:**

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

```
Input: board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
Output: true
```

**Example 2:**

```
Input: board = 
[["8","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
Output: false
Explanation: Same as Example 1, except with the 5 in the top left corner being modified to 8. Since there are two 8's in the top left 3x3 sub-box, it is invalid.
```

 

**Constraints:**

- `board.length == 9`
- `board[i].length == 9`
- `board[i][j]` is a digit or `'.'`.

## 1 3次遍历, 1个map

这个只需要构造一个hashmap即可.

遍历三次, 分别检查行, 列, 块是否满足标准. 

空间占用少, 时间占用多

```java
class Solution {
    Map<Character, Integer> auxMap = new HashMap<>();
    public boolean isValidSudoku(char[][] board) {
        boolean ans = true;
        for(int i = 0; i < 9; ++i)
        {
            ans = ans && checkRowValid(board, i);
            ans = ans && checkColumnValid(board, i);
        }
        for(int i = 0; i < 9; i = i + 3)
        {
            for(int j = 0; j < 9; j = j + 3)
            {
                ans = ans && checkBlockValid(board, i, i + 2, j, j + 2);
            }
        }
        return ans;
    }
    private boolean checkBlockValid(char[][] board, int iBegin, int iEnd, int jBegin, int jEnd)
    {
        auxMap.clear();
        for(int i = iBegin; i <= iEnd; ++i)
        {
            for(int j = jBegin; j <= jEnd; ++j)
            {
                auxMap.putIfAbsent(board[i][j], 0);
                auxMap.put(board[i][j], auxMap.get(board[i][j]) + 1);
            }
        }
        for(Map.Entry<Character, Integer> entry : auxMap.entrySet())
        {
            if(entry.getKey() != '.' && entry.getValue() > 1)
            {
                return false;
            }
        }
        return true;
    }
    private boolean checkRowValid(char[][] board, int row)
    {
        auxMap.clear();
        for(int j = 0; j < 9; ++j)
        {
            auxMap.putIfAbsent(board[row][j], 0);
            auxMap.put(board[row][j], auxMap.get(board[row][j]) + 1);
        }
        for(Map.Entry<Character, Integer> entry : auxMap.entrySet())
        {
            if(entry.getKey() != '.' && entry.getValue() > 1)
                return false;
        }
        return true;
    }
    private boolean checkColumnValid(char[][] board, int column)
    {
        auxMap.clear();
        for(int i = 0; i < 9; ++i)
        {
            auxMap.putIfAbsent(board[i][column], 0);
            auxMap.put(board[i][column], auxMap.get(board[i][column]) + 1);
        }
        for(Map.Entry<Character, Integer> entry : auxMap.entrySet())
        {
            if(entry.getKey() != '.' && entry.getValue() > 1)
                return false;
        }
        return true;
    }
}
```

## 2 1次遍历, 27个map

这里直接建立2维数组, 比hashmap更快.

```java
class Solution {
    // 0 - 8 is row0 to row8
    // 9 - 17 is column0 to column8
    // 18 - 26 is block0 to block8
    int[][] cnt = new int[27][9];
    public boolean isValidSudoku(char[][] board) {
        for(int i = 0; i < 9; ++i)
        {
            for(int j = 0; j < 9; ++j)
            {
                if(board[i][j] == '.')
                    continue;
                int tmp = board[i][j] - '1';
                cnt[i][tmp]++;
                cnt[9 + j][tmp]++;
                cnt[18 + getBlock(i,j)][tmp]++;
            }
        }
        for(int i = 0; i < 27; ++i)
        {
            for(int j = 0; j < 9; ++j)
            {
                if(cnt[i][j] > 1)
                    return false;
            }
        }
        return true;
    }
    
    private int getBlock(int i, int j)
    {
        return (i / 3) * 3 + (j / 3);
    }
}
```

