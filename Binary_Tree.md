# Binary Tree

| Type |
| :--: |
|      |



## Binary Tree Basics

| Difficulty |                           LeetCode                           | Note |
| :--------: | :----------------------------------------------------------: | :--: |
|     🟢      | [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/) |      |
|     🟢      | [144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/) |      |
|     🟢      | [543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/) |      |
|     🔴      | [124. Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/) |      |
|     🟢      | [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/) |      |
|     🟠      | [116. Populating Next Right Pointers in Each Node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/) |      |
|     🟠      | [114. Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/) |      |

**是否能够通过一次遍历得到答案？**

如果可以，可以用一个`traverse`函数配合外部变量来实现

**是否可以定义一个递归函数，通过子树的答案推导出原问题的答案？**

如果可以，写出递归函数的定义，利用返回值

### 104. Maximum Depth of Binary Tree

Given the `root` of a binary tree, return *its maximum depth*.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/26/tmp-tree.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: 3
```

**Example 2:**

```
Input: root = [1,null,2]
Output: 2
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `-100 <= Node.val <= 100`

```cpp
class Solution {
  public:
  int maxDepth(TreeNode* root) {
    if(root == nullptr) return 0;
    return max(maxDepth(root->left), maxDepth(root->right)) + 1;
  }
}
```

---

### 144. Binary Tree Preorder Traversal

Given the `root` of a binary tree, return *the preorder traversal of its nodes' values*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/09/15/inorder_1.jpg)

```
Input: root = [1,null,2,3]
Output: [1,2,3]
```

**Example 2:**

```
Input: root = []
Output: []
```

**Example 3:**

```
Input: root = [1]
Output: [1]
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`

```cpp
class Solution {
  public:
  vector<int> res;
  vector<int> preorderTraversal(TreeNode* root) {
    traverse(root);
    return res;
  }
  void traverse(TreeNode* root) {
    if(root == nullptr) return;
    res.push_back(root->val);
    traverse(root->left);
    traverse(root->right);
  }
}
```
---

### 543. Diameter of Binary Tree

Given the `root` of a binary tree, return *the length of the **diameter** of the tree*.

The **diameter** of a binary tree is the **length** of the longest path between any two nodes in a tree. This path may or may not pass through the `root`.

The **length** of a path between two nodes is represented by the number of edges between them.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg)

```
Input: root = [1,2,3,4,5]
Output: 3
Explanation: 3 is the length of the path [4,2,1,3] or [5,2,1,3].
```

**Example 2:**

```
Input: root = [1,2]
Output: 1
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `-100 <= Node.val <= 100`

```cpp
class Solution {
  int maxDiameter = 0;
  int diameterOfBinaryTree(TreeNode* root) {
    int depth = maxDepth(TreeNode* root);
    return maxDiameter;
  }
  int maxDepth(TreeNode* root) {
    if(root == nullptr) return 0;
    int left = maxDepth(root->left);
    int right = maxDepth(root->right);
    int Diameter = left + right;
    maxDiameter = max(Diameter, maxDiameter);
    return max(left,right)+1;
  }
}
```
---
### 124. Binary Tree Maximum Path Sum

A **path** in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence **at most once**. Note that the path does not need to pass through the root.

The **path sum** of a path is the sum of the node's values in the path.

Given the `root` of a binary tree, return *the maximum **path sum** of any **non-empty** path*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/13/exx1.jpg)

```
Input: root = [1,2,3]
Output: 6
Explanation: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/10/13/exx2.jpg)

```
Input: root = [-10,9,20,null,null,15,7]
Output: 42
Explanation: The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 3 * 104]`.
- `-1000 <= Node.val <= 1000`

```cpp
class Solution {
  public:
  int res = INT_MIN;
  int maxPathSum(TreeNode* root) {
    if(root == nullptr) return 0;
    int oneSideMaxVal = oneSideMax(root);
    return res;
  }
  int oneSideMax(TreeNode* root) {
    if(root == nullptr) return 0;
    int leftSum = max(0,oneSideMax(root->left));
    int rightSum = max(0, oneSideMax(root->right));
    int pathMaxSum = root->val + leftSum + rightSum;
    res = max(res, pathMaxSum);
    return max(leftSum, rightSum) + root->val;
  }
}
```

---

### 226. Invert Binary Tree

Given the `root` of a binary tree, invert the tree, and return *its root*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg)

```
Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/03/14/invert2-tree.jpg)

```
Input: root = [2,1,3]
Output: [2,3,1]
```

**Example 3:**

```
Input: root = []
Output: []
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`

```cpp
class Solution {
  public:
  TreeNode* invertTree(TreeNode* root) {
    Traverse(root);
    return root;
  }
  void Traverse(TreeNode* root) {
    if(root == nullptr) return;
    TreeNode* temp = root->left;
    root->left = root->right;
    root->right = temp;
    Traverse(left->left);
    Traverse(left->right);
  }
}
```

---

### 116. Populating Next Right Pointers in Each Node

You are given a **perfect binary tree** where all leaves are on the same level, and every parent has two children. The binary tree has the following definition:

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to `NULL`.

Initially, all next pointers are set to `NULL`.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2019/02/14/116_sample.png)

```
Input: root = [1,2,3,4,5,6,7]
Output: [1,#,2,3,#,4,5,6,7,#]
Explanation: Given the above perfect binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.
```

**Example 2:**

```
Input: root = []
Output: []
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 212 - 1]`.
- `-1000 <= Node.val <= 1000`

 

**Follow-up:**

- You may only use constant extra space.
- The recursive approach is fine. You may assume implicit stack space does not count as extra space for this problem.

```cpp
class Solution {
  public:
  Node* connect(Node* root) {
    traverse(root->left, root->right);
    return root;
  }
  void traverse(Node* node1, Node* node2) {
    if(node1 == nullptr && node2 == nullptr) {
      return;
    }
    node1->next = node2;
    traverse(node1->left, node1->right);
    traverse(node1->right, node2->left);
    traverse(node2->left, node2->right);
  }
}
```

---

### 114. Flatten Binary Tree to Linked List

Given the `root` of a binary tree, flatten the tree into a "linked list":

- The "linked list" should use the same `TreeNode` class where the `right` child pointer points to the next node in the list and the `left` child pointer is always `null`.
- The "linked list" should be in the same order as a [**pre-order** **traversal**](https://en.wikipedia.org/wiki/Tree_traversal#Pre-order,_NLR) of the binary tree.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/14/flaten.jpg)

```
Input: root = [1,2,5,3,4,null,6]
Output: [1,null,2,null,3,null,4,null,5,null,6]
```

**Example 2:**

```
Input: root = []
Output: []
```

**Example 3:**

```
Input: root = [0]
Output: [0]
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 2000]`.
- `-100 <= Node.val <= 100`

```cpp
class Solution {
  public:
  void flatten(TreeNode* root) {
    if(root == nullptr) return;
    flatten(root->left);
    flatten(root->right);
    TreeNode* left = root->left;
    TreeNode* right = root->right;
    root->left = nullptr;
    root->right = left;
    TreeNode* p = root;
    while(p->right != nullptr) {
      p = p->right;
    }
    p->right = right;
  }
}
```

## Binary Tree Build

| Difficulty |                           LeetCode                           | Note |
| :--------: | :----------------------------------------------------------: | :--: |
|     🟠      | [654. Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/) |      |
|     🟠      | [105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/) |      |
|     🟠      | [106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/) |      |
|     🟠      | [889. Construct Binary Tree from Preorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/) |      |

**构造整棵树 = 根节点 + 构造左子树 + 构造右子树**

### 654. Maximum Binary Tree

You are given an integer array `nums` with no duplicates. A **maximum binary tree** can be built recursively from `nums` using the following algorithm:

1. Create a root node whose value is the maximum value in `nums`.
2. Recursively build the left subtree on the **subarray prefix** to the **left** of the maximum value.
3. Recursively build the right subtree on the **subarray suffix** to the **right** of the maximum value.

Return *the **maximum binary tree** built from* `nums`.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/12/24/tree1.jpg)

```
Input: nums = [3,2,1,6,0,5]
Output: [6,3,5,null,2,0,null,null,1]
Explanation: The recursive calls are as follow:
- The largest value in [3,2,1,6,0,5] is 6. Left prefix is [3,2,1] and right suffix is [0,5].
    - The largest value in [3,2,1] is 3. Left prefix is [] and right suffix is [2,1].
        - Empty array, so no child.
        - The largest value in [2,1] is 2. Left prefix is [] and right suffix is [1].
            - Empty array, so no child.
            - Only one element, so child is a node with value 1.
    - The largest value in [0,5] is 5. Left prefix is [0] and right suffix is [].
        - Only one element, so child is a node with value 0.
        - Empty array, so no child.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/12/24/tree2.jpg)

```
Input: nums = [3,2,1]
Output: [3,null,2,null,1]
```

 

**Constraints:**

- `1 <= nums.length <= 1000`
- `0 <= nums[i] <= 1000`
- All integers in `nums` are **unique**.

```cpp
class Solution {
  public:
  TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
    return construct(nums, 0, nums.size()-1);
  }
  TreeNode* construct(vector<int>& nums, int lo, int hi) {
    if(lo > hi) return nullptr;
    int index = -1;
    int maxVal = INT_MIN;
    for(int i = lo; i <= hi; i++) {
      if(maxVal < nums[i]) {
        index = i;
        maxVal = nums[i];
      }
    }
    TreeNode* root = new TreeNode();
    root->val = maxVal;
    root->left = construct(nums, lo, index-1);
    root->right = construct(nums, index+1, hi);
    return root;
  }
}
```

---

### 105. Construct Binary Tree from Preorder and Inorder Traversal

Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return *the binary tree*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

```
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]
```

**Example 2:**

```
Input: preorder = [-1], inorder = [-1]
Output: [-1]
```

 

**Constraints:**

- `1 <= preorder.length <= 3000`
- `inorder.length == preorder.length`
- `-3000 <= preorder[i], inorder[i] <= 3000`
- `preorder` and `inorder` consist of **unique** values.
- Each value of `inorder` also appears in `preorder`.
- `preorder` is **guaranteed** to be the preorder traversal of the tree.
- `inorder` is **guaranteed** to be the inorder traversal of the tree.

```cpp
class Solution {
  public:
  TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
    
  }
  TreeNode* build(vector<int>& preorder, int prestart, int preend, vector<int>& inorder, int instart, int inend) {
    if(prestart > preend) {
      return nullptr;
    }
    int rootVal = preorder[prestart];
    int index = 0;
    for(int i = instart; i <= inend; i++) {
      if(inorder[i] == rootVal) {
        index = i;
        break;
      }
    }
    TreeNode* root = new TreeNode();
    root->val = rootVal;
    int leftSize = index - instart;
    root->left = build(preorder, prestart + 1, prestart + leftSize, inorder, instart ,index - 1);
    root->right = build(preorder, prestart + leftSize + 1, preend, inorder, index + 1, inend);
    return root;
  }
}
```

---

### 106. Construct Binary Tree from Inorder and Postorder Traversal

Given two integer arrays `inorder` and `postorder` where `inorder` is the inorder traversal of a binary tree and `postorder` is the postorder traversal of the same tree, construct and return *the binary tree*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

```
Input: inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
Output: [3,9,20,null,null,15,7]
```

**Example 2:**

```
Input: inorder = [-1], postorder = [-1]
Output: [-1]
```

 

**Constraints:**

- `1 <= inorder.length <= 3000`
- `postorder.length == inorder.length`
- `-3000 <= inorder[i], postorder[i] <= 3000`
- `inorder` and `postorder` consist of **unique** values.
- Each value of `postorder` also appears in `inorder`.
- `inorder` is **guaranteed** to be the inorder traversal of the tree.
- `postorder` is **guaranteed** to be the postorder traversal of the tree.

```cpp
class Solution {
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        return build(inorder, 0, inorder.size()-1, postorder, 0, postorder.size()-1);
    }
    TreeNode* build(vector<int>& inorder, int inStart, int inEnd, vector<int>& postorder, int postStart, int postEnd) {
        if(inStart > inEnd) {
            return nullptr;
        }
        int rootVal = postorder[postEnd];
        int index = 0;
        for(int i = 0; i < inorder.size(); i++) {
            if(inorder[i] == rootVal) {
                index = i;
                break;
            }
        }
        int leftSize = index - inStart;
        TreeNode* root = new TreeNode();
        root->val = rootVal;
        root->left = build(inorder, inStart, index-1, postorder, postStart, postStart + leftSize - 1);
        root->right = build(inorder, index+1, inEnd, postorder, postStart + leftSize,  postEnd - 1);
        return root;
    }
};
```

---

### 889. Construct Binary Tree from Preorder and Postorder Traversal

Given two integer arrays, `preorder` and `postorder` where `preorder` is the preorder traversal of a binary tree of **distinct** values and `postorder` is the postorder traversal of the same tree, reconstruct and return *the binary tree*.

If there exist multiple answers, you can **return any** of them.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/07/24/lc-prepost.jpg)

```
Input: preorder = [1,2,4,5,3,6,7], postorder = [4,5,2,6,7,3,1]
Output: [1,2,3,4,5,6,7]
```

**Example 2:**

```
Input: preorder = [1], postorder = [1]
Output: [1]
```

 

**Constraints:**

- `1 <= preorder.length <= 30`
- `1 <= preorder[i] <= preorder.length`
- All the values of `preorder` are **unique**.
- `postorder.length == preorder.length`
- `1 <= postorder[i] <= postorder.length`
- All the values of `postorder` are **unique**.
- It is guaranteed that `preorder` and `postorder` are the preorder traversal and postorder traversal of the same binary tree.

```cpp
class Solution {
public:
    TreeNode* constructFromPrePost(vector<int>& preorder, vector<int>& postorder) {
        return build(preorder, 0, preorder.size()-1, postorder, 0, postorder.size()-1);
    }
    TreeNode* build(vector<int>& preorder, int prestart, int preend, vector<int>& postorder, int poststart, int postend) {
        if(prestart > preend) return nullptr;
        if(prestart == preend) {
            return new TreeNode(preorder[prestart]);
        }
        int rootVal = preorder[prestart];
        int leftRootVal = preorder[prestart+1];
        int index;
        for(int i = 0; i < postorder.size(); i++) {
            if(postorder[i] == leftRootVal) {
                index = i;
                break;
            }
        }
        int leftSize = index - poststart + 1;
        TreeNode* root = new TreeNode(rootVal);
        root->left = build(preorder, prestart + 1, prestart + leftSize, postorder, poststart, index);
        root->right = build(preorder, prestart + leftSize + 1, preend, postorder, index + 1, postend - 1);
        return root;
    }
};
```

