Design a data structure that supports all following operations in *average* **O(1)** time.

 

1. `insert(val)`: Inserts an item val to the set if not already present.
2. `remove(val)`: Removes an item val from the set if present.
3. `getRandom`: Returns a random element from current set of elements (it's guaranteed that at least one element exists when this method is called). Each element must have the **same probability** of being returned.

 

**Example:**

```
// Init an empty set.
RandomizedSet randomSet = new RandomizedSet();

// Inserts 1 to the set. Returns true as 1 was inserted successfully.
randomSet.insert(1);

// Returns false as 2 does not exist in the set.
randomSet.remove(2);

// Inserts 2 to the set, returns true. Set now contains [1,2].
randomSet.insert(2);

// getRandom should return either 1 or 2 randomly.
randomSet.getRandom();

// Removes 1 from the set, returns true. Set now contains [2].
randomSet.remove(1);

// 2 was already in the set, so return false.
randomSet.insert(2);

// Since 2 is the only number in the set, getRandom always return 2.
randomSet.getRandom();
```

## vector + hashmap

vector里面储存各个元素, hashmap储存每一个元素对应的数组下标

+ insert时,先在vector后面pushback某个value, 再从hashmap中添加一个这个值到数组下标位置的映射(value => vector.size() - 1)
+ remove时, 先从hashmap中查找val对应的映射(val => valIndex), 和数组最后一个元素对应的映射, (back => backIndex) , 知道val在数组中的位置valIndex后交换val和数组最后一个元素, `swap(vc[index], vc.back());`同时在hashmap上登记相应的变动` swap(itBack->second, itVal->second);` 然后删除数组中最后一个元素, 删除hashmap中对应最后一个元素的映射
+ getRandom, 选择一个[0, vc.size)的随机数作为随即下标, 返回对应的值即可

```c++
class RandomizedSet {
private:
    vector<int> vc;
    unordered_map<int,int> mp;
public:
    /** Initialize your data structure here. */
    RandomizedSet() {
        ;
    }
    
    /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
    bool insert(int val) {
        bool hasVal = (mp.find(val) != mp.end());
        if(hasVal)
            return false;
        vc.push_back(val);
        mp.insert({val,vc.size()-1});
        return true;
    }
    
    /** Removes a value from the set. Returns true if the set contained the specified element. */
    bool remove(int val) {
        unordered_map<int, int>::iterator itVal = mp.find(val);
        if(itVal == mp.end())
            return false;
        unordered_map<int, int>::iterator itBack = mp.find(vc.back());
        int index = itVal->second;
        swap(vc[index], vc.back());
        swap(itBack->second, itVal->second);
        vc.pop_back();
        mp.erase(itVal);
        return true;
    }
    
    /** Get a random element from the set. */
    int getRandom() {
        int randomIndex = rand() % vc.size();
        return vc[randomIndex];
    }
};

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet* obj = new RandomizedSet();
 * bool param_1 = obj->insert(val);
 * bool param_2 = obj->remove(val);
 * int param_3 = obj->getRandom();
 */
```

