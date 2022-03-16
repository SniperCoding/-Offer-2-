# [剑指 Offer 17. 打印从1到最大的n位数](https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/)

## 题目描述

>输入数字 `n`，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。
>
>**示例 1:**
>
>```
>输入: n = 1
>输出: [1,2,3,4,5,6,7,8,9]
>```
>
>说明：
>
>- 用返回一个整数列表来代替打印
>- n 为正整数

## 解法

### 想法：求出最后一个数

本题较为简单，因为题目要返回的int[]，因此要返回的数均在 int 范围内，就无须考虑大数的问题，要打印的最后一个数（也是最大的那个数）与位数 n 的关系为`max = 10^n -1`

时间复杂度O( 10^n )，空间复杂度O( 10^n )

~~~java
class Solution {
    public int[] printNumbers(int n) {
        int max = Integer.MAX_VALUE;
        if(n <= 9){
           max = (int) Math.pow(10,n) -1;  
        }
        int[] result = new int[max];
        for(int i=0;i< max;i++){
            result[i] = i+1;
        }
        return result;
    }
}
~~~







