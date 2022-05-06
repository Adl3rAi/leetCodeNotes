# Linked List

## Single Linked List

| Difficulty |                           LeetCode                           |                           Note                           |
| :--------: | :----------------------------------------------------------: | :------------------------------------------------------: |
|     🟢      | [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) | [21. Merge Two Sorted Lists](#21-merge-two-sorted-lists) |
|     🔴      | [23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) |   [23. Merge k Sorted Lists](#23-merge-k-sorted-lists)   |
|     🟢      | [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/) |                      [141. Linked List Cycle](#141-linked-list-cycle)                                    |
|     🟠      | [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/) | [142. Linked List Cycle](#142-linked-list-cycle-ii)                                                          |
|     🟢      | [876. Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/) | [876. Middle of the Linked List](#876-middle-of-the-linked-list)                                                         |
|     🟠      | [19. Remove Nth Node From End of Lis](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) |  [19. Remove Nth Node From End of Lis](#19-remove-nth-node-from-end-of-list)                                                        |
|     🟢      | [160.Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/) |          [160.Intersection of Two Linked Lists](#160-intersection-of-two-linked-lists)                                                |

*2 Pointers* is very universal in Single Linked List, with the comparion between two values and the moving speeds(slow pointer and fast pointer)

### 21. Merge Two Sorted Lists

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists in a one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return *the head of the merged linked list*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)

```
Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]
```

**Example 2:**

```
Input: list1 = [], list2 = []
Output: []
```

**Example 3:**

```
Input: list1 = [], list2 = [0]
Output: [0]
```

 

**Constraints:**

- The number of nodes in both lists is in the range `[0, 50]`.
- `-100 <= Node.val <= 100`
- Both `list1` and `list2` are sorted in **non-decreasing** order.

---

```cpp
ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
  ListNode* dummy = new LisNode();
  dummy->val = -1;
  ListNode* p = dummy;
  ListNode* p1 = list1;
  ListNode* p2 = list2;
  while(p1 != nullptr && p2 != nullptr) {
    if(p1->val <= p2->val) {
      p->next = p1;
      p1 = p1->next;
    }
    else {
      p->next = p2;
      p2 = p2->next;
    }
    p = p->next;
  }
  if(p1 != nullptr) p->next = p1;
  if(p2 != nullptr) p->next = p2;
  return dummy->next;
}
```

### 23. Merge k Sorted Lists

You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

*Merge all the linked-lists into one sorted linked-list and return it.*

 

**Example 1:**

```
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6
```

**Example 2:**

```
Input: lists = []
Output: []
```

**Example 3:**

```
Input: lists = [[]]
Output: []
```

 

**Constraints:**

- `k == lists.length`
- `0 <= k <= 104`
- `0 <= lists[i].length <= 500`
- `-104 <= lists[i][j] <= 104`
- `lists[i]` is sorted in **ascending order**.
- The sum of `lists[i].length` will not exceed `104`.

---

The first thing we need to do is to sort values in different linked lists(`priority_queue`)

The compare must be rewritten

```cpp
ListNode* mergeKLists(vector<ListNode*>& lists) {
  priority_queue<ListNode*, vector<ListNode*>, cmp> pq;
  for(ListNode* head : lists) {
    if(head != nullptr) {
      pq.push(head);
    }
  }
  ListNode* dummy = new ListNode();
  ListNode* p = dummy;
  while(!pq.empty()) {
    ListNode* top = pq.top();
    p->next = top;
    p = p->next;
    pq.pop();
    if(top->next) {
      pq.push(top->next);
    }
  }
  return dummy->next;
}
struct cmp {
  bool operator()(const ListNode* a, const ListNode* b) {
    return a->val > b->val;
  }
};
```

### 141. Linked List Cycle

Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that `pos` is not passed as a parameter**.

Return `true` *if there is a cycle in the linked list*. Otherwise, return `false`.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

```
Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 0th node.
```

**Example 3:**

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

```
Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
```

 

**Constraints:**

- The number of the nodes in the list is in the range `[0, 104]`.
- `-105 <= Node.val <= 105`
- `pos` is `-1` or a **valid index** in the linked-list.

---

```cpp
bool hasCycle(ListNode* head) {
  ListNode* slow = head;
  ListNode* fast = head;
  while(fast != nullptr && fast->next != null) {
    slow = slow->next;
    fast = fast->next->next;
    if(fast == slow) {
      return true;
    }
  }
  return false;
}
```

**Notice**: the fast pointer moves forward in 2 steps. If the fast pointer is the last node in the linked list, which can only move to the `nullptr`, so the loop condition will be `fast->next != nullptr`

### 142 Linked List Cycle II

Given the `head` of a linked list, return *the node where the cycle begins. If there is no cycle, return* `null`.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to (**0-indexed**). It is `-1` if there is no cycle. **Note that** `pos` **is not passed as a parameter**.

**Do not modify** the linked list.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

```
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

```
Input: head = [1,2], pos = 0
Output: tail connects to node index 0
Explanation: There is a cycle in the linked list, where tail connects to the first node.
```

**Example 3:**

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

```
Input: head = [1], pos = -1
Output: no cycle
Explanation: There is no cycle in the linked list.
```

 

**Constraints:**

- The number of the nodes in the list is in the range `[0, 104]`.
- `-105 <= Node.val <= 105`
- `pos` is `-1` or a **valid index** in the linked-list.

---

```cpp
ListNode* detectCycle(listNode* head) {
  ListNode* slow = head;
  ListNode* fast = head;
  while(fast != nullptr && fast->next != nullptr) {
    slow = slow->next;
    fast = fast->next->next;
    if(slow == fast) {
      break;
    }
  }
  if(fast == nullptr || fast->next == nullptr) return nullptr;
  else{
    slow = head;
    while(slow != fast) {
      slow = slow->next;
      fast = fast->next;
    }
    return fast;
  }
}
```

### 876. Middle of the Linked List

Given the `head` of a singly linked list, return *the middle node of the linked list*.

If there are two middle nodes, return **the second middle** node.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/07/23/lc-midlist1.jpg)

```
Input: head = [1,2,3,4,5]
Output: [3,4,5]
Explanation: The middle node of the list is node 3.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/07/23/lc-midlist2.jpg)

```
Input: head = [1,2,3,4,5,6]
Output: [4,5,6]
Explanation: Since the list has two middle nodes with values 3 and 4, we return the second one.
```

 

**Constraints:**

- The number of nodes in the list is in the range `[1, 100]`.
- `1 <= Node.val <= 100`

---

```cpp
ListNode* middleNode(ListNode* head) {
  ListNode* slow = head;
  ListNode* fast = head;
  while(fast != nullptr && fast->next != nullptr) {
    slow = slow->next;
    fast = fast->next->next;
  }
  return slow;
}
```

### 19. Remove Nth Node From End of List

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```

**Example 2:**

```
Input: head = [1], n = 1
Output: []
```

**Example 3:**

```
Input: head = [1,2], n = 1
Output: [1]
```

 

**Constraints:**

- The number of nodes in the list is `sz`.
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

---

```cpp
ListNode* removeNthFromEnd(ListNode* head, int n) {
  ListNode* dummy = new ListNode();
  dummy->val = -1;
  dummy->next = head;
  ListNode* p = findFromEnd(dummy, n + 1);
  p->next = p->next->next;
  return dummy->next;
}
ListNode* findFromEnd(ListNode* head, int k) {
  ListNode* p1 = head;
  for(int i = 0; i < k; i++) {
    p1 = p1->next;
  }
  ListNode* p2 = head;
  while(p1 != nullptr) {
    p2 = p2->next;
    p1 = p1->next;
  }
  return p2;
}
```

pointer `dummy` solve the problem that wants to delete the first node

### 160. Intersection of Two Linked Lists

Given the heads of two singly linked-lists `headA` and `headB`, return *the node at which the two lists intersect*. If the two linked lists have no intersection at all, return `null`.

For example, the following two linked lists begin to intersect at node `c1`:

![img](https://assets.leetcode.com/uploads/2021/03/05/160_statement.png)

The test cases are generated such that there are no cycles anywhere in the entire linked structure.

**Note** that the linked lists must **retain their original structure** after the function returns.

**Custom Judge:**

The inputs to the **judge** are given as follows (your program is **not** given these inputs):

- `intersectVal` - The value of the node where the intersection occurs. This is `0` if there is no intersected node.
- `listA` - The first linked list.
- `listB` - The second linked list.
- `skipA` - The number of nodes to skip ahead in `listA` (starting from the head) to get to the intersected node.
- `skipB` - The number of nodes to skip ahead in `listB` (starting from the head) to get to the intersected node.

The judge will then create the linked structure based on these inputs and pass the two heads, `headA` and `headB` to your program. If you correctly return the intersected node, then your solution will be **accepted**.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/05/160_example_1_1.png)

```
Input: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
Output: Intersected at '8'
Explanation: The intersected node's value is 8 (note that this must not be 0 if the two lists intersect).
From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,6,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/03/05/160_example_2.png)

```
Input: intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
Output: Intersected at '2'
Explanation: The intersected node's value is 2 (note that this must not be 0 if the two lists intersect).
From the head of A, it reads as [1,9,1,2,4]. From the head of B, it reads as [3,2,4]. There are 3 nodes before the intersected node in A; There are 1 node before the intersected node in B.
```

**Example 3:**

![img](https://assets.leetcode.com/uploads/2021/03/05/160_example_3.png)

```
Input: intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
Output: No intersection
Explanation: From the head of A, it reads as [2,6,4]. From the head of B, it reads as [1,5]. Since the two lists do not intersect, intersectVal must be 0, while skipA and skipB can be arbitrary values.
Explanation: The two lists do not intersect, so return null.
```

 

**Constraints:**

- The number of nodes of `listA` is in the `m`.
- The number of nodes of `listB` is in the `n`.
- `1 <= m, n <= 3 * 104`
- `1 <= Node.val <= 105`
- `0 <= skipA < m`
- `0 <= skipB < n`
- `intersectVal` is `0` if `listA` and `listB` do not intersect.
- `intersectVal == listA[skipA] == listB[skipB]` if `listA` and `listB` intersect.

---

```cpp
ListNode* getIntersectionNode(ListNode* headA, ListNode* headB) {
  ListNode* p1 = headA;
  ListNode* p2 = headB;
  while(p1 != p2) {
    if(p1 != nullptr) {
      p1 = p1->next;
    }
    else {
      p1 = headB;
    }
    if(p2 != nullptr) {
      p2 = p2->next;
    }
    else {
      p2 = headA;
    }
  }
  return p1;
}
```

## Reverse the Linked List

| Difficulty |                           LeetCode                           | Note |
| :--------: | :----------------------------------------------------------: | :--: |
|     🟢      | [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/) |  [206. Reverse Linked List](#206-reverse-linked-list)    |
|     🟠      | [92. Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/) |  [92. Reverse Linked List II](#92-reverse-linked-list-ii)    |
|     🔴      | [25. Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/) |   [25. Reverse Nodes in k-Group](#25-reverse-nodes-in-k-group)   |

### 206. Reverse Linked List

Given the `head` of a singly linked list, reverse the list, and return *the reversed list*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

```
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

```
Input: head = [1,2]
Output: [2,1]
```

**Example 3:**

```
Input: head = []
Output: []
```

 

**Constraints:**

- The number of nodes in the list is the range `[0, 5000]`.
- `-5000 <= Node.val <= 5000`

---

```cpp
ListNode* reverseList(ListNode* head) {
  ListNode* prev = nullptr;
  ListNode* curr = head;
  while(curr != nullptr) {
    ListNode* next = curr->next;
    curr->next = prev;
    prev = curr;
    curr = next;
  }
  return prev
}
```

### 92. Reverse Linked List II

Given the `head` of a singly linked list and two integers `left` and `right` where `left <= right`, reverse the nodes of the list from position `left` to position `right`, and return *the reversed list*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev2ex2.jpg)

```
Input: head = [1,2,3,4,5], left = 2, right = 4
Output: [1,4,3,2,5]
```

**Example 2:**

```
Input: head = [5], left = 1, right = 1
Output: [5]
```

 

**Constraints:**

- The number of nodes in the list is `n`.
- `1 <= n <= 500`
- `-500 <= Node.val <= 500`
- `1 <= left <= right <= n`

---

`Reverse(ListNode* head)`

`ReverseN(ListNode* head, int k)`

`Reverse(ListNode* head, int left, int right)`

```cpp
ListNode* succ = nullptr;
ListNode* reverseBetween(ListNode* head, int left, int right) {
	if(left == 1) return reverseN(head, right);
  else {
    head->next = reverseBetween(head->next, left-1, right-1);
  	return head;
  } 
}
ListNode* reverseN(ListNode* head, int n) {
  if(n == 1) {
    succ = head->next;
    return head;
  }
  ListNode* last = reverseN(head->next, n-1);
  head->next->next = head;
  head->next = succ;
  return last;
}
```

### 25. Reverse Nodes in k-Group

Given the `head` of a linked list, reverse the nodes of the list `k` at a time, and return *the modified list*.

`k` is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of `k` then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex1.jpg)

```
Input: head = [1,2,3,4,5], k = 2
Output: [2,1,4,3,5]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex2.jpg)

```
Input: head = [1,2,3,4,5], k = 3
Output: [3,2,1,4,5]
```

 

**Constraints:**

- The number of nodes in the list is `n`.
- `1 <= k <= n <= 5000`
- `0 <= Node.val <= 1000`

---

```cpp
ListNode* reverseKGroup(ListNode* head, int k) {
	ListNode* a = head;
  ListNode* b = head;
  for(int i = 0; i < k; i++) {
  	if(b == nullptr) return head;
    b = b->next;
  }
  ListNode* newHead = reverse(a, b);
  a->next = reverseKGroup(b, k);
  return newHead;
}
ListNode* reverse(ListNode* a, ListNode* b) {
  ListNode* prev = b;
  ListNode* curr = a;
  while(curr != b) {
    ListNode* nxt = curr->next;
    curr->next = prev;
    prev = curr;
    curr = nxt;
  }
  return prev;
}
```



