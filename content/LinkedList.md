---
weight: 1
title: Linked List
math: true
---

# Linked List

## 21. Merge Two Sorted Lists

```cpp
typedef ListNode* ListLink;

class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        if (list1 == nullptr) return list2;
        if (list2 == nullptr) return list1;

        // 将头节点较小的链表放在 list1
        if (list1->val > list2->val) swap(list1, list2);

        // 遍历 list2，按大小插入 list1
        auto ls1 = list1, ls2 = list2;
        while (ls2 != nullptr) {
            if (ls1->next != nullptr and ls2->val > ls1->next->val) {}
            else {  // 插入节点
                ListLink temp = new ListNode(ls2->val, ls1->next);
                ls1->next = temp;
                ls2 = ls2->next;
            }

            ls1 = ls1->next;
        }

        return list1;
    }
};
```

```c
typedef ListNode* ListLink;

class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListLink res = new ListNode(0);

        auto p = res, p1 = list1, p2 = list2;
        while (p1 and p2) {
            if (p1->val < p2->val) {
                p->next = p1;
                p1 = p1->next;
            }
            else {
                p->next = p2;
                p2 = p2->next;
            }

            p = p->next;
        }

        while (p1) {
            p->next = p1;
            p1 = p1->next;
            p = p->next;
        }

        while (p2) {
            p->next = p2;
            p2 = p2->next;
            p = p->next;
        }

        return res->next;
    }
};
```

```java
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        if (list1 == nullptr) return list2;
        if (list2 == nullptr) return list1;

        if (list1->val <= list2->val) {
			list1->next = mergeTwoLists(list1->next, list2);
			return list1;
		}
		else {
			list2->next = mergeTwoLists(list1, list2->next);
			return list2;
		}
    }
};
```

### 题面

有两个链表，其中的节点都按照节点值有序排序，请你将这两个链表合并为一个有序链表。

### 题解

**Method One**: 直接法

- 将 `head->val` 较小的链表作为 `list1`，放在前面被插入
- 遍历 `list2` 按照 `node->val` 的大小依次插入 `list1`
- 需特判链表为空的情况

**Method Two**: 双指针

- 构造一个虚拟头节点 `res`，并创建指针 `p`
- 分别为两个链表创建指针 `p1` 和 `p2`
- 遍历 `p1` 和 `p2`，直到有一个走到终点
- 将没有走完的指针走完

**Method Three**: 递归

- 思想和直接法一样，只是插入的逻辑有变化
- 直接法是始终往一个链表插入
- 递归法是根据当前节点的 `val` 来判断插入方向

## 83. Remove Duplicates from Sorted List

```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        auto p = head;
        while (p != nullptr and p->next != nullptr) {
            if (p->val == p->next->val) {
                p->next = p->next->next;
            }
            else p = p->next;
        }

        return head;
    }
};
```

### 题面

给你一个有序的链表，请删除其中的重复节点。

### 题解

**Method One**: 直接法

- 因为链表是有序的，所以直接遍历链表
- 如果 `p->val == p->next->val` 则删除 `p->next` 节点
- 否则继续前进

## 141. Linked List Cycle

```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {
        auto fast = head, slow = head;
        while (fast and fast->next) {
            fast = fast->next->next;
            slow = slow->next;

            if (fast == slow) return true;
        }

        return false;
    }
};
```

```c
class Solution {
public:
    bool hasCycle(ListNode *head) {
        if (!head) return false;

        int cnt = 0;
        while (cnt < 10010) {
            if (head->next == nullptr) return false;

            head = head->next;
            cnt ++;
        }

        return true;
    }
};
```

### 题面

给你一个链表，请判断链表中是否存在环。

### 题解

**Method One**: 快慢指针

- 若链表中存在环，则遍历链表是一个死循环
- 因此我们构造两个快慢指针 `fast` 和 `slow`
- `fast` 每次前进 2 步，`slow` 每次前进 1 步
- 如果存在环，则 `fast` 和 `slow` 会相遇

**Method Two**: 暴力

- 注意到题目的数据量为 `1e4`
- 因此我们可以直接前进 `10010` 步，判断是否走到终点

## 160. Intersection of Two Linked Lists

```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        auto p = headA, q = headB;
        while (p != q) {
            if (!p) p = headB;
            else p = p->next;

            if (!q) q = headA;
            else q = q->next;
        }

        return p;
    }
};
```

### 题面

有两个链表 `headA` 和 `headB`，请你找出它们尾部相同的部分。

### 题解

**Method One**: 双指针

- `headA` 和 `headB` 长度不一致，但是尾部相同
- 则有 `headA = preA + tail`， `headB = preB + tail`
- 即 `headA + headB = x + tail`，`headB + headA = y + tail`
- 因此直接找 `headA + headB` 和 `headB + headA` 的相同尾部

## 203. Remove Linked List Elements

```cpp
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        while (head and head->val == val) head = head->next;

        auto p = head;
        while (p and p->next) {
            if (p->next->val == val) p->next = p->next->next;
            else p = p->next;
        }

        return head;
    }
};
```

### 题面

给你一个链表 `head` 和一个整数 `val`，请删除链表中 `p->val == val` 的节点。

### 题解

**Method One**: 直接法

- 首先特判头节点的值 `head->val` 是否等于 `val`
- 然后遍历链表，将 `p->val == val` 的节点删掉
