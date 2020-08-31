Given an integer array, find three numbers whose product is maximum and output the maximum product.

**Example 1:**

```
Input: [1,2,3]
Output: 6
```

 

**Example 2:**

```
Input: [1,2,3,4]
Output: 24
```

 

**Note:**

1. The length of the given array will be in range [3,104] and all elements are in the range [-1000, 1000].
2. Multiplication of any three numbers in the input won't exceed the range of 32-bit signed integer.

## 逻辑分析

很容易的就能推测出来最大值肯定是三个最大的数相乘, 或者是一个最大的和两个最小的相乘(考虑存在绝对值很大的负数). 扫描一遍找出来top3最大的和last2最小的即可

```c++
class Solution {
public:
    int maximumProduct(vector<int>& nums) {
        priority_queue<int, deque<int>, greater<int>> top3;
        top3.push(nums[0]);
        top3.push(nums[1]);
        top3.push(nums[2]);
        for(int i = 3; i < nums.size(); ++i)
        {
            if(nums[i] > top3.top())
            {
                top3.pop();
                top3.push(nums[i]);
            }
        }
        
        priority_queue<int, deque<int>, less<int>> last2;
        last2.push(nums[0]);
        last2.push(nums[1]);
        for(int i = 2; i < nums.size(); ++i)
        {
            if(nums[i] < last2.top())
            {
                last2.pop();
                last2.push(nums[i]);
            }
        }
        
        int smallest2nd = last2.top();
        last2.pop();
        int smallest = last2.top();
        int largest3rd = top3.top();
        top3.pop();
        int largest2nd = top3.top();
        top3.pop();
        int largest = top3.top();
        
        return max(largest * largest2nd * largest3rd, largest * smallest2nd * smallest);
        
    }
};
```

