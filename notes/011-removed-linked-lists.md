# [11.删除链表的倒数第N个结点]
## 依旧不会写，觉得递归应该可以做
### 方法一：计算整个链表长度之后，再进行删除
## 代码实现：
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode *p=head;
        int h=0;
        while (p!=nullptr)
        {
            h++;
            p=p->next;
        }
        ListNode *q=new ListNode(0,head);
        ListNode * o=q;
        for (int i=1;i<h-n+1;++i)
        {
            o=o->next;
        }
        o->next=o->next->next;
        ListNode* ans=q->next;
        delete q;
        return ans;
    }
};
```
## 方法二：双指针，让快慢指针差n个结点，最后，当快指针到达nullptr，慢指针刚好到需要删除的点。
## 代码实现
```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(0, head);
        ListNode* first = head;
        ListNode* second = dummy;
        for (int i = 0; i < n; ++i) {
            first = first->next;
        }
        while (first) {
            first = first->next;
            second = second->next;
        }
        second->next = second->next->next;
        ListNode* ans = dummy->next;
        delete dummy;
        return ans;
    }
};
```
