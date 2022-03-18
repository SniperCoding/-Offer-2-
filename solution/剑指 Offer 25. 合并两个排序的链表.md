# [剑指 Offer 25. 合并两个排序的链表](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

## 题目描述

>输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。
>
>**示例1：**
>
>```
>输入：1->2->4, 1->3->4
>输出：1->1->2->3->4->4
>```
>
>**限制：**
>
>```
>0 <= 链表长度 <= 1000
>```

### 想法：双指针

假设新链表为 cur，那么就需要将 l1 和 l2 中的所以节点添加到 cur 中：

- 如果 `l1.val >= l2.val`，也就是 l2 小，那么将 l2 当前节点添加到结果链表中，并且将 l2 往后更新一个节点。
- 如果`l1.val < l2.val`，也就是 l1 小，那么将 l1 当前节点添加到结果链表中，并且将 l1 往后更新一个节点。
- 由于 l1 和 l2 可能有一个先遍历完，那么就可以将另外一个没有遍历完的所有节点直接添加到结果链表中即可。
- 为了方便，可以先定义一个哑结点，之后返回哑结点的后一个节点即可。

时间复杂度O( n )，空间复杂度O( 1 )

~~~java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        // 定义返回结果的哑节点
        ListNode dumb = new ListNode(0);
        ListNode cur  = dumb;

        // 不断比较l1和l2的值
        while(l1!=null && l2!=null){
            if(l1.val>=l2.val){
                cur.next = l2;
                l2 = l2.next;
            }else{
                cur.next = l1;
                l1 = l1.next;
            }
            cur = cur.next;
        }

        // 链表可能有一个先遍历完，一个还没有遍历完
        cur.next = l1!=null ? l1:l2;
        return dumb.next;
    }
}
~~~













