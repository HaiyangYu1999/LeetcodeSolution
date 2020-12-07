Design a data structure that follows the constraints of a **[Least Recently Used (LRU) cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU)**.

Implement the `LRUCache` class:

- `LRUCache(int capacity)` Initialize the LRU cache with **positive** size `capacity`.
- `int get(int key)` Return the value of the `key` if the key exists, otherwise return `-1`.
- `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.

**Follow up:**
Could you do `get` and `put` in `O(1)` time complexity?

 

**Example 1:**

```
Input
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
```

 

**Constraints:**

- `1 <= capacity <= 3000`
- `0 <= key <= 3000`
- `0 <= value <= 104`
- At most `3 * 104` calls will be made to `get` and `put`.

## 1. LRU算法

数据结构为双向链表+hashmap

链表结构如下

```java
private static class Node{
        public int key;
        public int value;
        public Node prev;
        public Node next;
        public Node(int key, int value, Node prev, Node next){
            this.key = key;
            this.value = value;
            this.prev = prev;
            this.next = next;
        }
        public Node(int key, int value){
            this(key, value, null, null);
        }
    }
```

Hash表存储key和key-value所在的Node的映射

链表中的节点越靠近head就说明越新, 越靠近tail说明越旧. 当需要evict某一个元素的时候, 就evict链表中最后一个元素即可.

对于get方法, 查看Hashmap中有没有相应的key, 如果有, 就返回对应的Node中的值value, 并且还要将这个Node节点移动到链表头部(因为get之后算最新访问了一次). 如果没有, 就返回-1

对于put方法, 要分类讨论

+ 如果key在链表中存在, 那么修改Node处的value之后仍然要移动到链表头部. (因为put之后算最新访问了一次)
+ 如果key在链表中不存在, 并且当前缓存数量小于capacity, 直接在链表头部加一个对应的Node节点, 并在HashMap中记录即可.
+ 如果key在链表中不存在, 并且当前缓存数量等于capacity, 第一步需要剔除链表中最后一个节点removedNode.(因为访问的距离最远). 并且在HashMap删除removedNode的记录. 第二步在链表头部加一个对应的节点newNode, 并在HashMap中增加newNode的记录.

如此操作可以保证get和put都为O(1)时间

```java
class LRUCache {
    
    private static class Node{
        public int key;
        public int value;
        public Node prev;
        public Node next;
        public Node(int key, int value, Node prev, Node next){
            this.key = key;
            this.value = value;
            this.prev = prev;
            this.next = next;
        }
        public Node(int key, int value){
            this(key, value, null, null);
        }
    }
    
    Node head;                    
    Node tail;
    int count;
    Map<Integer, Node> keyNodeMap;
    int capacity;
    
    public LRUCache(int capacity) {
        if(capacity <= 0){
            throw new IllegalArgumentException();
        }
        head = new Node(-1, -1);
        tail = new Node(-2, -2);
        head.next = tail;
        tail.prev = head;
        count = 0;
        keyNodeMap = new HashMap<>(capacity);
        this.capacity = capacity;
    }
    
    public int get(int key) {
        Node ans = keyNodeMap.get(key);
        if(ans == null){
            return -1;
        }
        put(ans.key, ans.value);  //确保get后被标记为最新使用的
        return ans.value;
    }
    
    public void put(int key, int value) {
        if(keyNodeMap.containsKey(key)){        //缓存中有key, 要把更新后的节点移动到链表头部
            Node movedNode = keyNodeMap.get(key);  
            movedNode.prev.next = movedNode.next;
            movedNode.next.prev = movedNode.prev;
            movedNode.next = null;
            movedNode.prev = null;
            movedNode.value = value;

            Node nextNode = head.next;
            head.next = movedNode;
            movedNode.prev = head;
            movedNode.next = nextNode;
            nextNode.prev = movedNode;
        }
        else if(count < capacity){     //缓存中没有原来的key, 并且缓存未满, 直接在链表头部加节点即可
            Node newNode = new Node(key, value);
            keyNodeMap.put(key, newNode);
            ++count;
            Node nextNode = head.next;
            head.next = newNode;
            newNode.prev = head;
            newNode.next = nextNode;
            nextNode.prev = newNode;
        }
        else{         //缓存中没有原来的key, 并且缓存满了, 删除链表最后一个节点,在链表头部加新节点
            Node removedNode = tail.prev;
            tail.prev = removedNode.prev;
            removedNode.next = null;
            removedNode.prev.next = tail;
            removedNode.prev = null;
            keyNodeMap.remove(removedNode.key);
            
            Node newNode = new Node(key, value);
            keyNodeMap.put(key, newNode);
            Node nextNode = head.next;
            head.next = newNode;
            newNode.prev = head;
            newNode.next = nextNode;
            nextNode.prev = newNode;
        }
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```

