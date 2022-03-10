# [剑指 Offer 10- II. 青蛙跳台阶问题](https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/)

## 题目描述

>一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 n 级的台阶总共有多少种跳法。
>
>答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。
>
>示例 1：
>
>~~~
>输入：n = 2
>输出：2
>~~~
>
>示例 2：
>
>~~~
>输入：n = 7
>输出：21
>~~~
>
>示例 3：
>
>~~~
>输入：n = 0
>输出：1
>~~~
>
>提示：
>
>~~~
>0 <= n <= 100
>~~~

## 解法

### 想法：动态规划

本题和 [剑指 Offer 10- I. 斐波那契数列](https://github.com/SniperCoding/The_sword_refers_to_offer/blob/main/solution/%E5%89%91%E6%8C%87%20Offer%2010-%20I.%20%E6%96%90%E6%B3%A2%E9%82%A3%E5%A5%91%E6%95%B0%E5%88%97.md) 解题思想一致，状态方程也完全一致，小青蛙一次只能跳 1 级台阶或 2 级台阶，那么它跳到第 n 阶楼梯时，一定是从第 n-1 阶楼梯或者第 n-2 阶楼梯跳上来的，那么有状态转移方程 F(n) = F(n - 1) + F(n - 2) ，其中 F(n) 表示跳到第 n 阶楼梯的总跳法。

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
        // 3.状态转移方程
        for(int i=2;i<=n;i++){
            dp[i] = (dp[i-1] + dp[i-2]) % 1000000007;
        }
        // 4.返回最终
        return dp[n];
    }
}
~~~

由递推公式 dp[i] = dp[i-1] + dp[i-2] 值求 <u>dp[i] 只与 dp[i-1] 和 dp[i-2] 这两个值有关</u>，那么我们可以使用三个变量 first、second 和 next  分别表示  dp[i] 、 dp[i - 1] 和dp[i - 2]，则有 result = first + second，之后往后遍历（即 i++）时，first 的值应该更新为 second，而 second 的值更新为 result 即可。

时间复杂度 O(n)，空间复杂度 O(1)。

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

