\11. Container With Most Water

Medium

6231587Add to ListShare

Given *n* non-negative integers *a1*, *a2*, ..., *an* , where each represents a point at coordinate (*i*, *ai*). *n* vertical lines are drawn such that the two endpoints of line *i* is at (*i*, *ai*) and (*i*, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

**Note:** You may not slant the container and *n* is at least 2.

 

![img](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.

 

**Example:**

```
Input: [1,8,6,2,5,4,8,3,7]
Output: 49
```



## 1 暴力算法(超时)

```c++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int max = 0;
        for(int i = 0; i < height.size(); ++i)
        {
            for(int j = i + 1; j < height.size(); ++j)
            {
                int tmp = (j - i) * min(height[i],height[j]);
                if(tmp > max)
                    max = tmp;
            }
        }
        return max;
    }
};
```

## 2 贪婪算法

我们发现, 要想容量最大, 要么就两个bar之间的宽度大,  要么bar的高度大. 

可以先计算出宽度最大的容量(let i:=1 j:=length-1), 然后再让i,j向中间缩进寻找高度更高的bar，判断是否超过刚才两个bar宽度比较大的容量。

向中间缩进是有技巧的，对于两个bar height[i]和height[j]，一定要短的bar向中间缩进，否则可能会错过最优解。

+ 例如，heights = [2, 6, 5, 5, 5, 4]中, 当i = 0; j = 5;时，如果将长的bar (height[5]=4)向中间移动,移动后j=4，这样就再也寻找不到最优解(i=1,j=5,volume=16)了。
+ 如果移动短的bar (height[0]=2), 移动后i=1. 这样也错过了某些解。比如(i=0,j=4,),(i=0,j=3)等。但是这些解不可能比原来的解大(i = 0; j = 5, volume = 10)，这是因为错过的这些解长度不可能比原来的解长， 高度也不可能比原来的解高(即使j对应的bar高，因为i对应的bar比j对应的bar低，所以总体高度也不可能大于i对应的bar)。
+ 也正因为这些错过的解不可能是最优解，所以减少了许多计算和比较，将复杂度降为O(n)。

```c++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int volume = 0;
        int i = 0;
        int j = height.size() - 1;
        while(i < j)
        {
            volume = max(volume,(j-i) * min(height[i],height[j]));
            if(height[i] < height[j])
            {
                ++i;
            }
            else
            {
                --j;
            }
        }
        return volume;
    }
};
```

