# [7.循环链表]
## 由于节点数<10000，所以直接不断读入数据，如果计数的num>10000，肯定有环，反之无。
### 代码实现
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode *p=head;
        int num=0;
        while (p!=nullptr)
        {
            num++;
            p=p->next;
            if (num>10009)return true;
        }
        return false;
    }
};
```
## 官解
## 方法一：哈希表，把地址存进哈希表中，如果有地址在哈希表找到了，说明有环，反之没有，学过，可惜没想到
### 代码实现
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        unordered_set<ListNode *>seen;
        while (head!=nullptr)
        {
            if (seen.count(head))return true;
            seen.insert(head);
            head=head->next;
        }
        return false;
};
}；
```
## 方法二：快慢指针，如果有环，两个指针一定会相遇，反之，快指针会提前指向nullptr
### 代码实现
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        if(head==nullptr||head->next==nullptr)return false;
        ListNode * slow=head;
        ListNode * fast=head->next;
        while(slow!=fast)
        {
            if (fast==nullptr||fast->next==nullptr)
            {
                return false;
            }
            slow = slow->next;
            fast = fast->next->next;
        }
        return true;

};
};
```