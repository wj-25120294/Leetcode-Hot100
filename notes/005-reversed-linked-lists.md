# [5.反转链表]
## 这道题比较简单，之前上课的作业做到过
### 方法一： 我直接用三个指针存前三个，然后不断更新指针，最终实现反转
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
    ListNode* reverseList(ListNode* head) {
        if (head==nullptr)return nullptr;
        ListNode *p=head;
        ListNode *q=head->next;
        ListNode *o=nullptr;
        p->next=nullptr;
        while (q!=nullptr)
        {
            o=q->next;
            q->next=p;
            p=q;
            q=o;
        }
        return p;
        
    }
};
```
# 还有就是更标准的写法
```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr;
        ListNode* curr = head;
        while (curr) {
            ListNode* next = curr->next; // 保存下一个节点
            curr->next = prev;            // 反转当前节点指向
            prev = curr;                  // 前移
            curr = next;                   // 前移
        }
        return prev; // 新头节点
    }
};
```
### 方法二：官解有个递归的方法，我把他记录下来，学习一下，思路没什么区别
```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (!head || !head->next) {
            return head;
        }
        ListNode* newHead = reverseList(head->next);
        head->next->next = head;
        head->next = nullptr;
        return newHead;
    }
};
```
## 下面是ai的解释，一开始没看懂，对递归还是不理解
[这个程序使用**递归**的方法反转单链表。它的核心思想是：先递归到链表的最后一个节点，然后在回溯的过程中，逐步改变每个节点的指向，最终实现整个链表的反转。

下面以链表 `1 -> 2 -> 3 -> 4 -> 5` 为例，详细解释每一步的执行过程。

---

### 递归调用阶段（递推）

函数 `reverseList(ListNode head)` 会不断调用自身，传入 `head.next`，直到链表末尾。

1. **第一次调用**：`head = 1`  
   因为 `head.next` 不为空，执行 `reverseList(2)`，并等待返回。

2. **第二次调用**：`head = 2`  
   执行 `reverseList(3)`，等待返回。

3. **第三次调用**：`head = 3`  
   执行 `reverseList(4)`，等待返回。

4. **第四次调用**：`head = 4`  
   执行 `reverseList(5)`，等待返回。

5. **第五次调用**：`head = 5`  
   此时 `head.next == null`，满足递归终止条件，直接返回 `head`（即节点5）。  
   所以第五次调用返回节点5。

---

### 回溯阶段（回归）

现在开始从最深处逐步返回，每一层都会执行反转操作。

#### 第四层返回（`head = 4`）
- 上一层的递归调用 `reverseList(5)` 返回了节点5，赋值给 `newHead`，此时 `newHead` 指向节点5。
- 当前 `head` 指向节点4，`head.next` 指向节点5。
- 执行 `head.next.next = head;` 即 `5.next = 4`，此时节点5指向节点4，形成 `4 <-> 5` 的双向链接（两个节点互相指向）。
- 然后执行 `head.next = null;` 即 `4.next = null`，断开原来4指向5的指针，现在只有5指向4，即 `4 <- 5`。
- 返回 `newHead`（节点5）给上一层。

此时链表状态：`1 -> 2 -> 3 -> 4 <- 5`，节点5成为当前反转后的头节点（但尚未完全反转）。

#### 第三层返回（`head = 3`）
- `newHead` 仍然是节点5（由上一层返回）。
- 当前 `head` 指向节点3，`head.next` 指向节点4。
- 执行 `head.next.next = head;` 即 `4.next = 3`，此时节点4指向节点3，形成 `3 <-> 4 <- 5`。
- 执行 `head.next = null;` 即 `3.next = null`，断开3指向4的指针，现在只有4指向3，即 `3 <- 4 <- 5`。
- 返回节点5。

链表状态：`1 -> 2 -> 3 <- 4 <- 5`

#### 第二层返回（`head = 2`）
- `newHead` 仍为节点5。
- 当前 `head` 指向节点2，`head.next` 指向节点3。
- 执行 `head.next.next = head;` 即 `3.next = 2`，形成 `2 <-> 3 <- 4 <- 5`。
- 执行 `head.next = null;` 即 `2.next = null`，断开2指向3的指针，现在 `2 <- 3 <- 4 <- 5`。
- 返回节点5。

链表状态：`1 -> 2 <- 3 <- 4 <- 5`

#### 第一层返回（`head = 1`）
- `newHead` 仍为节点5。
- 当前 `head` 指向节点1，`head.next` 指向节点2。
- 执行 `head.next.next = head;` 即 `2.next = 1`，形成 `1 <-> 2 <- 3 <- 4 <- 5`。
- 执行 `head.next = null;` 即 `1.next = null`，断开1指向2的指针，最终得到 `1 <- 2 <- 3 <- 4 <- 5`。
- 返回节点5。

此时整个链表反转完成，新头节点为5。

---

### 关键点总结
- **递归终止条件**：当节点为 `null` 或只有一个节点时，直接返回该节点。
- **回溯操作**：每一层都将当前节点的下一个节点的 `next` 指向当前节点（即反转方向），然后将当前节点的 `next` 设为 `null`（断开原指向，避免形成环）。
- **返回值**：每一层都返回最底层传上来的新头节点（原链表的最后一个节点），最终该节点成为反转后的头节点。

这种递归方法虽然代码简洁，但深度受链表长度限制（可能导致栈溢出），不过对于理解递归思想很有帮助。]