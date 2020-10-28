Shuffle a set of numbers without duplicates.

**Example:**

```
// Init an array with set 1, 2, and 3.
int[] nums = {1,2,3};
Solution solution = new Solution(nums);

// Shuffle the array [1,2,3] and return its result. Any permutation of [1,2,3] must equally likely to be returned.
solution.shuffle();

// Resets the array back to its original configuration [1,2,3].
solution.reset();

// Returns the random shuffling of array [1,2,3].
solution.shuffle();
```

## shuffle算法

要保证每一种可能性是公平的. 而所有的可能排列数为`n!`. 所以这就需要洗牌出来每一个排列的概率都是`1/n!`

可以这样, 

首先在数组中随便选一个值, 即从0到n-1随机选取一个下标, 将这个值作为结果的第一个. 这一次抽取的概率是`1/n`(n个里面抽1个)

然后把被选中的那个值移到数组最后, 数组最后的元素移动到被选中的位置. 这时候数组最后一个元素已经被选中. 从0到n-2的位置放置的是没被选中的元素. 这次就从0到n-2随机选取一个下边, 作为结果的第二个. 这次抽取的概率是1/(n-1). 

然后把这次抽取的也放置在末尾. 下次从0到n-3抽.

以此类推, 直至抽完所有元素. 每一次抽取的概率都是1/(n-i)

所以总概率即为`1/n!`

```java
class Solution {
    int[] origins;
    int[] shuffled;
    public Solution(int[] nums) {
        origins = Arrays.copyOf(nums, nums.length);
        shuffled= Arrays.copyOf(nums, nums.length);
    }
    
    /** Resets the array to its original configuration and return it. */
    public int[] reset() {
        return (int[]) origins.clone();
    }
    
    /** Returns a random shuffling of the array. */
    public int[] shuffle() {
        Random r = new Random();
        int n = shuffled.length;
        int[] ans = new int[n];
        int k = 0;
        while(n > 0)
        {
            int randomIndex = r.nextInt(n);
            ans[k++] = shuffled[randomIndex];
            // swap shuffled[randomIndex] and shuffled[n]. 
            // because next iteration will randomly select a value from 0 to n-1
            int tmp = shuffled[n-1];
            shuffled[n-1] = shuffled[randomIndex];
            shuffled[randomIndex] = tmp;
            --n;
        }
        return ans;
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(nums);
 * int[] param_1 = obj.reset();
 * int[] param_2 = obj.shuffle();
 */
```

