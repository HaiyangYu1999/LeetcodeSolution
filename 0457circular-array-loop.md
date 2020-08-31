You are given a **circular** array `nums` of positive and negative integers. If a number *k* at an index is positive, then move forward *k* steps. Conversely, if it's negative (-*k*), move backward *k* steps. Since the array is circular, you may assume that the last element's next element is the first element, and the first element's previous element is the last element.

Determine if there is a loop (or a cycle) in `nums`. A cycle must start and end at the same index and the cycle's length > 1. Furthermore, movements in a cycle must all follow a single direction. In other words, a cycle must not consist of both forward and backward movements.

 

**Example 1:**

```
Input: [2,-1,1,2,2]
Output: true
Explanation: There is a cycle, from index 0 -> 2 -> 3 -> 0. The cycle's length is 3.
```

**Example 2:**

```
Input: [-1,2]
Output: false
Explanation: The movement from index 1 -> 1 -> 1 ... is not a cycle, because the cycle's length is 1. By definition the cycle's length must be greater than 1.
```

**Example 3:**

```
Input: [-2,1,-1,-2,-2]
Output: false
Explanation: The movement from index 1 -> 2 -> 1 -> ... is not a cycle, because movement from index 1 -> 2 is a forward movement, but movement from index 2 -> 1 is a backward movement. All movements in a cycle must follow a single direction.
```

 

**Note:**

1. -1000 ≤ nums[i] ≤ 1000
2. nums[i] ≠ 0
3. 1 ≤ nums.length ≤ 5000

## 快慢指针

个人感觉这个题不难, 就是麻烦, 条件多! 中心思想只有将数组看成是链表, 利用快慢指针判断.

首先考虑对于数组中任意一个开始的index看作链表的头节点, 如果通过快慢指针的方法确定了有环, 那么直接可以返回true了.

但是如果没有找到环, 比如, 我们从a开始寻找(将a看成头节点), 寻找路径为a->b->c->d->e, 当遍历到e的时候发现快指针的前进方向和一开始的前进方向相反了(例如[-2,1,-1,-2,-2], The movement from index 1 -> 2 -> 1 -> ... is not a cycle), 不满足题意, 返回false后, 下一次从b开始寻找的时候就不需要再调用一次`hasCircleFromIndex(nums,b)`了，**因为如果再次从b作为头节点开始找的话，也只能是走b->c->d->e的寻找路径，最后还是会发现e出现了问题要返回false**。 

所以，对每一次经过遍历的所有数组元素都要做一个标记，当开始的index已经被标记了，就说明沿着这条路走肯定会false。直接跳过，寻找下一个没有被标记的index再次寻找即可。

> 如果不标记,直接暴力寻找, 考虑[1,1,1,1,1,1,1,1,9]这样的数组, 每一次开始的index都会用双指针前进到最后才发现不行, 复杂度为O(n^2)! 而如果标记了前面的1, 则可降为线性复杂度

但是**标记的时候要用慢指针标记，慢指针遍历过的才标记，而不是用快指针，否则会出现错误**，下面举例说明

> 对于**[-1,1,2]** ， 我们先将-1作为头节点，第一次迭代，快指针遍历了-1 -> 2 -> 1，如果把2 和 1都标记了，那么就不会返回true了。如果用慢指针，那么会只标记2，再次选取index=1的时候就会判断为true。 但是这里我感觉好奇怪，明明从2出发可以找到一个circle，但是却会被标记。更稳妥的方法是先判断fast的方向对不对，如果不对，不标记slow，直接返回false，方向对再标记slow。这样可以确保任意一个被标记的index出发都不会找到circle。 但是前面那种先标记的方法也能ac，而且速度更快。(代码中/*注释的地方)

从nums[0]遍历到nums[size-1], 对于每一个元素作为index, 如果被标记了就直接跳过,没被标记就从这个index作为头节点找是否存在circle.最后返回查找结果

时间复杂度分析: 因为每一次while循环都会标记一个慢指针遍历的数, 所以**所有while花的时间等于标记慢指针的时间**. 而所有的慢指针最多有n个, 故时间复杂度为O(n)

还有需要注意的地方是, c++的负数取模和通常认知的取模不一样, 所以要重新定义.

> [-1, -1, -1]中, index = -1, 往前退1步应该为 -1 % 3 = 2, 但是c++的%运算符会返回-1.  需要重新定义
>
> `auto mod = []\(int a, int b){return (a % b >= 0) ? a % b : a % b + b;}`

```c++
class Solution {
public:
    bool circularArrayLoop(vector<int>& nums) {
        //bool result = false;
        for(int i = 0; i < nums.size(); ++i)
        {
            if(isMarked(nums[i]))
                continue;
            if(hasCircleFromIndex(nums,i))
                return true;
        }
        return false;
    }
private:
    const int M = 3000;
    const int inf = -1000;
    const int sup = 1000;
    bool hasCircleFromIndex(vector<int>& nums, int index)
    {
        int fast = index;
        int slow = index;
        //initial direction right is true, left is false    
        bool direction = (undoMark(nums[index]) > 0);  
        do
        {
            fast = mod(fast + undoMark(nums[fast]), nums.size());
            bool fastDirection1 = (undoMark(nums[fast]) > 0);
            fast = mod(fast + undoMark(nums[fast]), nums.size());
            bool fastDirection2 = (undoMark(nums[fast]) > 0);
            slow = mod(slow + undoMark(nums[slow]), nums.size());
            
            nums[slow] = mark(nums[slow]);
            if(fastDirection1 != direction || fastDirection2 != direction)
                return false;
            
            /* use this one is safer but slower than above
            if(fastDirection1 != direction || fastDirection2 != direction)
                return false;
            nums[slow] = mark(nums[slow]);
            */
        }while(fast != slow);
        
        //check if has only one node in circle
        if(mod(slow + undoMark(nums[slow]), nums.size())== slow)
            return false;
        return true;
    }
    
    int mark(int a)
    {
        return (a >= M + inf) ? a : (a + M);
    }
    
    int undoMark(int a)
    {
        return (a >= M + inf) ? (a - M) : a;
    }
    
    bool isMarked(int a)
    {
        return a >= M + inf;
    }
    
    int mod(int a, int b)    // modify a % b
    {
        return (a % b >= 0) ? a % b : a % b + b;
    }
};
```
