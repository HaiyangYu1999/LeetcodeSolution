Implement a trie with `insert`, `search`, and `startsWith` methods.

**Example:**

```
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // returns true
trie.search("app");     // returns false
trie.startsWith("app"); // returns true
trie.insert("app");   
trie.search("app");     // returns true
```

**Note:**

- You may assume that all inputs are consist of lowercase letters `a-z`.
- All inputs are guaranteed to be non-empty strings.

## 1. 字典树

每个节点的定义如下

```java
private static class Node{
    public int count;
    public Node[] children;
    public Node(){
        count = 0;
        children = new Node[26];
    }
}
```

插入比较简单, 就是遍历一遍字符串, 看看每一个字符是不是在当前节点的孩子中, 如果是, 就更新当前节点为对应的孩子节点`currNode = currNode.children[index];` 如果没有对应的孩子, 就新建孩子节点`currNode.children[index] = new Node();`之后再更新当前节点.

查找也很简单, 顺着树查找下去, 走不通了就说明没有, 走到尽头要判断count是不是大于0, 大于0才说明有这个字符串. count等于0只能说明有单词是以当前的字符串为前缀.

判断前缀要说明一下, 还是顺着树查找, 当前缀能走到头时, 就一定说明存在以当前前缀开头的单词. **因为我们不在字典树中删除元素, 所以只要一个节点被创建, 就说明要么存在以这个节点结尾的单词, 要么这个节点的某个后代是一个单词, 而这个后代肯定是以prefix作为前缀的.** 

```java
class Trie {
    private static class Node{
        public int count;
        public Node[] children;
        public Node(){
            count = 0;
            children = new Node[26];
        }
    }
    private Node root;
    /** Initialize your data structure here. */
    public Trie() {
        root = new Node();
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        if(word == null){
            throw new NullPointerException();
        }
        Node currNode = root;
        for(int i = 0; i < word.length(); ++i){
            int index = word.charAt(i) - 'a';
            if(currNode.children[index] == null){
                currNode.children[index] = new Node();
            }
            currNode = currNode.children[index];
        }
        currNode.count++;
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        if(word == null){
            throw new NullPointerException();
        }
        Node currNode = root;
        for(int i = 0; i < word.length(); ++i){
            int index = word.charAt(i) - 'a';
            if(currNode.children[index] == null){
                return false;
            }
            currNode = currNode.children[index];
        }
        return currNode.count > 0;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        if(prefix == null){
            throw new NullPointerException();
        }
        Node currNode = root;
        for(int i = 0; i < prefix.length(); ++i){
            int index = prefix.charAt(i) - 'a';
            if(currNode.children[index] == null){
                return false;
            }
            currNode = currNode.children[index];
        }
        return true;
    }
}

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
```

