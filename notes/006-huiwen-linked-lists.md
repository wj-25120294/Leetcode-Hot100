# 这周末把上周做的五道题重新做了一遍，发现还是有些不会，不过大致思路都会，就是代码还不熟悉。
# [6.回文链表]
## 我一开始的思路是把链表的数存成一个整数，判断整数是否回文，最后发现数据太多，超过int的数据范围
## 代码如下
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
    bool isPalindrome(ListNode* head) {
        ListNode *p = head;
        int num=0;
        while(p!=nullptr)
        {
            num=num*10+p->val;
            p=p->next;
        }
        int reversed_num=0;
        int ans = num;
        while(ans)
        {
            reversed_num=reversed_num*10+ans%10;
            ans=ans/10;
        }
        if (num==reversed_num)return true;
        else return false;
    }
};
```
### 方法一：用数组去存链表的数据，然后分别从头尾遍历，很简单的思路可惜我没想到
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
    bool isPalindrome(ListNode* head) {
        vector<int>nums;
        while (head!=nullptr)
        {
            nums.push_back(head->val);
            head=head->next;
        }
        for (int i =0,j=nums.size()-1;i<j;++i,--j)
        {
            if (nums[i]!=nums[j])return false;
        }
        return true;
};
};
```
### 方法二：用递归到达链表的末节点，提前存下头节点，进行分别比较，一个通过递归回到上个节点，一个通过p=p->next,递归还是太难想了
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
ListNode * frontPointer;
    bool reverseCheck (ListNode * currentNode)
    {
        if (currentNode!=nullptr)
        {
            if (!reverseCheck(currentNode->next))
            {
                return false;
            }
            if (currentNode->val!=frontPointer->val)return false;
            frontPointer=frontPointer->next;
        }
        return true;
    }
    bool isPalindrome(ListNode* head) {
        frontPointer=head;
        return reverseCheck(head); 
};
};
```
### 方法三：用快慢指针来获取链表的中点信息，然后反转后面的链表，逐一比较，最后还原链表，这个思路太妙了
## 代码实现：
```cpp
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if (head == nullptr) {
            return true;
        }

        // 找到前半部分链表的尾节点并反转后半部分链表
        ListNode* firstHalfEnd = endOfFirstHalf(head);
        ListNode* secondHalfStart = reverseList(firstHalfEnd->next);

        // 判断是否回文
        ListNode* p1 = head;
        ListNode* p2 = secondHalfStart;
        bool result = true;
        while (result && p2 != nullptr) {
            if (p1->val != p2->val) {
                result = false;
            }
            p1 = p1->next;
            p2 = p2->next;
        }

        // 还原链表并返回结果
        firstHalfEnd->next = reverseList(secondHalfStart);
        return result;
    }

    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr;
        ListNode* curr = head;
        while (curr != nullptr) {
            ListNode* nextTemp = curr->next;
            curr->next = prev;
            prev = curr;
            curr = nextTemp;
        }
        return prev;
    }

    ListNode* endOfFirstHalf(ListNode* head) {
        ListNode* fast = head;
        ListNode* slow = head;
        while (fast->next != nullptr && fast->next->next != nullptr) {
            fast = fast->next->next;
            slow = slow->next;
        }
        return slow;
    }
};
```