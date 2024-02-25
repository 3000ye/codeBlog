---
weight: 1
title: LeetCode
math: true
---

# Linked List

## 21. Merge Two Sorted Lists

### 题面

有两个链表，其中的节点都按照节点值有序排序，请你将这两个链表合并为一个有序链表。

### 题解

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

## 第二题


# Stack