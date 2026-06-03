# 经典例题：两数相加（链表版）

## 问题描述

**LeetCode No.2** - 两个逆序链表表示的数相加，返回和的链表。

### 示例

```
输入：l1 = [2,4,3], l2 = [5,6,4]   （表示 342 + 465）
输出：[7,0,8]                        （表示 807）
```

---

## 核心思路

**模拟竖式加法**：从低位到高位逐位相加，处理进位。

```
  2 → 4 → 3   (342)
+ 5 → 6 → 4   (465)
─────────────
  7 → 0 → 8   (807)
```

**关键点**：
1. 遍历两个链表，对应位相加
2. 当前位 = `(n1 + n2 + carry) % 10`
3. 进位 = `(n1 + n2 + carry) / 10`
4. 短链表高位补0（空节点视为0）
5. 最后检查是否还有进位

---

## 完整代码

```cpp
struct ListNode {
    int val;
    ListNode *next;
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode *next) : val(x), next(next) {}
};

class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *head = nullptr, *tail = nullptr;
        int carry = 0;

        while (l1 || l2) {
            int n1 = l1 ? l1->val : 0;   // 空指针取0
            int n2 = l2 ? l2->val : 0;
            int sum = n1 + n2 + carry;

            if (!head) {
                head = tail = new ListNode(sum % 10);   // 首节点
            } else {
                tail->next = new ListNode(sum % 10);    // 尾插法
                tail = tail->next;
            }

            carry = sum / 10;           // 更新进位
            if (l1) l1 = l1->next;      // 指针后移
            if (l2) l2 = l2->next;
        }

        if (carry > 0)                  // 处理最后进位
            tail->next = new ListNode(carry);

        return head;
    }
};
```

---

## 关键技术点

### 1. 尾插法构建链表

```cpp
if (!head) {
    head = tail = new ListNode(val);     // 第一个节点
} else {
    tail->next = new ListNode(val);      // 后续挂在tail后面
    tail = tail->next;                   // tail后移
}
```
用`tail`记录末尾位置，插入O(1)，避免每次遍历找尾。

### 2. 空指针安全处理

```cpp
int n1 = l1 ? l1->val : 0;   // 三元运算符，空则取0
int n2 = l2 ? l2->val : 0;
```
短链表遍历完后，高位自动补零。

### 3. 进位处理

```cpp
int sum = n1 + n2 + carry;
int digit = sum % 10;      // 当前位（0-9）
carry = sum / 10;          // 进位（0或1）
```
如 `9+8=17`：digit=7, carry=1。

### 4. 最终进位不能漏

```cpp
if (carry > 0)
    tail->next = new ListNode(carry);
```
否则 `999+1` 会错误返回 `[0,0,0]` 而非 `[0,0,0,1]`。

---

## 复杂度分析

| 指标 | 复杂度 |
|:----:|:------:|
| 时间 | O(max(m,n)) |
| 空间 | O(max(m,n)) |

m、n为两个链表长度，结果最多多一位（进位）。

---

## 常见错误

| 错误 | 后果 | 正确做法 |
|:-----|:-----|:---------|
| 忘记最后处理carry | `999+1`结果少一位 | 循环外单独判断 |
| 未判空直接访问`l1->val` | 程序崩溃 | 用三元运算符保护 |
| 指针忘记后移`l1=l1->next` | 死循环 | 每轮迭代必须移动 |
| 只维护tail丢失head | 返回尾节点 | 同时维护head和tail |

---

## 优化写法（哨兵节点）

```cpp
ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
    ListNode dummy(0);              // 哨兵节点，简化首节点判断
    ListNode* tail = &dummy;
    int carry = 0;

    while (l1 || l2 || carry) {    // 统一条件，包含carry
        int sum = (l1?l1->val:0) + (l2?l2->val:0) + carry;
        tail->next = new ListNode(sum % 10);
        tail = tail->next;
        carry = sum / 10;
        if (l1) l1 = l1->next;
        if (l2) l2 = l2->next;
    }

    return dummy.next;             // 跳过哨兵，返回真正的头
}
```

**优势**：消除if-else，代码更简洁。

---

## 总结

- **算法本质**：模拟竖式加法，逐位计算+进位
- **核心技巧**：尾插法建链、空指针保护、进位处理
- **易错点**：最后的carry、指针移动、head/tail双指针
- **一句话**：两个链表同时遍历，按位相加带进位，尾插法构建结果。
