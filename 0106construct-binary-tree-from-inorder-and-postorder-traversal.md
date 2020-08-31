Given inorder and postorder traversal of a tree, construct the binary tree.

**Note:**
You may assume that duplicates do not exist in the tree.

For example, given

```
inorder = [9,3,15,20,7]
postorder = [9,15,7,20,3]
```

Return the following binary tree:

```
    3
   / \
  9  20
    /  \
   15   7
```

解法完全与 leetcode 105 相同

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
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        unordered_map<int, vector<int>::iterator> mp;
        for(vector<int>::iterator it = inorder.begin(); it != inorder.end(); ++it)
            mp.insert({*it,it});
        return buildTree(mp, postorder.begin(), postorder.end(), inorder.begin(), inorder.end());
    }
private:
    TreeNode* buildTree(const unordered_map<int, vector<int>::iterator>& mp, vector<int>::iterator postorder_begin, vector<int>::iterator postorder_end, vector<int>::iterator inorder_begin, vector<int>::iterator inorder_end)
    {
        if(postorder_begin == postorder_end || inorder_begin == inorder_end)
            return nullptr;
        vector<int>::iterator inorder_parentNode= mp.find(*(postorder_end - 1)) -> second;
        int leftTreeNums = inorder_parentNode - inorder_begin;
        int rightTreeNums = inorder_end - inorder_parentNode - 1;
        TreeNode* res = new TreeNode(*inorder_parentNode);
        res->left = buildTree(mp, postorder_begin, postorder_begin + leftTreeNums, inorder_begin, inorder_parentNode);
        res->right = buildTree(mp, postorder_end - 1 - rightTreeNums, postorder_end - 1, inorder_parentNode + 1, inorder_end);
        return res;
    }
};
```

