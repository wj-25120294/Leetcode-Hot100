# [10.两数之和]
## 这道题目，我用ai检查我一开始的做法，纠正思路后才对，我的思路：创建一个新的链表，把每次加和的数对10取模放进链表，并记录是否要进位，最后把头指针传出即可
### 代码实现：
```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *ans = new ListNode(0);
        ListNode *anss=ans;
        int flag=0;
        while (l1!=nullptr||l2!=nullptr||flag)
        {
            int sum =((l1!=nullptr)?l1->val:0)+((l2!=nullptr)?l2->val:0)+flag;
            flag=sum/10;
            ans->next=new ListNode(sum%10);
            ans=ans->next;
            if (l1!=nullptr)l1=l1->next;
            if (l2!=nullptr)l2=l2->next;
        }
        return anss->next;
    }
};
```
# 这个思路与官方题解差不多。