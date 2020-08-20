According to the [Wikipedia's article](https://en.wikipedia.org/wiki/Conway's_Game_of_Life): "The **Game of Life**, also known simply as **Life**, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."

Given a *board* with *m* by *n* cells, each cell has an initial state *live* (1) or *dead* (0). Each cell interacts with its [eight neighbors](https://en.wikipedia.org/wiki/Moore_neighborhood) (horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):

1. Any live cell with fewer than two live neighbors dies, as if caused by under-population.
2. Any live cell with two or three live neighbors lives on to the next generation.
3. Any live cell with more than three live neighbors dies, as if by over-population..
4. Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.

Write a function to compute the next state (after one update) of the board given its current state. The next state is created by applying the above rules simultaneously to every cell in the current state, where births and deaths occur simultaneously.

**Example:**

```
Input: 
[
  [0,1,0],
  [0,0,1],
  [1,1,1],
  [0,0,0]
]
Output: 
[
  [0,0,0],
  [1,0,1],
  [0,1,1],
  [0,1,0]
]
```

**Follow up**:

1. Could you solve it in-place? Remember that the board needs to be updated at the same time: You cannot update some cells first and then use their updated values to update other cells.
2. In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches the border of the array. How would you address these problems?

## 数组原地修改

虽然题目长, 但是真心不难. 

就是要原地记录下每个cell下一个时刻的状态, 并且不能覆盖当前的状态(因为算后面的cell的状态需要用到这个cell当前的状态)

既然只有0和1, 并且是int数组而不是bool数组, 所以可以在每个int中增加信息量

用二进制表示, 若下一个state变为dead, 就在最高位标记为1, 若下一个state变为alive 则在中间位标记为1, 最后一位仍然是当前的状态不变. 

> 例如, 现状态为1, 下一个状态死(101). 现状态为0, 下一个状态活(011)

而从标记的值中提取现状态的值只需要 `a = a & 1`即可.

 ```c++
class Solution {
public:
    void gameOfLife(vector<vector<int>>& board) {
        for(int i = 0; i < board.size(); ++i)
        {
            for(int j = 0; j < board.front().size(); ++j)
            {
                int aliveNeighbors = getAliveNeighbors(board, i, j);
                if(isCurrentAlive(board[i][j]) && aliveNeighbors < 2)
                {
                    board[i][j] = markDead(board[i][j]);
                }
                else if(isCurrentAlive(board[i][j]) && (aliveNeighbors == 2 || aliveNeighbors == 3))
                {
                    board[i][j] = board[i][j];
                }
                else if(isCurrentAlive(board[i][j]) && aliveNeighbors > 3)
                {
                    board[i][j] = markDead(board[i][j]);
                }
                else if(!isCurrentAlive(board[i][j]) && aliveNeighbors == 3)
                {
                    board[i][j] = markAlive(board[i][j]);
                }
            }
        }
     
        for(int i = 0; i < board.size(); ++i)
        {
            for(int j = 0; j < board.front().size(); ++j)
            {
                if(willDie(board[i][j]))
                    board[i][j] = 0;
                else if(willAlive(board[i][j]))
                    board[i][j] = 1;
            }
        }
        
    }
private:
    const int nowAliveMark = 1;
    const int nextAliveMark = 2;
    const int nextDeadMark = 4;
    int markDead(int a)
    {
        return a | nextDeadMark;
    }
    int markAlive(int a)
    {
        return a | nextAliveMark;
    }
    bool isCurrentAlive(int a)
    {
        return a & nowAliveMark; 
    }
    bool willDie(int a)
    {
        return a & nextDeadMark;
    }
    bool willAlive(int a)
    {
        return a & nextAliveMark;
    }
    
    bool isLocationValid(const vector<vector<int>>& board, int m, int n)
    {
        return m > -1 && m < board.size() && n > -1 && n < board.front().size();
    }
    
    int getAliveNeighbors(const vector<vector<int>>& board, int m, int n)
    {
        int aliveNumber = 0;
        vector<int> dx = {0, 0,-1, 1, 1, 1,-1,-1};
        vector<int> dy = {1,-1, 0, 0, 1,-1, 1,-1};
        for(int i = 0; i < dx.size(); ++i)
        {
            if(isLocationValid(board,m + dx[i], n + dy[i]) && isCurrentAlive(board[m+dx[i]][n+dy[i]]))
                ++aliveNumber;
        }
        return aliveNumber;
    }
};
 ```

