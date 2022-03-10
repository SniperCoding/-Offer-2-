# [剑指 Offer 10- I. 斐波那契数列](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)

## 题目描述

>写一个函数，输入 `n` ，求斐波那契（Fibonacci）数列的第 `n` 项（即 `F(N)`）。斐波那契数列的定义如下：
>
>```
>F(0) = 0,   F(1) = 1
>F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
>```
>
>斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。
>
>答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。
>
>**示例 1：**
>
>```
>输入：n = 2
>输出：1
>```
>
>**示例 2：**
>
>```
>输入：n = 5
>输出：5
>```
>
>**提示：**
>
>- `0 <= n <= 100`

## 解法

### 想法：动态规划

首先定义一个备忘录dp，数组中存储 n+1 个数，其中 dp[i] 表示 F(i)， 根据 F(N) = F(N - 1) + F(N - 2) 递推公式知  `dp[i] = dp[i-1] + dp[i-2] ` ，分别计算出 dp[2]、dp[3]、dp[4]...dp[n]，最终 返回 dp[n] 即为要求的F(n)

时间复杂度O(n)，空间复杂度O(n)。

~~~java
class Solution {
    public int fib(int n) {
        if(n==0){
            return 0;
        }
        // 动态规划 F(N) = F(N - 1) + F(N - 2)
        // 1.定义备忘录
        int[] dp = new int[n+1];
        // 2. 定义初始条件
        dp[0] = 0;
        dp[1] = 1;
        for(int i=2;i<=n;i++){
            dp[i] = (dp[i-1] + dp[i-2]) % 1000000007;
        }
        return dp[n];
    }
}
~~~

由递推公式 dp[i] = dp[i-1] + dp[i-2] 值求 <u>dp[i] 只与 dp[i-1] 和 dp[i-2] 这两个值有关</u>，那么我们可以使用三个变量 first、second 和 next  分别表示  dp[i] 、 dp[i - 1] 和dp[i - 2]，则有 result = first + second，之后往后遍历（即 i++）时，first 的值应该更新为 second，而 second 的值更新为 result 即可。

- 时间复杂度 O(n)，空间复杂度 O(1)。

~~~java
class Solution {
    public int fib(int n) {
        if(n==0){
            return 0;
        }
        // 优化空间复杂度
        int first = 0;  //  F(N - 2)
        int second = 1; //  F(N - 1)
        int result = first + second;  // F(N)
        for(int i=2;i<=n;i++){
            result = (first + second) % 1000000007;
            // 更新下一轮
            first = second; 
            second = result;
        }
        return second;
    }
}
~~~

