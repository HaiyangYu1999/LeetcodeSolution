Given preorder and inorder traversal of a tree, construct the binary tree.

**Note:**
You may assume that duplicates do not exist in the tree.

For example, given

```
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
```

Return the following binary tree:

```
    3
   / \
  9  20
    /  \
   15   7
```

## 1 递归

注意到

> 前序遍历中, 第一个元素肯定是根节点的元素. 中序遍历中, 根节点元素左边的元素为左子树的元素, 右边的元素为右子树的元素

所以, 给定四个参数

```c++
TreeNode* buildTree(vector<int>::iterator preorder_begin, vector<int>::iterator preorder_end, vector<int>::iterator inorder_begin, vector<int>::iterator inorder_end)
```

那么根节点的元素一定是`*preorder_begin`, 在中序遍历中找到`*preorder_begin`对应的位置`inorder_parentNode`

那么`inorder_parentNode`左边的元素都为左子树, 统计左子树节点的数量`leftTreeNums`, 在前序遍历的数组中, `preorder_begin + 1`到` preorder_begin + leftTreeNums + 1`即为左子树的位置,  在中序遍历的数组中`inorder_begin`到 `inorder_parentNode` 即为左子树的位置. 右子树在前序遍历和中序遍历的位置同理可得. 

递归的构造左子树和右子树, 再加上parentNode即可.

最好情况时间复杂度O(nlogn)

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        return buildTree(preorder.begin(), preorder.end(), inorder.begin(), inorder.end());
    }
private:
    TreeNode* buildTree(vector<int>::iterator preorder_begin, vector<int>::iterator preorder_end, vector<int>::iterator inorder_begin, vector<int>::iterator inorder_end)
    {
        if(preorder_begin == preorder_end || inorder_begin == inorder_end)
            return nullptr;
        int parentNodeValue = *preorder_begin;
        vector<int>::iterator inorder_parentNode= find(inorder_begin, inorder_end, parentNodeValue);
        int leftTreeNums = inorder_parentNode - inorder_begin;
        int rightTreeNums = inorder_end - inorder_parentNode - 1;
        TreeNode* res = new TreeNode(parentNodeValue);
        res->left = buildTree(preorder_begin + 1, preorder_begin + leftTreeNums + 1, inorder_begin, inorder_parentNode);
        res->right = buildTree(preorder_end - rightTreeNums, preorder_end, inorder_parentNode + 1, inorder_end);
        return res;
    }
};
```

## 2 递归 with HashMap

上面的做法只超过25%的cpp程序, 寻找运行慢的原因是因为, 在每一次的递归中, 都需要查找根节点的值`parentNodeValue`在中序遍历中的位置`inorder_parentNode`. 

假如二叉树是平衡的, 那么第1层递归要在1个长为n的数组中找1个`inorder_parentNode`, 第2层递归要在2个长为n/2的数组中找2个`inorder_parentNode`, ..., 第log n层递归要在2\^(n-1)个长为n/2\^(n-1)的数组中找2\^(n-1)个`inorder_parentNode`. 总的查找时间为n * log n. 若二叉树不平衡, 考虑任何一个节点只有左子树, 则查找时间升高到n(n-1)/2. 非常费时间

 **所以在相同的数组中多次进行元素查找可以用哈希表加快查询速度. ** 参考leetcode 1.

```c++
unordered_map<int, vector<int>::iterator> mp;
for(vector<int>::iterator it = inorder.begin(); it != inorder.end(); ++it)
	mp.insert({*it,it});
```

每一次递归中的find查找只花费常数时间复杂度. 总复杂度为O(n)

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        unordered_map<int, vector<int>::iterator> mp;
        for(vector<int>::iterator it = inorder.begin(); it != inorder.end(); ++it)
            mp.insert({*it,it});
        return buildTree(mp, preorder.begin(), preorder.end(), inorder.begin(), inorder.end());
    }
private:
    TreeNode* buildTree(const unordered_map<int, vector<int>::iterator>& mp, vector<int>::iterator preorder_begin, vector<int>::iterator preorder_end, vector<int>::iterator inorder_begin, vector<int>::iterator inorder_end)
    {
        if(preorder_begin == preorder_end || inorder_begin == inorder_end)
            return nullptr;
        int parentNodeValue = *preorder_begin;
        vector<int>::iterator inorder_parentNode= mp.find(parentNodeValue) -> second;
        int leftTreeNums = inorder_parentNode - inorder_begin;
        int rightTreeNums = inorder_end - inorder_parentNode - 1;
        TreeNode* res = new TreeNode(parentNodeValue);
        res->left = buildTree(mp, preorder_begin + 1, preorder_begin + leftTreeNums + 1, inorder_begin, inorder_parentNode);
        res->right = buildTree(mp, preorder_end - rightTreeNums, preorder_end, inorder_parentNode + 1, inorder_end);
        return res;
    }
};
```

## 3 非递归

**贴一个leetcode官方给的题解, 用的栈模拟了递归. 感觉时间和空间上不会好很多但是难度增加了很多, 个人不是太喜欢这个做法.**

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/solution/cong-qian-xu-yu-zhong-xu-bian-li-xu-lie-gou-zao-9/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

思路

迭代法是一种非常巧妙的实现方法。

对于前序遍历中的任意两个连续节点 u 和 v，根据前序遍历的流程，我们可以知道 u 和 v 只有两种可能的关系：

+ v 是 u 的左儿子。这是因为在遍历到 u 之后，下一个遍历的节点就是 u 的左儿子，即 v；

+ u 没有左儿子，并且 v 是 u 的某个祖先节点（或者 u 本身）的右儿子。如果 u 没有左儿子，那么下一个遍历的节点就是 u 的右儿子。如果 u 没有右儿子，我们就会向上回溯，直到遇到第一个有右儿子（且 u 不在它的右儿子的子树中）的节点 u_a  ，那么 v 就是 u_a  的右儿子。

第二种关系看上去有些复杂。我们举一个例子来说明其正确性，并在例子中给出我们的迭代算法。

例子

我们以树


             3
            / \
           9  20
          /  /  \
         8  15   7
       / \
      5  10
     /
    4
为例，它的前序遍历和中序遍历分别为


preorder = [3, 9, 8, 5, 4, 10, 20, 15, 7]
inorder = [4, 5, 8, 10, 9, 3, 15, 20, 7]
我们用一个栈 stack 来维护「当前节点的所有还没有考虑过右儿子的祖先节点」，栈顶就是当前节点。也就是说，只有在栈中的节点才可能连接一个新的右儿子。同时，我们用一个指针 index 指向中序遍历的某个位置，初始值为 0。index 对应的节点是「当前节点不断往左走达到的最终节点」，这也是符合中序遍历的，它的作用在下面的过程中会有所体现。

首先我们将根节点 3 入栈，再初始化 index 所指向的节点为 4，随后对于前序遍历中的每个节点，我们依此判断它是栈顶节点的左儿子，还是栈中某个节点的右儿子。

我们遍历 9。9 一定是栈顶节点 3 的左儿子。我们使用反证法，假设 9 是 3 的右儿子，那么 3 没有左儿子，index 应该恰好指向 3，但实际上为 4，因此产生了矛盾。所以我们将 9 作为 3 的左儿子，并将 9 入栈。

stack = [3, 9]
index -> inorder[0] = 4
我们遍历 8，5 和 4。同理可得它们都是上一个节点（栈顶节点）的左儿子，所以它们会依次入栈。

stack = [3, 9, 8, 5, 4]
index -> inorder[0] = 4
我们遍历 10，这时情况就不一样了。我们发现 index 恰好指向当前的栈顶节点 4，也就是说 4 没有左儿子，那么 10 必须为栈中某个节点的右儿子。那么如何找到这个节点呢？栈中的节点的顺序和它们在前序遍历中出现的顺序是一致的，而且每一个节点的右儿子都还没有被遍历过，那么这些节点的顺序和它们在中序遍历中出现的顺序一定是相反的。

这是因为栈中的任意两个相邻的节点，前者都是后者的某个祖先。并且我们知道，栈中的任意一个节点的右儿子还没有被遍历过，说明后者一定是前者左儿子的子树中的节点，那么后者就先于前者出现在中序遍历中。

因此我们可以把 index 不断向右移动，并与栈顶节点进行比较。如果 index 对应的元素恰好等于栈顶节点，那么说明我们在中序遍历中找到了栈顶节点，所以将 index 增加 1 并弹出栈顶节点，直到 index 对应的元素不等于栈顶节点。按照这样的过程，我们弹出的最后一个节点 x 就是 10 的双亲节点，这是因为 10 出现在了 x 与 x 在栈中的下一个节点的中序遍历之间，因此 10 就是 x 的右儿子。

回到我们的例子，我们会依次从栈顶弹出 4，5 和 8，并且将 index 向右移动了三次。我们将 10 作为最后弹出的节点 8 的右儿子，并将 10 入栈。

stack = [3, 9, 10]
index -> inorder[3] = 10
我们遍历 20。同理，index 恰好指向当前栈顶节点 10，那么我们会依次从栈顶弹出 10，9 和 3，并且将 index 向右移动了三次。我们将 20 作为最后弹出的节点 3 的右儿子，并将 20 入栈。

stack = [20]
index -> inorder[6] = 15
我们遍历 15，将 15 作为栈顶节点 20 的左儿子，并将 15 入栈。

stack = [20, 15]
index -> inorder[6] = 15
我们遍历 7。index 恰好指向当前栈顶节点 15，那么我们会依次从栈顶弹出 15 和 20，并且将 index 向右移动了两次。我们将 7 作为最后弹出的节点 20 的右儿子，并将 7 入栈。

stack = [7]
index -> inorder[8] = 7
此时遍历结束，我们就构造出了正确的二叉树。

算法

我们归纳出上述例子中的算法流程：

我们用一个栈和一个指针辅助进行二叉树的构造。初始时栈中存放了根节点（前序遍历的第一个节点），指针指向中序遍历的第一个节点；

我们依次枚举前序遍历中除了第一个节点以外的每个节点。如果 index 恰好指向栈顶节点，那么我们不断地弹出栈顶节点并向右移动 index，并将当前节点作为最后一个弹出的节点的右儿子；如果 index 和栈顶节点不同，我们将当前节点作为栈顶节点的左儿子；

无论是哪一种情况，我们最后都将当前的节点入栈。

最后得到的二叉树即为答案。

```c++
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        if (!preorder.size()) {
            return nullptr;
        }
        TreeNode* root = new TreeNode(preorder[0]);
        stack<TreeNode*> stk;
        stk.push(root);
        int inorderIndex = 0;
        for (int i = 1; i < preorder.size(); ++i) {
            int preorderVal = preorder[i];
            TreeNode* node = stk.top();
            if (node->val != inorder[inorderIndex]) {
                node->left = new TreeNode(preorderVal);
                stk.push(node->left);
            }
            else {
                while (!stk.empty() && stk.top()->val == inorder[inorderIndex]) {
                    node = stk.top();
                    stk.pop();
                    ++inorderIndex;
                }
                node->right = new TreeNode(preorderVal);
                stk.push(node->right);
            }
        }
        return root;
    }
};

```

