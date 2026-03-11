# [8.环形链表二]
## 思路：采用上一题的哈希表，不过将true变为head，false变为nullptr即可
### 代码实现：
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
    ListNode *detectCycle(ListNode *head) {
            unordered_set<ListNode*> seen;
            while (head != nullptr) {
                if (seen.count(head)) {
                    return head;
                }
            seen.insert(head);
            head = head->next;
            }
            return nullptr;
    }
};
```
## 方法二：快慢指针
### 通过数学推导得到关系，我感觉我想不到，有点难
## 代码实现：
```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode *slow = head, *fast = head;
        while (fast != nullptr) {
            slow = slow->next;
            if (fast->next == nullptr) {
                return nullptr;
            }
            fast = fast->next->next;
            if (fast == slow) {
                ListNode *ptr = head;
                while (ptr != slow) {
                    ptr = ptr->next;
                    slow = slow->next;
                }
                return ptr;
            }
        }
        return nullptr;
    }
};
```