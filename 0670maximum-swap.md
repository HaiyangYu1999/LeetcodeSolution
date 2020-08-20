Given a non-negative integer, you could swap two digits **at most** once to get the maximum valued number. Return the maximum valued number you could get.

**Example 1:**

```
Input: 2736
Output: 7236
Explanation: Swap the number 2 and the number 7.
```



**Example 2:**

```
Input: 9973
Output: 9973
Explanation: No swap.
```



**Note:**

1. The given number is in the range [0, 10^8]

## 贪心

首先肯定要把这个整数变成一个数组再处理的. 我这个题用的`deque<int>`, 但是看了网上的解答觉得转成`string`更好, 空间由4n字节降为n字节, n为这个数的位数. 

思路很简单, 从左到右开始找, 找到第一个不是递减的值记为nums[j], 再从j到最后一位中寻找一个最大的值(如果有多个最大的值, 取后面的那个). 找到最大的值M之后, 数组中的元素从左到右和M比较, 如果大于等于M, 就向后继续找, 直到找到一个比M小的值, 这个值和M交换, 即为所求

举例: 

>  9870  因为一直递减, 所以不用交换
>
>  9708, 第一个不是递减的元素是0, 从元素0开始到最后一个元素的范围, 最大的是8, M=8, 从左到右找, 第一个小于8的是7, 所以7和8交换得到9807
>
> 1993, 第一个不是递减的元素是1, 从元素1到最后一个元素的范围中, 最大的是9, 但是这时候要选择靠后面的9, 用靠后面的9与1交换, 得到9913, 否则将会得到错误的解9193.

```c++
class Solution {
public:
    int maximumSwap(int num) {
        if(num < 10)
            return num;
        deque<int> nums = int2deque(num);
        int i;
        for(i = 0; i < nums.size() - 1; ++i)
        {
            if(nums[i] < nums[i+1])
                break;
        }
        if(i == nums.size() - 1)
            return num;
        else
        {
            deque<int>::iterator maxIt;
            int max = INT_MIN;
            for(deque<int>::iterator j = nums.begin() + i; j != nums.end(); ++j)
            {
                if(*j >= max)
                {
                    max = *j;
                    maxIt = j;
                }
            }
            deque<int>::iterator begin = nums.begin();
            while(*begin >= *maxIt)
                ++begin;
            swap(*begin, *maxIt);
        }
        return deque2int(nums);
    }
private:
    const int scaleSystemBaseInt2Array = 10;  // Decimal system
    const int scaleSystemBaseArray2Int = 10;  // Decimal system
    int deque2int(deque<int>& nums)
    {
        int a = 0;
        const int scaleSystemBase = 10;
        while(!nums.empty())
        {
            a = a * scaleSystemBaseArray2Int + nums.front();
            nums.pop_front();
        }
        return a;
    }
    deque<int> int2deque(int a)
    {
        deque<int> ans;
        do
        {
            ans.push_front(a % scaleSystemBaseInt2Array);
            a /= scaleSystemBaseInt2Array;
        }while(a > 0);
        return ans;
    }
};
```

