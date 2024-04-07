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


## 100. Same Tree

```cpp
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (p == nullptr and q == nullptr) return true;
        else if ((p == nullptr and q != nullptr) or (p != nullptr and q == nullptr)) return false;
        else {
            if (p->val == q->val) return isSameTree(p->left, q->left) and isSameTree(p->right, q->right);
            else return false;
        }
    }
};
```

### 题面

给你两棵二叉树的根节点 `p` 和 `q` ，编写一个函数来检验这两棵树是否相同。

如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。

### 题解

