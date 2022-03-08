# [剑指 Offer 09. 用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

## 题目描述

>用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 `appendTail` 和 `deleteHead` ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，`deleteHead` 操作返回 -1 )
>
>**示例 1：**
>
>```
>输入：
>["CQueue","appendTail","deleteHead","deleteHead"]
>[[],[3],[],[]]
>输出：[null,null,3,-1]
>```
>
>**示例 2：**
>
>```
>输入：
>["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
>[[],[],[5],[2],[],[]]
>输出：[null,-1,null,null,5,2]
>```
>
>**提示：**
>
>- `1 <= values <= 10000`
>- `最多会对 appendTail、deleteHead 进行 10000 次调用`

## 解法

### 想法：一个栈负责解决入队，一个栈负责解决出队

栈：`后进先出`

队列：`先进先出`

注意栈的特性为后进先出，如果将数据插入到栈1中，然后再分别出栈，将出栈结果插入到栈2中，此时再对栈2进行出栈操作，会使得出栈的顺序与数据最初插入到栈1的顺序一致。因而就能实现队列的效果，先进先出！！

<img src="https://cdn.jsdelivr.net/gh/SniperCoding/pictures1/20220308220954.png" style="zoom:50%;" />

思想类似于`负负得正`，后进先出 + 后进先出 = 先进先出。

- 对于 `appendTail `方法，将所有数值插入到 stack1 中。
- 对于 `deleteHead`方法，需要分三种情况
  - 若两个栈均为空，则返回-1
  - 若 stack2 不为空，则只需要将 stack2 的第一个数出栈即可。
  - 若 stack2 为空，stack1 不为空，那么将 stack1 中的所有数据出栈并插入到 stack2 中，之后将 stack2 的第一个数出栈即可。

- 注意，对于任一个操作数而言，它会入栈两次和出栈两次（分别是栈1和栈2），因而总体时间复杂度O(n)，空间复杂度O(n)。

~~~java
class CQueue {

    Deque<Integer> stack1;  // 负责解决入队
    Deque<Integer> stack2;  // 负责解决出队
    public CQueue() {
        stack1 = new LinkedList<>();
        stack2 = new LinkedList<>();
    }
    
    public void appendTail(int value) {
        stack1.push(value);
    }
    
    public int deleteHead() {
        // 1. stack2不为空
        if(!stack2.isEmpty()){
            return stack2.pop();
        }
        // 2. stack2为空，但stack1不为空，将stack1中的所有值转移到stack2中
        if(!stack1.isEmpty()){
             while(!stack1.isEmpty()){
                stack2.push(stack1.pop());
            }
            return stack2.pop();
        }
        // 3. 二者都为空
        return -1;

    }
}
~~~

