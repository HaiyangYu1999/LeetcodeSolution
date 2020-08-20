Suppose you have a long flowerbed in which some of the plots are planted and some are not. However, flowers cannot be planted in adjacent plots - they would compete for water and both would die.

Given a flowerbed (represented as an array containing 0 and 1, where 0 means empty and 1 means not empty), and a number **n**, return if **n** new flowers can be planted in it without violating the no-adjacent-flowers rule.

**Example 1:**

```
Input: flowerbed = [1,0,0,0,1], n = 1
Output: True
```



**Example 2:**

```
Input: flowerbed = [1,0,0,0,1], n = 2
Output: False
```



**Note:**

1. The input array won't violate no-adjacent-flowers rule.
2. The input array size is in the range of [1, 20000].
3. **n** is a non-negative integer which won't exceed the input array size.

## 遍历

从左到右依次判断该位置是否能种花`flowerbed[i - 1] == 0 && flowerbed[i + 1] == 0 && flowerbed[i] == 0`. 若能, ++capacity,并且在该位置设置为1. 最后判断capacity和n的大小即可.

注意当n == 0 和 n == flowerbed.size() - 1 时判断条件改为flowerbed[i + 1] == 0 && flowerbed[i] == 0 和 flowerbed[i - 1] == 0 && flowerbed[i] == 0

```c++
class Solution {
public:
    bool canPlaceFlowers(vector<int>& flowerbed, int n) {
        if(n == 0)
            return true;
        if(flowerbed.empty())
            return false;
        if(flowerbed.size() == 1)
        {
            if(flowerbed[0] == 1)
                return false;
            else
            {
                if(n == 1) 
                    return true;
                else
                    return false;
            }
        }
        int capacity = 0;
        for(int i = 0; i < flowerbed.size(); ++i)
        {
            if(i == 0)
            {
                if(flowerbed[i + 1] == 0 && flowerbed[i] == 0)
                {
                    ++capacity;
                    flowerbed[i] = 1;
                }
            }
            else if(i == flowerbed.size() - 1)
            {
                if(flowerbed[i - 1] == 0 && flowerbed[i] == 0)
                {
                    ++capacity;
                    flowerbed[i] = 1;
                }
            }
            else
            {
                if(flowerbed[i - 1] == 0 && flowerbed[i + 1] == 0 && flowerbed[i] == 0)
                {
                    ++capacity;
                    flowerbed[i] = 1;
                }
            }
        }
        return capacity >= n;
    }
};
```

