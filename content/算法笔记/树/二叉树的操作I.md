+++
date = '2025-02-01T11:21:12+08:00'
draft = false
title = '二叉树的操作I'
+++

### [翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)

```cpp
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if(root==NULL)return root;
        swap(root->left,root->right);
        invertTree(root->left);
        invertTree(root->right);
        return root;
    }
};
```

### [对称二叉树](https://leetcode.cn/problems/symmetric-tree/)

```cpp
class Solution {
public:
    bool compare(TreeNode* left,TreeNode* right)
    {
        if(left==NULL && right!=NULL)return false;
        else if(left!=NULL && right ==NULL)return false;
        else if(left==NULL && right ==NULL)return true;
        else if(left->val != right->val)return false;

        bool outside=compare(left->left,right->right);
        bool inside=compare(left->right,right->left);
        bool isSame=outside && inside;
        return isSame;
    }
    bool isSymmetric(TreeNode* root) {
        if(!root)return true;
        return compare(root->left,root->right);
    }
};
```

### [二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

```cpp
class Solution {
public:
    int getdepth(TreeNode* node) {
        if (node == NULL) return 0;
        int leftdepth = getdepth(node->left);       // 左
        int rightdepth = getdepth(node->right);     // 右
        int depth = 1 + max(leftdepth, rightdepth); // 中
        return depth;
    }
    int maxDepth(TreeNode* root) {
        return getdepth(root);
    }
};

```

### [二叉树的最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)

```cpp
class Solution {
public:
    int getDepth(TreeNode* node) {
        if (node == NULL) return 0;
        int leftDepth = getDepth(node->left);           // 左
        int rightDepth = getDepth(node->right);         // 右
                                                        // 中
        // 当一个左子树为空，右不为空，这时并不是最低点
        if (node->left == NULL && node->right != NULL) {
            return 1 + rightDepth;
        }
        // 当一个右子树为空，左不为空，这时并不是最低点
        if (node->left != NULL && node->right == NULL) {
            return 1 + leftDepth;
        }
        int result = 1 + min(leftDepth, rightDepth);
        return result;
    }

    int minDepth(TreeNode* root) {
        return getDepth(root);
    }
};
```

### [完全二叉树的节点个数](https://leetcode.cn/problems/count-complete-tree-nodes/)

```cpp
class Solution {
public:
    int getNum(TreeNode* cur)
    {
        if(cur==NULL)return 0;
        int leftNum=getNum(cur->left);
        int rightNum=getNum(cur->right);
        int Num=leftNum+rightNum+1;
        return Num;
    }
    int countNodes(TreeNode* root) {
        return getNum(root);
    }
};
```

### [平衡二叉树](https://leetcode.cn/problems/balanced-binary-tree/)

```cpp
class Solution {
public:
    // 返回以该节点为根节点的二叉树的高度，如果不是平衡二叉树了则返回-1
    int getHeight(TreeNode* node) {
        if (node == NULL) {
            return 0;
        }
        int leftHeight = getHeight(node->left);
        if (leftHeight == -1) return -1;
        int rightHeight = getHeight(node->right);
        if (rightHeight == -1) return -1;
        return abs(leftHeight - rightHeight) > 1 ? -1 : 1 + max(leftHeight, rightHeight);
    }
    bool isBalanced(TreeNode* root) {
        return getHeight(root) == -1 ? false : true;
    }
};
```

### [二叉树的所有路径](https://leetcode.cn/problems/binary-tree-paths/)

```cpp
class Solution {
private:

    void traversal(TreeNode* cur, string path, vector<string>& result) {
        path += to_string(cur->val); // 中
        if (cur->left == NULL && cur->right == NULL) {
            result.push_back(path);
            return;
        }
        if (cur->left) traversal(cur->left, path + "->", result); // 左
        if (cur->right) traversal(cur->right, path + "->", result); // 右
    }

public:
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> result;
        string path;
        if (root == NULL) return result;
        traversal(root, path, result);
        return result;

    }
};

```
