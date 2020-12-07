Write an efficient algorithm that searches for a value in an *m* x *n* matrix. This matrix has the following properties:

- Integers in each row are sorted in ascending from left to right.
- Integers in each column are sorted in ascending from top to bottom.

**Example:**

Consider the following matrix:

```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```

Given target = `5`, return `true`.

Given target = `20`, return `false`.

## 1. 分治

**这是我看答案之前想出来的方法. 这个方法贼麻烦.** 但是复杂度并不高

对于一个矩阵, 我们考虑对角线元素.

对对角线元素组成的数组进行二分查找

```
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
```

例如在上面这个矩阵中, 我们要寻找15. 对角线数组为[1, 5, 9, 17, 30]. 15位于9和17之间. (如果对角线中含有15, 就直接返回15即可. 不用继续查找了)

那么可以把矩阵分为4部分, 左上角为小于等于9的, 右下角为大于17的, 左下角和右上角的元素可能是9到17中间的, 也可能大于17. 所以元素15只有可能出现在左下和右上的子矩阵中.

>   [**1,   4,  7**,  ==11, 15==],
>   [**2,   5,  8**,  ==12, 19==],
>   [**3,   6,  9,**  ==16, 22==],
>   [==10, 13, 14==, **17, 24**],
>   [==18, 21, 23==, **26,** **30**]

所以, 接下来就可以从这两个子矩阵中递归地查找元素15了. 递归边界为矩阵退化为行向量或列向量. 这时只需要用普通的二分查找即可.

这里还有个要注意的点, 就是对角线的二分查找问题.

如果矩阵是不是方阵, 那么如果target小于对角线第一个元素, target肯定小于这个子矩阵的所有元素. 但是, 如果target大于对角线最后一个元素, target却不一定大于这个子矩阵所有元素. 

>  [**1**,   4,  7, ==11, 15==],
>   [2,   **5**,  8, ==12, 19==],
>   [3,   6,  **9**, ==16, 22==],           如果target = 10, 大于对角线最后一个元素9, 就要从后面的元素中开始找

> [**1**,   4,  7],
> [2,   **5**,  8],
> [3,   6,  **9**],
> [==10, 13, 14==],
> [==18, 21, 23==]                   如果target = 10, 大于对角线最后一个元素9, 就要从后面的元素中开始找

> [1,   4,  7]
> [2,   5,  8]
> [3,   6,  9]                    如果矩阵是方阵, target = 10 大于对角线最后一个元素9, 那么矩阵中不可能存在target

复杂度分析: 为了简单起见, 设矩阵为方阵, 有`n`个元素. **注意, 这里是假设矩阵的元素个数为n, 而不是矩阵的维数为n.** **矩阵的维数应该为`sqrt(n) * sqrt(n)`**

那么每一次对size为`sqrt(n)`的矩阵进行查找, 就需要先进行一次对角线的二分查找. 花费`log(sqrt(n))`, 然后对2个size为`n / 4`的矩阵进行查找.

故有`T(n) = log(sqrt(n)) + 2T(n/4)`

做个换元, `n = 2 ^ k`, 得到`T(2^k) = k/2 + 2*T(2^(k-2))` 然后经过漫长的推到, 得到结果是`T(n) ~ sqrt(n)`.

即时间复杂度和矩阵大小成正比例.

如果矩阵是a * b的话, 复杂度应该为`sqrt(a*b)`

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if(matrix == null || matrix.length == 0 || matrix[0].length == 0)
            return false;
        return searchMatrix(matrix, target, 0, matrix.length - 1, 0, matrix[0].length - 1);
    }
    private boolean searchMatrix(int[][] matrix, int target, int beginRow, int endRow, int beginColumn, int endColumn)
    {
        if(beginRow == endRow)
        {
            return binarySearchRow(matrix, target, beginColumn, endColumn, beginRow);
        }
        else if(beginColumn == endColumn)
        {
            return binarySearchColumn(matrix, target, beginRow, endRow, beginColumn);
        }
        
        int m = endRow - beginRow + 1;
        int n = endColumn - beginColumn + 1;
        int diagonalLen = Math.min(m, n);
        int minDiagonal = matrix[beginRow][beginColumn];
        int maxDiagonal = matrix[beginRow + diagonalLen - 1][beginColumn + diagonalLen - 1];
        
        if(target < minDiagonal)
            return false;
        else if(target > maxDiagonal)
        {
            if(m == n)
                return false;
            else if(m > n)
                return searchMatrix(matrix, target, beginRow + n, endRow, beginColumn, endColumn);
            else
                return searchMatrix(matrix, target, beginRow, endRow, beginColumn + m, endColumn);
        }
        else
        {
            int i = 0;
            int j = diagonalLen - 1;
            while(i <= j)
            {
                int mid = i + (j - i) / 2;
                if(matrix[beginRow + mid][beginColumn + mid] == target)
                    return true;
                else if(matrix[beginRow + mid][beginColumn + mid] < target)
                    i = mid + 1;
                else
                    j = mid - 1;
            }
            return searchMatrix(matrix, target, beginRow, beginRow + j, beginColumn + i, endColumn) || searchMatrix(matrix, target, beginRow + i, endRow, beginColumn, beginColumn + j);
        }
    }
    
    private boolean binarySearchRow(int[][] matrix, int target, int beginColumn, int endColumn, int row)
    {
        int begin = beginColumn;
        int end = endColumn;
        while(begin <= end)
        {
            int mid = begin + (end - begin) / 2;
            if(matrix[row][mid] == target)
                return true;
            else if(matrix[row][mid] < target)
            {
                begin = mid + 1;
            }
            else
            {
                end = mid - 1;
            }
        }
        return false;
    }
    
    private boolean binarySearchColumn(int[][] matrix, int target, int beginRow, int endRow, int column)
    {
        int begin = beginRow;
        int end = endRow;
        while(begin <= end)
        {
            int mid = begin + (end - begin) / 2;
            if(matrix[mid][column] == target)
                return true;
            else if(matrix[mid][column] < target)
            {
                begin = mid + 1;
            }
            else
            {
                end = mid - 1;
            }
        }
        return false;
    }
}
```

## 2. 线性搜索法

**这种方法真的是又简单又快! 但是感觉如果第一次做应该想不出来.**

从左下或右上开始找, 不妨假设从左下开始.

找的时候只能向上或向右. 原因如下, 记当前的坐标为(row, column)初始为左下角. 

>   [1,   4,  7, 11, 15],
>   [2,   5,  8, 12, 19],
>   [3,   6,  9, 16, 22],
>   [10, 13, 14, 17, 24],            target = 12, curr = 18 > target.  因为curr是最后一行的最小值
>   [==18, 21, 23, 26, 30==]             所以最底下一整行都不可能是target, 全部排除掉. curr向上移动变为10

>   [==1==,   4,  7, 11, 15],
>   [==2==,   5,  8, 12, 19],
>   [==3==,   6,  9, 16, 22],
>   [==10==, 13, 14, 17, 24],            target = 12, curr = 10 < target. 因为curr是第一行的最大值
>   [==18, 21, 23, 26, 30==]             所以最左侧一整列都不可能是target, 全部排除掉. curr向右移动变为13

>   [==1==,   4,  7, 11, 15],
>   [==2==,   5,  8, 12, 19],
>   [==3==,   6,  9, 16, 22],
>   [==10==, ==13, 14, 17, 24==],            target = 12, curr = 13 > target. 因为curr是倒数第二行的最小值
>   [==18, 21, 23, 26, 30==]             所以倒数第二行都不可能是target, 全部排除掉. curr向上移动变为6

>  [==1==,   ==4==,  7, 11, 15],
>   [==2==,   ==5==,  8, 12, 19],
>   [==3==,   ==6==,  9, 16, 22],
>   [==10==, ==13, 14, 17, 24==],            target = 12, curr = 6 < target. 因为curr是第二列的最大值
>   [==18, 21, 23, 26, 30==]             所以第二列都不可能是target, 全部排除掉. curr向右移动变为9

>  [==1==,   ==4==,  ==7==, 11, 15],
>   [==2==,   ==5==,  ==8==, 12, 19],
>   [==3==,   ==6==,  ==9==, 16, 22],
>   [==10==, ==13, 14, 17, 24==],            target = 12, curr = 9 < target. 因为curr是第三列的最大值
>   [==18, 21, 23, 26, 30==]             所以第三列都不可能是target, 全部排除掉. curr向右移动变为16

>  [==1==,   ==4==,  ==7==, 11, 15],
>   [==2==,   ==5==,  ==8==, 12, 19],
>   [==3==,   ==6==,  ==9==, ==16, 22==],
>   [==10==, ==13, 14, 17, 24==],            target = 12, curr = 16 > target. 因为curr是第三行的最大值
>   [==18, 21, 23, 26, 30==]             所以第三行都不可能是target, 全部排除掉. curr向上移动变为12, 找到解了!

时间复杂度O(a + b). 虽然时间复杂度一样, 但是这种方法空间复杂度是O(1). 并且代码**极其简洁**!!!

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if(matrix == null || matrix.length == 0 || matrix[0].length == 0)
            return false;
        int m = matrix.length;
        int n = matrix[0].length;
        int i = m - 1;
        int j = 0;
        while(i > -1 && j < n)
        {
            int curr = matrix[i][j];
            if(curr == target)
                return true;
            else if(curr < target)
                ++j;
            else 
                --i;
        }
        return false;
    }
}
```

