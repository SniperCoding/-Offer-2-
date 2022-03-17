# [剑指 Offer 24. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)

## 题目描述

>定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。
>
>**示例:**
>
>```
>输入: 1->2->3->4->5->NULL
>输出: 5->4->3->2->1->NULL
>```
>
>**限制：**
>
>```
>0 <= 节点个数 <= 5000
>```

### 想法：双指针

对链表进行反转，需要让遍历到的每个节点指向其前一个节点，因此我们需要定义两个指针，一个指向当前节点 cur，一个指向当前节点的前一个节点 pre。当我们令 `cur.next = pre`后，原先当前节点的后一个节点就被断开了，无法再遍历原先的链表，因此我们可以先将 cur.next 存起来，然后再令 cur.next = pre 即可。

时间复杂度O( n )，空间复杂度O( 1 )

~~~java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode pre = null;    // 记录前一个节点
        ListNode cur = head;    // 记录当前节点
        while(cur!=null){
            ListNode temp = cur.next;  // 记录下一个节点
            cur.next = pre;  // 当前节点指向前一个节点
            // 更新
            pre = cur;
            cur = temp;
        }
        return pre;
    }
}
~~~













