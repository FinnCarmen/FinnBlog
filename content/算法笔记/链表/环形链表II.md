+++
date = '2025-01-25T17:31:08+08:00'
draft = false
title = '环形链表II'
+++

[题目链接](https://leetcode.cn/problems/linked-list-cycle-ii/)

### 哈希

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        unordered_set<ListNode*>visited;
        while(head!=NULL)
        {
            if(visited.count(head))return head;
            visited.insert(head);
            head=head->next;
        }
        return NULL;
    }
};
```

### 双指针

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast = head;
        ListNode* slow = head;
        while(fast!=NULL && fast->next!=NULL)
        {
            slow=slow->next;
            fast=fast->next->next;
            if(fast==slow)
            {
                ListNode* node1=fast;
                ListNode* node2=head;
                while(node1!=node2)
                {
                    node1=node1->next;
                    node2=node2->next;
                }
                return node2;
            }
        }
        return NULL;
    }
};
```
