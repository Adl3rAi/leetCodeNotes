# Binary Tree

| Type |
| :--: |
|      |



## Binary Tree Basics

| Difficulty |                           LeetCode                           | Note |
| :--------: | :----------------------------------------------------------: | :--: |
|     🟢      | [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/) |[104. Maximum Depth of Binary Tree](#104-maximum-depth-of-binary-tree)      |
|     🟢      | [144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/) |[144. Binary Tree Preorder Traversal](#144-binary-tree-preorder-traversal)      |
|     🟢      | [543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/) |[543. Diameter of Binary Tree](#543-diameter-of-binary-tree)      |
|     🔴      | [124. Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/) |[124. Binary Tree Maximum Path Sum](#124-binary-tree-maximum-path-sum)      |
|     🟢      | [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/) |[226. Invert Binary Tree](#226-invert-binary-tree)      |
|     🟠      | [116. Populating Next Right Pointers in Each Node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/) |[116. Populating Next Right Pointers in Each Node](#116-populating-next-right-pointers-in-each-node)      |
|     🟠      | [114. Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/) |[114. Flatten Binary Tree to Linked List](#114-flatten-binary-tree-to-linked-list)      |
|     🟠      | [652. Find Duplicate Subtree](https://leetcode.com/problems/find-duplicate-subtrees/) |[652. Find Duplicate Subtree](#652-find-duplicate-subtree)      |

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

---

### 652. Find Duplicate Subtree

Given the `root` of a binary tree, return all **duplicate subtrees**.

For each kind of duplicate subtrees, you only need to return the root node of any **one** of them.

Two trees are **duplicate** if they have the **same structure** with the **same node values**.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/08/16/e1.jpg)

```
Input: root = [1,2,3,4,null,2,4,null,null,4]
Output: [[2,4],[4]]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/08/16/e2.jpg)

```
Input: root = [2,1,1]
Output: [[1]]
```

**Example 3:**

![img](https://assets.leetcode.com/uploads/2020/08/16/e33.jpg)

```
Input: root = [2,2,2,3,null,3,null]
Output: [[2,3],[3]]
```

 

**Constraints:**

- The number of the nodes in the tree will be in the range `[1, 10^4]`
- `-200 <= Node.val <= 200`

```cpp
class Solution {
public:
    map<string,int> memo;
    vector<TreeNode*> res;
    vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
        string subTree = traverse(root);
        return res;
    }
    string traverse(TreeNode* root) {
        if(root == nullptr) return "#";
        string left = traverse(root->left);
        string right = traverse(root->right);
        string subTree = left + "," + right + "," + to_string(root->val);
        int freq;
        if(!memo.count(subTree)) {
            freq = 0;
        }
        else {
            freq = memo[subTree];
        }
        if(freq == 1) {
            res.push_back(root);
        }
        memo[subTree] = freq + 1;
        return subTree;
    }
};
```

## Binary Tree Build

| Difficulty |                           LeetCode                           | Note |
| :--------: | :----------------------------------------------------------: | :--: |
|     🟠      | [654. Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/) |[654. Maximum Binary Tree](#654-maximum-binary-tree)      |
|     🟠      | [105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/) |[105. Construct Binary Tree from Preorder and Inorder Traversal](#105-construct-binary-tree-from-preorder-and-inorder-traversal)      |
|     🟠      | [106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/) |[106. Construct Binary Tree from Inorder and Postorder Traversal](#106-construct-binary-tree-from-inorder-and-postorder-traversal)      |
|     🟠      | [889. Construct Binary Tree from Preorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/) |[889. Construct Binary Tree from Preorder and Postorder Traversal](#889-construct-binary-tree-from-preorder-and-postorder-traversal)      |

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

---

## Binary Search Tree Basics

| Difficulty |                           LeetCode                           | Note |
| :--------: | :----------------------------------------------------------: | :--: |
|     🟠      | [230. Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/) |      |
|     🟠      | [538. Convert BST to Greater Tree](https://leetcode.com/problems/convert-bst-to-greater-tree/) |      |
|     🟠      | [1038. Binary Search Tree to Greater Sum Tree](https://leetcode.com/problems/binary-search-tree-to-greater-sum-tree/) |      |

### 230. Kth Smallest Element in a BST

Given the `root` of a binary search tree, and an integer `k`, return *the* `kth` *smallest value (**1-indexed**) of all the values of the nodes in the tree*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/28/kthtree1.jpg)

```
Input: root = [3,1,4,null,2], k = 1
Output: 1
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/01/28/kthtree2.jpg)

```
Input: root = [5,3,6,2,4,null,null,1], k = 3
Output: 3
```

 

**Constraints:**

- The number of nodes in the tree is `n`.
- `1 <= k <= n <= 104`
- `0 <= Node.val <= 104`

```cpp
class Solution {
  public:
  int kthSmallest(TreeNode* root, int k) {
    traverse(root, k);
    return res;
  }
  int res = 0;
  int rank = 0;
  void traverse(TreeNode* root, int k) {
    if(root == nullptr) return;
    traverse(root->left, k);
    rank++;
    if(k == rank) {
      res = root->val;
      return;
    }
    traverse(root->right, k);
  }
}
```

---

### 538. Convert BST to Greater Tree

Given the `root` of a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus the sum of all keys greater than the original key in BST.

As a reminder, a *binary search tree* is a tree that satisfies these constraints:

- The left subtree of a node contains only nodes with keys **less than** the node's key.
- The right subtree of a node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2019/05/02/tree.png)

```
Input: root = [4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
Output: [30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]
```

**Example 2:**

```
Input: root = [0,null,1]
Output: [1,null,1]
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `-104 <= Node.val <= 104`
- All the values in the tree are **unique**.
- `root` is guaranteed to be a valid binary search tree.

```cpp
class Solution {
public:
    TreeNode* convertBST(TreeNode* root) {
        traverse(root);
        return root;
    }
    int sum = 0;
    void traverse(TreeNode* root) {
        if(root == nullptr) return;
        traverse(root->right);
        sum += root->val;
        root->val = sum;
        traverse(root->left);
    }
};
```

---

### 1038. Binary Search Tree to Greater Sum Tree

Given the `root` of a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus the sum of all keys greater than the original key in BST.

As a reminder, a *binary search tree* is a tree that satisfies these constraints:

- The left subtree of a node contains only nodes with keys **less than** the node's key.
- The right subtree of a node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2019/05/02/tree.png)

```
Input: root = [4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
Output: [30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]
```

**Example 2:**

```
Input: root = [0,null,1]
Output: [1,null,1]
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 100]`.
- `0 <= Node.val <= 100`
- All the values in the tree are **unique**.

```cpp
class Solution {
public:
    TreeNode* bstToGst(TreeNode* root) {
        traverse(root);
        return root;
    }
    int sum = 0;
    void traverse(TreeNode* root) {
        if(root == nullptr) return;
        traverse(root->right);
        sum += root->val;
        root->val = sum;
        traverse(root->left);
    }
};
```







## Merge Sort

| Difficulty |                           LeetCode                           | Note |
| :--------: | :----------------------------------------------------------: | :--: |
|     🟠      | [912. Sort an Array](#https://leetcode.com/problems/sort-an-array/) |[912. Sort an Array](#912-sort-an-array)      |
|     🔴      | [315. Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/) |[315. Count of Smaller Numbers After Self](#315-count-of-smaller-numbers-after-self)      |
|     🔴      | [493. Reverse Pairs](https://leetcode.com/problems/reverse-pairs/) |[493. Reverse Pairs](#493-reverse-the-pairs)      |
|     🔴      | [327. Count of Range Sum](https://leetcode.com/problems/count-of-range-sum/) |[327. Count of Range Sum](#327-count-of-range-sum)      |

**归并排序本质上就是二叉树的后序遍历**

**所有递归的算法，本质上就是遍历一棵（递归）树，然后再节点（前中后序位置）上执行代码，写递归算法，本质上就是告诉每个节点要干什么**

```cpp
void sort(vector<int>& nums, int lo, int hi) {
  if(lo == hi) return;
  int mid = lo + (hi - lo) / 2;
  sort(nums, lo, mid);
  sort(nums, mid + 1; hi);
  merge(nums, lo, mid, hi);
}
void merge(vector<int>& nums, int lo, int mid, int hi);
```

```cpp
class Merge {
  private:
  vector<int> temp;
  const void sort(vector<int>& nums, int lo, int hi) {
    if(lo == hi) return;
    int mid = lo + (hi - lo) / 2;
    sort(nums, lo, mid);
    sort(nums, mid+1, hi);
    merge(nums, lo, mid, hi);
  }
  const void merge(vector<int>& nums, int lo, int mid, int hi) {
    for(int i = lo; i <= hi; i++) {
      temp[i] = nums[i];
    }
    int i = lo;
    int j = mid + 1;
    for(int p = lo; p <= hi; p++) {
      if(i == mid + 1) {
        nums[p] = temp[j];
        j++;
      }
      else if(j == hi + 1) {
        nums[p] = temp[i];
        i++;
      }
      else if(temp[i] > temp[j]) {
        nums[p] = temp[j];
        j++;
      }
      else {
        nums[p] = temp[i];
        i++;
      }
    }
  }
  public:
  const void sort(vector<int>& nums) {
    temp.resize(nums.size());
    sort(nums, 0, nums.size()-1);
  }
}
```

---

### 912. Sort an Array

Given an array of integers `nums`, sort the array in ascending order.

 

**Example 1:**

```
Input: nums = [5,2,3,1]
Output: [1,2,3,5]
```

**Example 2:**

```
Input: nums = [5,1,1,2,0,0]
Output: [0,0,1,1,2,5]
```

 

**Constraints:**

- `1 <= nums.length <= 5 * 104`
- `-5 * 104 <= nums[i] <= 5 * 104`

```cpp
class Merge {
private:
    vector<int> temp;
    void sort(vector<int>& nums, int lo, int hi) {
        if(lo == hi) return;
        int mid = lo + (hi - lo) / 2;
        sort(nums, lo, mid);
        sort(nums, mid+1, hi);
        merge(nums, lo, mid, hi);
    }
    void merge(vector<int>& nums, int lo, int mid, int hi) {
        for(int i = lo; i <= hi; i++) {
            temp[i] = nums[i];
        }
        int i = lo;
        int j = mid + 1;
        for(int p = lo; p <= hi; p++) {
            if(i == mid + 1) {
                nums[p] = temp[j++];
            }
            else if(j == hi + 1) {
                nums[p] = temp[i++];
            }
            else if(temp[i] > temp[j]) {
                nums[p] = temp[j++];
            }
            else {
                nums[p] = temp[i++];
            }
        }
    }
public:
    void sort(vector<int>& nums) {
        temp.resize(nums.size());
        sort(nums, 0, nums.size()-1);
    }
    
};

class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        Merge merge;
        merge.sort(nums);
        return nums;
    }
};
```

---

### 315. Count of Smaller Numbers After Self

You are given an integer array `nums` and you have to return a new `counts` array. The `counts` array has the property where `counts[i]` is the number of smaller elements to the right of `nums[i]`.

 

**Example 1:**

```
Input: nums = [5,2,6,1]
Output: [2,1,1,0]
Explanation:
To the right of 5 there are 2 smaller elements (2 and 1).
To the right of 2 there is only 1 smaller element (1).
To the right of 6 there is 1 smaller element (1).
To the right of 1 there is 0 smaller element.
```

**Example 2:**

```
Input: nums = [-1]
Output: [0]
```

**Example 3:**

```
Input: nums = [-1,-1]
Output: [0,0]
```

 

**Constraints:**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`

```cpp
struct Pair {
  int val;
  int id;
}
class Solution {
  private:
  vector<Pair> temp;
  vector<int> count;
  void sort(vector<Pair>& arr, int lo, int hi) {
    if(lo == hi) return;
    int mid = lo + (hi - lo) / 2;
    sort(arr, lo, mid);
    sort(arr, mid+1, hi);
    merge(arr, lo, mid, hi);
  }
  void merge(vector<Pair>& arr, int lo, int mid, int hi) {
    for(int i = lo; i <= hi; i++) {
      temp[i] = arr[i];
    }
    int i = lo;
    int j = mid + 1;
    for(int p = lo; p <= hi; p++) {
      if(i == mid + 1) {
        arr[p] = temp[j++];
      }
      else if(j == hi + 1) {
        arr[p] = temp[i++];
        count(arr[p].id) += j - mid - 1;
      }
      else if(temp[i].val > temp[j].val) {
        arr[p] = temp[j++];
      }
      else {
        arr[p] = temp[i++];
        count[arr[p].id] += j - mid - 1;
      }
    }
  }
  public:
  vector<int> countSmaller(vector<int>& nums) {
    int n = nums.size();
    temp.resize(n);
    count.resize(n);
    vector<Pair> arr;
    arr.resize(n);
    for(int i = 0; i < n; i++) {
      Pair pair;
      pair.val = nums[i];
      pair.id = i;
      arr[i] = pair;
    }
    sort(arr, 0, n - 1);
    vector<int> res;
    for(auto c : count) {
      res.push_back(c)
    }
    return res;
  }
}
```

---

### 493. Reverse the Pairs

Given an integer array `nums`, return *the number of **reverse pairs** in the array*.

A reverse pair is a pair `(i, j)` where `0 <= i < j < nums.length` and `nums[i] > 2 * nums[j]`.

 

**Example 1:**

```
Input: nums = [1,3,2,3,1]
Output: 2
```

**Example 2:**

```
Input: nums = [2,4,3,5,1]
Output: 3
```

 

**Constraints:**

- `1 <= nums.length <= 5 * 104`
- `-231 <= nums[i] <= 231 - 1`

```cpp
class Solution {
public:
    int reversePairs(vector<int>& nums) {
        sort(nums);
        return count;
    }
    void sort(vector<int>& nums) {
        temp.resize(nums.size());
        sort(nums, 0, nums.size()-1);
    }
private:
    vector<int> temp;
    int count = 0;
    void sort(vector<int>& nums, int lo, int hi) {
        if(lo == hi) return;
        int mid = lo + (hi-lo)/2;
        sort(nums, lo, mid);
        sort(nums, mid+1, hi);
        merge(nums, lo, mid, hi);
    }
    void merge(vector<int>& nums, int lo, int mid, int hi) {
        for(int i = lo; i <= hi; i++) {
            temp[i] = nums[i];
        }
        int end = mid + 1;
        for(int i = lo; i <= mid; i++) {
            while(end <= hi && (long)nums[i] > (long)nums[end] * 2) {
                end++;
            }
            count += end - (mid + 1);
        };
        int i = lo;
        int j = mid + 1;
        for (int p = lo; p <= hi; p++) {
            if(i == mid + 1) {
                nums[p] = temp[j++];
            }
            else if(j == hi + 1) {
                nums[p] = temp[i++];
            }
            else if(temp[i] < temp[j]) {
                nums[p] = temp[i++];
            }
            else {
                nums[p] = temp[j++];
            }
        }
    }
};
```

---

### 327. Count of Range Sum

Given an integer array `nums` and two integers `lower` and `upper`, return *the number of range sums that lie in* `[lower, upper]` *inclusive*.

Range sum `S(i, j)` is defined as the sum of the elements in `nums` between indices `i` and `j` inclusive, where `i <= j`.

 

**Example 1:**

```
Input: nums = [-2,5,-1], lower = -2, upper = 2
Output: 3
Explanation: The three ranges are: [0,0], [2,2], and [0,2] and their respective sums are: -2, -1, 2.
```

**Example 2:**

```
Input: nums = [0], lower = 0, upper = 0
Output: 1
```

 

**Constraints:**

- `1 <= nums.length <= 105`
- `-231 <= nums[i] <= 231 - 1`
- `-105 <= lower <= upper <= 105`
- The answer is **guaranteed** to fit in a **32-bit** integer.

```cpp
class Solution {
public:
    int countRangeSum(vector<int>& nums, int lower, int upper) {
        this->lower = lower;
        this->upper = upper;
        vector<long> preSum;
        preSum.resize(nums.size()+1);
        for(int i = 0; i < nums.size(); i++) {
            preSum[i+1] = (long)nums[i] + preSum[i];
        }
        sort(preSum);
        return count;
    }
    void sort(vector<long>& nums) {
        temp.resize(nums.size());
        sort(nums, 0, nums.size()-1);
    }
private:
    int lower;
    int upper;
    int count = 0;
    vector<long> temp;
    void sort(vector<long>& nums, int lo, int hi) {
        if(lo == hi) return;
        int mid = lo + (hi - lo) / 2;
        sort(nums, lo, mid);
        sort(nums, mid+1, hi);
        merge(nums, lo, mid, hi);
    }
    void merge(vector<long>& nums, int lo, int mid, int hi) {
        for(int i = lo; i <= hi; i++) {
            temp[i] = nums[i];
        }
        int start = mid + 1;
        int end = mid + 1;
        for(int i = lo; i <= mid; i++) {
            while(start <= hi && nums[start] - nums[i] < lower) {
                start++;
            }
            while(end <= hi && nums[end] - nums[i] <= upper) {
                end++;
            }
            count += (end-start);
        }
        int i = lo;
        int j = mid + 1;
        for(int p = lo; p <= hi; p++) {
            if (i == mid + 1) {
                nums[p] = temp[j++];
            }
            else if(j == hi + 1) {
                nums[p] = temp[i++];
            }
            else if(temp[i] < temp[j]) {
                nums[p] = temp[i++];
            }
            else {
                nums[p] = temp[j++];
            }
        }
    }
};
```

---

## Quick Sort
| Difficulty |                           LeetCode                           | Note |
| :--------: | :----------------------------------------------------------: | :--: |
|     🟠      | [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/) | [215. Kth Largest Element in an Array](#215-kth-largest-element-in-an-array)     |


```cpp
void sort(vector<int>& nums, int lo, int hi) {
  if(lo >= hi) return;
  // 切分nums, um nums[lo..p-1] <= nums[p] < nums[p+1..hi] zu erreichen
  int p = partition(nums, lo, hi);
  sort(nums, lo, p - 1);
  sort(nums, p + 1, hi);
}
// 本质上，就是二叉树的前序遍历
// 或者说， 快速排序本质上是一个构建搜索二叉树的过程
```

```cpp
class Quick {
public:
    void sort(vector<int>& nums) {
        shuffle(nums);
        sort(nums, 0, nums.size()-1);
    }
private:
    void sort(vector<int>& nums, int lo, int hi) {
        if(lo >= hi) return;
        int p = partition(nums, lo, hi);
        sort(nums, lo, p - 1);
        sort(nums, p + 1, hi);
    }
    int partition(vector<int>& nums, int lo, int hi) {
        int pivot = nums[lo];
        int i = lo + 1;
        int j = hi;
        while(i <= j) {
            while( i < hi && nums[i] <= pivot) {
                i++;
            }
            while( j > lo && nums[j] > pivot ) {
                j--;
            }
            if(i >= j) {
                break;
            }
            swap(nums, i, j);
        }
        swap(nums, lo, j);
        return j;
    }
    void shuffle(vector<int>& nums) {
        int n = nums.size();
        for(int i = 0; i < n; i++) {
            int r = i + rand() % (n - i);
            swap(nums, i, r);
        }
    }
    void swap(vector<int>& nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
};
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        Quick quick;
        quick.sort(nums);
        return nums;
    }
};
```

### 215. Kth Largest Element in an Array

Given an integer array `nums` and an integer `k`, return *the* `kth` *largest element in the array*.

Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

 

**Example 1:**

```
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5
```

**Example 2:**

```
Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
Output: 4
```

 

**Constraints:**

- `1 <= k <= nums.length <= 104`
- `-104 <= nums[i] <= 104`

```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        shuffle(nums);
        int lo = 0;
        int hi = nums.size() - 1;
        k = nums.size() - k;
        while(lo <= hi) {
            int p = partition(nums, lo, hi);
            if(p < k) {
                lo = p + 1;
            }
            else if(p > k) {
                hi = p - 1;
            }
            else {
                return nums[p];
            }
        }
        return -1;
    }
    int partition(vector<int>& nums, int lo, int hi) {
        int pivot = nums[lo];
        int i = lo + 1;
        int j = hi;
        while(i <= j) {
            while(i < hi && nums[i] <= pivot) {
                i++;
            }
            while(j > lo && nums[j] > pivot) {
                j--;
            }
            if(i >= j) {
                break;
            }
            swap(nums, i, j);
        }
        swap(nums, lo, j);
        return j;
    }
    void shuffle(vector<int>& nums) {
        int n = nums.size();
        for(int i = 0; i < n; i++) {
            int r = i + rand() % (n - i);
            swap(nums, i, r);
        }
    }
    void swap(vector<int>& nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
};
```

