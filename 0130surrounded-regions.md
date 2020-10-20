Given a 2D board containing `'X'` and `'O'` (**the letter O**), capture all regions surrounded by `'X'`.

A region is captured by flipping all `'O'`s into `'X'`s in that surrounded region.

**Example:**

```
X X X X
X O O X
X X O X
X O X X
```

After running your function, the board should be:

```
X X X X
X X X X
X X X X
X O X X
```

**Explanation:**

Surrounded regions shouldn’t be on the border, which means that any `'O'` on the border of the board are not flipped to `'X'`. Any `'O'` that is not on the border and it is not connected to an `'O'` on the border will be flipped to `'X'`. Two cells are connected if they are adjacent cells connected horizontally or vertically.

## BFS

关键是读懂题意. 这题读懂题意之后就很好做了

对于内部的一个点'O', 我们判断某个这个块是否到达边界是不好判定的.

但是可以从边界开始找, 找到四周边界上所有O组成的块, 将他们置为P, 然后遍历二维数组, O变为X, P变为O.

```
X X X X
X O O X
X X O X
X O X X
```

```
X X X X
X O O X
X X O X
X P X X
```

```
X X X X
X X X X
X X X X
X O X X
```

```java
class Solution {
    public void solve(char[][] board) {
        if(board == null || board.length == 0)
            return;
        final int M = board.length;
        final int N = board[0].length;
        for(int j = 0; j < N; ++j)
        {
            setOtoP(board, 0, j);
            setOtoP(board, M - 1, j);
        }
        for(int i = 0; i < M; ++i)
        {
            setOtoP(board, i, 0);
            setOtoP(board, i, N - 1);
        }
        
        for(int i = 0; i < M; ++i)
        {
            for(int j = 0; j < N; ++j)
            {
                if(board[i][j] == 'O')
                    board[i][j] = 'X';
                else if(board[i][j] == 'P')
                    board[i][j] = 'O';
            }
        }
        
    }
    
    private void setOtoP(char[][] board, int i , int j)
    {
        if(isLocationValid(board, i, j) && board[i][j] == 'O')
        {
            board[i][j] = 'P';
            setOtoP(board, i + 1, j);
            setOtoP(board, i - 1, j);
            setOtoP(board, i, j + 1);
            setOtoP(board, i, j - 1);
        }
    }
    
    private boolean isLocationValid(char[][] board, int i, int j)
    {
        int M = board.length;
        int N = board[0].length;
        return i > -1 && i < M && j > -1 && j < N;
    }
}
```

