+++
date = '2025-01-25T09:55:43+08:00'
draft = false
title = '删除链表的倒数第N个节点'
+++

[题目链接](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/description/)

### 正向思维

倒数第 n 个，即为正数第 len-n+1 个

```cpp
class Solution {
public:
    int getlen(ListNode* head)
    {
        int len = 0;
        while(head)
        {
            len++;
            head=head->next;
        }
        return len;
    }

    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(0);
        dummy->next=head;
        int len = getlen(head);
        ListNode* cur = dummy;
        int cnt=len-n;
        int steps = len - n;
        while (steps--)cur = cur->next;
        ListNode* nodeToRemove = cur->next;
        cur->next = cur->next->next;
        delete nodeToRemove;
            ListNode* res = dummy->next;
        delete dummy;
        return res;
    }
};
```

### 快慢指针

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n){
        ListNode* dummy = new ListNode(0);
        dummy->next=head;
        ListNode* fast = dummy;
        ListNode* slow = dummy;
        while(n--){fast=fast->next;}
        while(fast->next)
        {
            fast=fast->next;
            slow=slow->next;
        }
        slow->next=slow->next->next;
        ListNode* res=dummy->next;
        delete dummy;
        return res;
    }
};
```

### 栈

求倒数第 n 个，让人联想到栈的特点

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n){
        stack<ListNode*>st;//*
        ListNode* dummy = new ListNode(0);
        dummy->next=head;
        ListNode* cur = dummy;
        while(cur)
        {
            st.push(cur);
            cur=cur->next;
        }
        while(n--)st.pop();
        //*
        ListNode* pre = st.top();
        pre->next = pre->next->next;
        ListNode* res = dummy->next;
        delete dummy;
        return  res;
    }
};
```
