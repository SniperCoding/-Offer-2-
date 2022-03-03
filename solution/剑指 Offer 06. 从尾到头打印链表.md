# [剑指 Offer 06. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

## 题目描述

>输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。
>
>**示例 1：**
>
>```
>输入：head = [1,3,2]
>输出：[2,3,1]
>```
>
>**限制：**
>
>```
>0 <= 链表长度 <= 10000
>```

## 解法

### 想法1：使用列表或栈进行存储

以列表为例，遍历链表，将所有元素存放在列表中，然后定义数组，长度等于列表的长度，然后逆序遍历列表（正序遍历数组），放入到数组中。

时间复杂度O(n)，空间复杂度O(n)

~~~java
class Solution {
    public int[] reversePrint(ListNode head) {
        List<Integer> list = new ArrayList<>();
        ListNode current = head;
        while(current!=null){
            list.add(current.val);
            current = current.next;
        }
        int size = list.size();
        int[] result = new int[size];
        for(int i=0;i<size;i++){
            result[i] = list.get(size-1-i); // 数组和链表的顺序相反
        }
        return result;
    }
}
~~~

### 想法2：两次遍历链表

除了定义数组占用的空间外，不使用其他辅助空间。由于不知道链表的长度，因而也就不知道数组的长度。可以先遍历一遍链表，获取到链表的长度后即可定义数组，然后再次遍历链表，将值逆序存放到数组中。

时间复杂度O(n)，空间复杂度O(n)

~~~java
class Solution {
    public int[] reversePrint(ListNode head) {
        int len = 0;
        ListNode current = head;
        while(current!=null){
            len++;
            current = current.next;
        }
        int[] result = new int[len];
        current = head;
        while(current!=null){
            result[--len] = current.val;
            current = current.next;
        }
        return result;
    }
}
~~~

