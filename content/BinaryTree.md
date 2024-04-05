---
weight: 4
title: BinaryTree
math: true
---

# BinaryTree

## 94. Binary Tree Inorder Traversal

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        subFunction(root, res);

        return res;
    }

    void subFunction(TreeNode* root, vector<int>& res) {
        if (root == nullptr) return;
        else {
            subFunction(root->left, res);
            res.push_back(root->val);
            subFunction(root->right, res);
        }
    }
};
```

### 题面

给定一个二叉树的根节点 `root` ，返回 _它的 **中序** 遍历_ 。

### 题解

**Method One**: 递归

- 初始化一个结果数组
- 递归遍历二叉树，并按中根序添加元素

**Method Two**: 迭代

