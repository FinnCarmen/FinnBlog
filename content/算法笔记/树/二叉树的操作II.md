+++
date = '2025-02-01T11:24:45+08:00'
draft = false
title = '二叉树的操作II'
+++

### [左叶子之和](https://leetcode.cn/problems/sum-of-left-leaves/)

```cpp
class Solution {
public:
    int sumOfLeftLeaves(TreeNode* root) {
        if(root==NULL)return 0;
        if(root->left==NULL && root->right==NULL)return 0;

        int leftVal=sumOfLeftLeaves(root->left);//左
        if(root->left && !root->left->left && !root->left->right)
            leftVal=root->left->val;
        int rightVal=sumOfLeftLeaves(root->right);//右
        int sum=leftVal+rightVal;//中
        return sum;
    }
};
```

### [找树左下角的值](https://leetcode.cn/problems/find-bottom-left-tree-value/)

```cpp
class Solution {
public:
    int res;
    int maxDepth = INT_MIN;
    void traversal(TreeNode* root,int depth)
    {
        if(root->left==NULL&&root->right==NULL)
        {
            if(depth > maxDepth)
            {
                maxDepth=depth;
                res=root->val;
            }
            return;
        }
        if(root->left)
        {
            depth++;
            traversal(root->left,depth);
            depth--;
        }
        if(root->right)
        {
            depth++;
            traversal(root->right,depth);
            depth--;
        }
        return;
    }
    int findBottomLeftValue(TreeNode* root) {
        traversal(root,0);
        return res;
    }
};
```

### 路径总和 I

https://leetcode.cn/problems/path-sum/

#### 递归

```cpp
class Solution {
private:
    bool traversal(TreeNode* cur, int count) {
        if (!cur->left && !cur->right && count == 0) return true; // 遇到叶子节点，并且计数为0
        if (!cur->left && !cur->right) return false; // 遇到叶子节点直接返回

        if (cur->left) { // 左
            count -= cur->left->val; // 递归，处理节点;
            if (traversal(cur->left, count)) return true;
            count += cur->left->val; // 回溯，撤销处理结果
        }
        if (cur->right) { // 右
            count -= cur->right->val; // 递归，处理节点;
            if (traversal(cur->right, count)) return true;
            count += cur->right->val; // 回溯，撤销处理结果
        }
        return false;
    }

public:
    bool hasPathSum(TreeNode* root, int sum) {
        if (root == NULL) return false;
        return traversal(root, sum - root->val);
    }
};
```

#### 迭代

```cpp
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) {
        if (!root) return false;
        stack<pair<TreeNode*,int>>st;//(节点指针，路径数值)
        st.push(pair<TreeNode*,int>(root,root->val));
        while(!st.empty())
        {
            pair<TreeNode*,int>node=st.top();
            st.pop();
            if(!node.first->left && !node.first->right && sum==node.second)
                return true;
            if(node.first->right)st.push(pair<TreeNode*,int>(node.first->right,node.second+node.first->right->val);
            if(node.first->left)st.push(pair<TreeNode*,int>(node.first->left,node.second+node.first->left->val);
        }
        return false;
    }
};
```

### 路径总和 II

https://leetcode.cn/problems/path-sum-ii/

```cpp
class solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    // 递归函数不需要返回值，因为我们要遍历整个树
    void traversal(TreeNode* cur, int count) {
        if (!cur->left && !cur->right && count == 0) { // 遇到了叶子节点且找到了和为sum的路径
            result.push_back(path);
            return;
        }

        if (!cur->left && !cur->right) return ; // 遇到叶子节点而没有找到合适的边，直接返回

        if (cur->left) { // 左 （空节点不遍历）
            path.push_back(cur->left->val);
            count -= cur->left->val;
            traversal(cur->left, count);    // 递归
            count += cur->left->val;        // 回溯
            path.pop_back();                // 回溯
        }
        if (cur->right) { // 右 （空节点不遍历）
            path.push_back(cur->right->val);
            count -= cur->right->val;
            traversal(cur->right, count);   // 递归
            count += cur->right->val;       // 回溯
            path.pop_back();                // 回溯
        }
        return ;
    }

public:
    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        result.clear();
        path.clear();
        if (root == NULL) return result;
        path.push_back(root->val); // 把根节点放进路径
        traversal(root, sum - root->val);
        return result;
    }
};
```

[从中序与后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

以 后序数组的最后一个元素为切割点，先切中序数组，根据中序数组，反过来再切后序数组。

一层一层切下去，每次后序数组最后一个元素就是节点元素。

```cpp
class Solution {
public:
    TreeNode* traversal(vector<int>&inorder,vector<int>&postorder)
    {
        if(postorder.size()==0)return NULL;

        //后序遍历的最后一个元素为当前节点
        int rootVal=postorder[postorder.size()-1];
        TreeNode* root=new TreeNode(rootVal);

        //叶子节点
        if(postorder.size()==1)return root;

        int id;//中序遍历的切割点
        for(id=0;id<inorder.size();id++)
            if(inorder[id]==rootVal)break;

        //切割中序遍历数组
        vector<int>leftInorder(inorder.begin(),inorder.begin()+id);
        vector<int>rightInorder(inorder.begin()+id+1,inorder.end());

        postorder.resize(postorder.size()-1);//剔除后序数组中的当前节点

        //切割后序遍历数组(切割点为leftInorder.size())
        vector<int>leftpostorder(postorder.begin(),postorder.begin()+leftInorder.size());
        vector<int>rightpostorder(postorder.begin()+leftInorder.size(),postorder.end());

        //递归
        root->left=traversal(leftInorder,leftpostorder);
        root->right=traversal(rightInorder,rightpostorder);
        return root;

    }
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        if(inorder.size()==0 || postorder.size()==0)return NULL;
        return traversal(inorder,postorder);
    }
};
```

[最大二叉树](https://leetcode.cn/problems/maximum-binary-tree/)

```cpp
class Solution {
public:
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        TreeNode* node=new TreeNode(0);
        if(nums.size()==1)
        {
            node->val=nums[0];
            return node;
        }

        int maxVal=0;
        int maxValId=0;
        for(int i=0;i<nums.size();i++)
        {
            if(nums[i]>maxVal)
            {
                maxVal=nums[i];
                maxValId=i;
            }
        }
        node->val=maxVal;
        //
        if(maxValId>0)
        {
            vector<int>newVec(nums.begin(),nums.begin()+maxValId);
            node->left=constructMaximumBinaryTree(newVec);
        }

        if(maxValId<(nums.size()-1))
        {
            vector<int>newVec(nums.begin()+maxValId+1,nums.end());
            node->right=constructMaximumBinaryTree(newVec);
        }
        return node;
    }
};
```
