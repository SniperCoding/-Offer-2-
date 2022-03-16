# [剑指 Offer 22. 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

## 题目描述

>输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。
>
>例如，一个链表有 `6` 个节点，从头节点开始，它们的值依次是 `1、2、3、4、5、6`。这个链表的倒数第 `3` 个节点是值为 `4` 的节点。
>
>**示例：**
>
>```
>给定一个链表: 1->2->3->4->5, 和 k = 2.
>
>返回链表 4->5.
>```

### 想法1：遍历两次链表

第一次遍历链表得到链表长度，而倒数第k个节点就是正数第 length-k个节点，于是再遍历一次链表就可以得到最终结果。

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
    public ListNode getKthFromEnd(ListNode head, int k) {
        // 1.第一次遍历，求出链表长度
        ListNode cur = head;
        int length = 0;
        while(cur!=null){
            length++;
            cur = cur.next;
        }
        // 2.第二次遍历，倒数第k个就是正数第length-k+1个（从1开始）
        cur = head;
        for(int i=1;i<=length-k;i++){
            cur = cur.next;
        }
        return cur;
    }
}
~~~

### 想法2：遍历一次链表（双指针）

 定义两个指针，第一个指针 cur 先遍历链表，知道指向第 k+1 个节点，第二个指针 cur 指向头结点，此时 pre 是 cur 的第前 k 个节点，此时令 pre 与 cur 同时往后遍历，那么当 cur 走到最后时，pre 就是倒数第 k 个节点了。

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
    public ListNode getKthFromEnd(ListNode head, int k) {
        ListNode cur = head; 
        ListNode pre = head;
        // cur 先遍历到第k个节点
        for(int i=1;i<=k;i++){
            cur = cur.next;
        }
 		// 此时pre是cur的第前k个节点，
        // 令pre与cur同时往后遍历，那么当cur走到最后时，pre就是倒数第k个节点了
        while(cur!=null){
            pre = pre.next;
            cur = cur.next;
        }
        return pre;
    }
}
~~~













