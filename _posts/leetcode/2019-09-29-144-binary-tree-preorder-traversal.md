---
title:  "144 - binary tree preorder traversal"
tags: 
    - Medium
    - Stack
    - Tree
---
Given a binary tree, return the preorder traversal of its nodes' values.

Example:
```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [1,2,3]
```
Follow up: Recursive solution is trivial, could you do it iteratively?

## Solution

### 58%

与那些基本上都有标准遍历方式（通常是按线性顺序）的线性数据结构（如链表、一维数组）所不同的是，树结构有多种不同的遍历方式。从二叉树的根节点出发，节点的遍历分为三个主要步骤：对当前节点进行操作（称为“访问”节点）、遍历左边子节点、遍历右边子节点。这三个步骤的先后顺序也是不同遍历方式的根本区别。

由于从给定的某个节点出发，有多个可以前往的下一个节点（树不是线性数据结构），所以在顺序计算（即非并行计算）的情况下，只能推迟对某些节点的访问——即以某种方式保存起来以便稍后再访问。常见的做法是采用栈（LIFO）或队列（FIFO）。由于树本身是一种自我引用（即递归定义）的数据结构，因此很自然也可以用递归方式，或者更准确地说，用corecursion，来实现延迟节点的保存。
这时（采用递归的情况）这些节点被保存在call stack中。

注意我们是如何小心地处理空树这种退化情况的。虽然小心总是重要的，但在递归程序中它尤为重要。空树是一种`基准情况`，空树出现在两种位置，一种是树根为空，另一种是树叶的左右孩子为空树，如果没有依靠这种自然的基准情况 return函数，递归调用将无法退出，造成程序死循环耗尽内存栈空间，即 stack overflow。

在实现编译器时，会遇到尾递归优化问题。

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        
        if (root == nullptr)
            return nodes;

        nodes.push_back(root->val);
    
        if (root->left != nullptr)
            preorderTraversal(root->left);
        if (root->right != nullptr)
            preorderTraversal(root->right);
        return nodes;
    }

    vector<int> nodes{};
};
```

```
Success
Details 
Runtime: 4 ms, faster than 58.84% of C++ online submissions for Binary Tree Preorder Traversal.
Memory Usage: 10.4 MB, less than 6.90% of C++ online submissions for Binary Tree Preorder Traversal.
Next challenges:
Binary Tree Inorder Traversal
Verify Preorder Sequence in Binary Search Tree
N-ary Tree Preorder Traversal
```

### 100.00%

但是，递归解法造成内存栈空间占用过多；函数上下文频繁切换，运行时间长。
由于递归解法是依赖函数的语言的 call stack，根据提示要求直接迭代，需手动模拟递归 call stack。

此外，还要注意对 root的改变是安全的，因为我们只拷贝并修改了指针本身，而未修改指针所指向的值。
```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> nodes;
        stack<TreeNode*> todo;
        while (root || !todo.empty()) {
            if (root) {
                nodes.push_back(root -> val);
                if (root -> right) {
                    todo.push(root -> right);
                }
                root = root -> left;
            } else {
                root = todo.top();
                todo.pop();
            }
        }
        return nodes;
    }
};
```

```
Success
Details 
Runtime: 0 ms, faster than 100.00% of C++ online submissions for Binary Tree Preorder Traversal.
Memory Usage: 9 MB, less than 100.00% of C++ online submissions for Binary Tree Preorder Traversal.
Next challenges:
Binary Tree Inorder Traversal
Verify Preorder Sequence in Binary Search Tree
N-ary Tree Preorder Traversal
```