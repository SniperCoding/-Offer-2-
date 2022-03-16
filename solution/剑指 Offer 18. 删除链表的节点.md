# [剑指 Offer 18. 删除链表的节点](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)

## 题目描述

>给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。
>
>返回删除后的链表的头节点。
>
>**注意：**此题对比原题有改动
>
>**示例 1:**
>
>```
>输入: head = [4,5,1,9], val = 5
>输出: [4,1,9]
>解释: 给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9.
>```
>
>**示例 2:**
>
>```
>输入: head = [4,5,1,9], val = 1
>输出: [4,5,9]
>解释: 给定你链表中值为 1 的第三个节点，那么在调用了你的函数之后，该链表应变为 4 -> 5 -> 9.
>```
>
>**说明：**
>
>- 题目保证链表中节点的值互不相同
>- 若使用 C 或 C++ 语言，你不需要 `free` 或 `delete` 被删除的节点

## 解法

### 想法：一次遍历

本题较为简单，想要删除一个节点，只需要让被删除节点的前一个节点指向被删除节点的后一个节点即可。唯一需要注意的是需要判断头结点是否为要删除的节点。

时间复杂度O( n )，空间复杂度O( 1 )

~~~java
class Solution {
    public ListNode deleteNode(ListNode head, int val) {
        if(head==null){
            return head;
        }
        if(head.val == val){  // 删除头节点
            return head.next;
        }
        ListNode pre = head;		// 记录前一个节点
        ListNode cur = head.next;	// 记录当前节点
        while(cur!=null){
            if(cur.val == val){  // 当前节点是要被删除的点
                pre.next = cur.next;  // 上一个节点的next指向当前节点的next
                break;
            }
            // 更新节点
            pre = cur;
            cur = cur.next;
        }
        return head;
    }
}
~~~







