# [剑指 Offer 30. 包含min函数的栈](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/)

## 题目描述

>定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。
>
>**示例:**
>
>```
>MinStack minStack = new MinStack();
>minStack.push(-2);
>minStack.push(0);
>minStack.push(-3);
>minStack.min();   --> 返回 -3.
>minStack.pop();
>minStack.top();   --> 返回 0.
>minStack.min();   --> 返回 -2.
>```
>
>**提示：**
>
>1. 各函数的调用总次数不超过 20000 次

### 想法：单调栈

定义两个栈，一个栈用于存放所有值，一个栈用于存放最小值（单调栈）

时间复杂度O( 1 )，空间复杂度O( 1 ）

~~~java
class MinStack {

    // 单调栈
    Deque<Integer> stack1;  // 用于存放所有值
    Deque<Integer> stack2;   // 顶端用于存放最小值，单调栈

    /** initialize your data structure here. */
    public MinStack() {
        stack1 = new LinkedList<>();
        stack2 = new LinkedList<>();
    }
    
    public void push(int x) {
        stack1.push(x);
        if(stack2.isEmpty() || stack2.peek() >= x){
            stack2.push(x);
        }
    }
    
    public void pop() {
        int temp = stack1.pop();
        if(temp == stack2.peek()){
            stack2.pop();
        }
    }
    
    public int top() {
        return stack1.peek();
    }
    
    public int min() {
        return stack2.peek();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.min();
 */
~~~













