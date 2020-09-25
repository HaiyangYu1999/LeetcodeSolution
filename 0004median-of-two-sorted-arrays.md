Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

**Follow up:** The overall run time complexity should be `O(log (m+n))`.

 

**Example 1:**

```
Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.
```

**Example 2:**

```
Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
```

**Example 3:**

```
Input: nums1 = [0,0], nums2 = [0,0]
Output: 0.00000
```

**Example 4:**

```
Input: nums1 = [], nums2 = [1]
Output: 1.00000
```

**Example 5:**

```
Input: nums1 = [2], nums2 = []
Output: 2.00000
```

 

**Constraints:**

- `nums1.length == m`
- `nums2.length == n`
- `0 <= m <= 1000`
- `0 <= n <= 1000`
- `1 <= m + n <= 2000`
- `-106 <= nums1[i], nums2[i] <= 106`

## 1 线性复杂度

先根据总个数是奇数还是偶数来确定中位数的位置在哪里, 然后从最小的元素开始找, 找(m + n) / 2次即可. 复杂度O(m + n)

```c++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int len = nums1.size() + nums2.size();
        int cursor1 = 0;
        int cursor2 = 0;
        int median1 = 0;
        int median2 = 0;
        if(len % 2 == 0)
        {
            for(int i = 0; i < len; ++i)
            {
                int tmp;
                if(cursor1 == nums1.size())
                {
                    tmp = nums2[cursor2];
                    ++cursor2;
                }
                else if(cursor2 == nums2.size())
                {
                    tmp = nums1[cursor1];
                    ++cursor1;
                }
                else if(nums1[cursor1] > nums2[cursor2])
                {
                    tmp = nums2[cursor2];
                    ++cursor2;
                }
                else
                {
                    tmp = nums1[cursor1];
                    ++cursor1;
                }
                if(i == len / 2 - 1)
                    median1 = tmp;
                if(i == len / 2)
                {
                    median2 = tmp;
                    break;
                }
            }
            return ((double)median1 + (double) median2) / 2;
        }
        else
        {
            for(int i = 0; i < len; ++i)
            {
                int tmp;
                if(cursor1 == nums1.size())
                {
                    tmp = nums2[cursor2];
                    ++cursor2;
                }
                else if(cursor2 == nums2.size())
                {
                    tmp = nums1[cursor1];
                    ++cursor1;
                }
                else if(nums1[cursor1] > nums2[cursor2])
                {
                    tmp = nums2[cursor2];
                    ++cursor2;
                }
                else
                {
                    tmp = nums1[cursor1];
                    ++cursor1;
                }
                if(i == len / 2)
                {
                    median2 = tmp;
                    break;
                }
            }
            return (double)median2;
        }
    }
};
```

## 2 对数复杂度1

思路来源于 `https://leetcode-cn.com/problems/median-of-two-sorted-arrays/solution/xun-zhao-liang-ge-you-xu-shu-zu-de-zhong-wei-s-114/`,递归做的, 比较慢, 只超过33%.

```c++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        const int m = nums1.size();
        const int n = nums2.size();
        int pos1 = (m + n + 1) / 2;
        int pos2 = (m + n + 2) / 2;
        return (findKth(nums1, nums2, 0, 0, pos1) + findKth(nums1, nums2, 0, 0, pos2)) / 2.0;
    }
private:
    int findKth(const vector<int>& nums1, const vector<int>& nums2, int offset1, int offset2, int k)
    {
        if(offset1 == nums1.size())
            return nums2[offset2 + k - 1];
        if(offset2 == nums2.size())
            return nums1[offset1 + k - 1];
        if(k == 1)
            return min(nums1[offset1], nums2[offset2]);
        int mid = k / 2 - 1;
        int nums1Mid = mid + offset1;
        int nums2Mid = mid + offset2;
        bool isOutofIndex1 = false;
        bool isOutofIndex2 = false;
        int nums1Offset = offset1;
        int nums2Offset = offset2;
        if(nums1Mid > nums1.size() - 1)
        {
            nums1Mid = nums1.size() - 1;
            isOutofIndex1 = true;
            nums1Offset = nums1.size() - 1;
        }
        if(nums2Mid > nums2.size() - 1)
        {
            nums2Mid = nums2.size() - 1;
            isOutofIndex2 = true;
            nums2Offset = nums2.size() - 1;
        }
        
        if(nums1[nums1Mid] > nums2[nums2Mid])
        {
            if(isOutofIndex2)
            {
                k -= nums2.size() - offset2;
                offset2 = nums2.size();
            }
            else
            {
                offset2 += k/2;
                k -= k/2;
                
            }
        }
        else
        {
            if(isOutofIndex1)
            {
                k -= nums1.size() - offset1;
                offset1 = nums1.size();
            }
            else
            {
                offset1 += k/2;
                k -= k/2;
                
            }
        }
        return findKth(nums1, nums2, offset1, offset2, k);
    }
    
};
```

## 3 对数复杂度2

将上面的递归改成了循环.

```c++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        const int m = nums1.size();
        const int n = nums2.size();
        int pos1 = (m + n + 1) / 2;
        int pos2 = (m + n + 2) / 2;
        return (findKth(nums1, nums2, pos1) + findKth(nums1, nums2, pos2)) / 2.0;
    }
private:
    int findKth(const vector<int>& nums1, const vector<int>& nums2, int k)
    {
        int offset1 = 0;
        int offset2 = 0;
        int m = nums1.size();
        int n = nums2.size();
        while(true)
        {
            int mid = k / 2 - 1;
            //边界情况
            if(offset1 == m)
            {
                return nums2[offset2 + k - 1];
            }
            if(offset2 == n)
            {
                return nums1[offset1 + k - 1];
            }
            if(k == 1)
            {
                return min(nums1[offset1], nums2[offset2]);
            }
            //正常情况
            int mid1 = offset1 + mid;
            int mid2 = offset2 + mid;
            bool isOutOfIndex1 = false;
            bool isOutOfIndex2 = false;
            if(mid1 > m - 1)
            {
                mid1 = m - 1;
                isOutOfIndex1 = true;
            }
            if(mid2 > n - 1)
            {
                mid2 = n - 1;
                isOutOfIndex2 = true;
            }
            if(nums1[mid1] > nums2[mid2])
            {
                if(isOutOfIndex2)
                {
                    k -= n - offset2;
                    offset2 = n;
                }
                else
                {
                    offset2 += k/2;
                    k -= k/2;
                }
            }
            else
            {
                if(isOutOfIndex1)
                {
                    k -= m - offset1;
                    offset1 = m;
                }
                else
                {
                    offset1 += k/2;
                    k -= k/2;
                }
            }
        }
    }
    
};
```

