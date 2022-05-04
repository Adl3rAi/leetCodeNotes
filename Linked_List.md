# Linked List

## Single Linked List

| Difficulty |                           LeetCode                           | Node |
| :--------: | :----------------------------------------------------------: | :--: |
|     🟢      | [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) |  [21. Merge Two Sorted Lists](#21-merge-two-sorted-lists)    |
|     🔴      | [23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) |   [23. Merge k Sorted Lists](#23-merge-k-sorted-lists)   |
|     🟢      | [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/) |      |
|     🟠      | [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/) |      |
|     🟢      | [876. Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/) |      |
|     🟠      | [19. Remove Nth Node From End of Lis](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) |      |
|     🟢      | [160.Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/) |      |

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

