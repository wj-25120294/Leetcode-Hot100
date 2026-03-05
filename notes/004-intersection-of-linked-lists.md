# 昨天把hot100中的哈希表部分做完（抄完）了，感觉还是没怎么掌握，之后应该会二刷，再找一些相关题巩固一下，今天开始链表的题目，对于链表相关的知识点，已经在大一第一学期学习过了，就不在回顾，直接在题目里复习
## 如何历遍链表里的每个值
# 代码实现
```cpp
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(nullptr) {}
};

void traverse(ListNode* head) {
    ListNode* current = head;
    while (current != nullptr) {
        cout << current->val << endl; // 访问值
        current = current->next;
    }
}
```
## 自己做时，没太理解题目意思，认为这个题目就是比较地址是否相同，就想到用两个while历遍链表，最后大错特错
# 错误代码
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
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        struct ListNode *A =headA;
        struct ListNode *B =headB;
        while (A!=NULL)
        {
            while (B!=NULL)
            {
                if (&A==&B)
                {
                    return A;
                }
                B=B->next;
            }
            A=A->next;
        }
        return NULL;
    }
};
```
### 错因分析：找ai纠错，发现A,B本身就是地址，我还加了个&，同时结束内层循环忘记重置B了，改完之后过了，但是效率太低，打算学习别人的高效解法。
## 方法一 双层循环历遍
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
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        struct ListNode *A =headA;
        struct ListNode *B =headB;
        while (A!=NULL)
        {
            while (B!=NULL)
            {
                if (A==B)
                {
                    return A;
                }
                B=B->next;
            }
            B =headB;
            A=A->next;
        }
        return NULL;
    }
};
```
## 方法二 哈希表
### 看到就懂了，把A节点地址存进哈希表里面，当遍历B第一次找到在哈希表里面的节点，就是相交节点
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
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
       unordered_set<ListNode *>visited;
       ListNode *temp=headA;
       while (temp !=nullptr)
       {
        visited.insert(temp);
        temp=temp->next;
       }
       temp =headB;
       while (temp !=nullptr)
       {
        if (visited.count(temp))
        {
            return temp;
        }
        temp=temp->next;
       }
       return nullptr;
    }
};
```
### 方法三 双指针
## 先学习一下知识点：发现本质就是设置两个指针移动，不过使用方面有不少。
1.快慢指针 更新速度不同
2.左右指针 两者最终会相遇
3.同步指针 更新速度相同
# 这个题目使用双指针分别用pa，pb遍历链表A,B,这样会出现一个问题就是，因为更新速度一样，会导致相交的节点刚好错过，所以想到当pa遍历完A后，让其继续遍历B,，pb同理，这样两者的步长会统一到相同，最后两者在相交节点相遇。
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
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
       if (headA==nullptr||headB==nullptr)
       {
        return nullptr;
       }
       ListNode *pA = headA,*pB = headB;
       while (pA!= pB)
       {
        pA = pA == nullptr ? headB : pA->next;
        pB = pB == nullptr ? headA : pB->next;
       }
        return pA;
    }
};
```