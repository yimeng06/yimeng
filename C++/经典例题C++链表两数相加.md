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
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *head = nullptr,*tail = nullptr;
        int carry = 0;
        while(l1 || l2)
        {
            int n1=l1 ?l1->val:0; //三元运算，如果l1不是空指针（即还有节点），则n1取l1当前节点的值（l1->val）；如果l1是空指针（即已经遍历完了），则n1取0。
            int n2=l2 ?l2->val:0;
            int sum = n1+n2+carry;
            if(!head) //意思是head为空
            {
                head = tail = new ListNode(sum%10);
            }
            else{
                tail->next = new ListNode(sum%10);
                tail = tail->next;
            }
            carry=sum/10;
            if(l1)
            {
                l1=l1->next;
            }
            if(l2)
            {
                l2=l2->next;
            }
        }
        if(carry>0)
        {
            tail->next = new ListNode(carry);
        }
        return head;
    }
};

例子：
输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807.