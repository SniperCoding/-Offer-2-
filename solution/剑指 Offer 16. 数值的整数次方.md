# [剑指 Offer 16. 数值的整数次方](https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/)

## 题目描述

>实现 [pow(*x*, *n*)](https://www.cplusplus.com/reference/valarray/pow/) ，即计算 x 的 n 次幂函数（即，x^n）。不得使用库函数，同时不需要考虑大数问题。
>
>**示例 1：**
>
>```
>输入：x = 2.00000, n = 10
>输出：1024.00000
>```
>
>**示例 2：**
>
>```
>输入：x = 2.10000, n = 3
>输出：9.26100
>```
>
>**示例 3：**
>
>```
>输入：x = 2.00000, n = -2
>输出：0.25000
>解释：2-2 = 1/22 = 1/4 = 0.25
>```
>
>**提示：**
>
>- `-100.0 < x < 100.0`
>- `-2^31 <= n <= 2^31-1`
>- `-10^4 <= x^n <= 10^4`

## 解法

### 想法：二分法（递归）

对于本题而言，最简单的方法就是直接将 x 连乘 n 次，时间复杂度O(n)，然而在 LeetCode 中会执行超时，n 的取值范围是`-2^31 <= n <= 2^31-1`。

为了降低时间复杂度，我们可以考虑以下规律：

- 当 n 为偶数时，` x^n = x^(n/2) * x^(n/2) `
-  当 n 为奇数时，` x^n = x^(n/2) * x^(n/2) * x `

因此我们再求 `x^n` 时，只需要知道 `x^(n/2)`即可，直接可以节省一半时间复杂度，比如我们要知道 x^20，那么我们只需要求出 x^10即可，而要知道 x^10，只需要知道 x^5 即可，一次类推，因此我们可以采取二分法的思想进行递归求解。

由于n可以为正数也可以为负数，为了方便起见，我们可以利用`x^n = (1/x)^-n`这个转换公式将n变为正数，方便求解。需要另 n = -n; x = 1/x; 即可，然而这种变换存在一个问题，即当 n 等于-2^31，就不能这样进行变换了，我们可以对 n = -2^31 进行额外处理，将其变成 `myPow(x, n+1) * 1/x`！

时间复杂度O(logn)，空间复杂度O(logn)

~~~java
class Solution {
    public double myPow(double x, int n) {
        // 终止条件
        if(n==0){
            return 1.0; 
        }
        // 特殊情况
        if(n==Integer.MIN_VALUE){
            return myPow(x,n+1) * 1/x;
        }
        // 为了方便，当n小于0时，对n和x进行一下转换， 转换公式为：x^n = (1/x)^-n
        if(n < 0){
            n = -n;
            x = 1/x;
        }
        // 递归
        double half = myPow(x,n/2);
        double result = half*half;
        return n%2==0?result:result*x;  // 如果n是奇数，则需要再乘以一个
    }
}
~~~

### 想法：二分法（迭代）

可以用迭代来代替递归，进一步优化空间复杂度。

上面我们发现了以下规律：

- 当 n 为偶数时，` x^n = x^(n/2) * x^(n/2) `
-  当 n 为奇数时，` x^n = x^(n/2) * x^(n/2) * x `

因此在求 x ^ n 时我们需要知道 x^(n/2)，于是想到了递归的方法，而我们可以将上面规律进行变形：

- 当 n 为偶数时，` x^n = x^(n/2) * x^(n/2)= (x^2)^(n/2) ` 
-  当 n 为奇数时，` x^n = x^(n/2) * x^(n/2) * x = (x^2)^(n/2) * x`

在求 x^n 时，我们实际只需要求 (x^2^)^(n/2)^，因此我们可以另 x = x * x，n = n/2；，如果 n 是奇数，就先乘以 x。

![](https://cdn.jsdelivr.net/gh/SniperCoding/pictures1/20220316155610.png)

时间复杂度O(logn)，空间复杂度O(1)

~~~java
class Solution {
    public double myPow(double x, int n) {
        if(n==Integer.MIN_VALUE){
            return myPow(x,n+1) * 1/x;
        }
        // 为了方便，当n小于0时，对n和x进行一下转换， 转换公式为：x^n = (1/x)^-n
        if(n < 0){
            n = -n;
            x = 1/x;
        }
        // 迭代法
        double result = 1.0;
        while(n!=0){
            if(n%2!=0){  // 一旦为奇数，就需要乘一个x
                result *= x;
            }
            x = x * x;
            n = n >> 1;  // 比 n=n/2 效率高
        }
        return result;

    }
}
~~~



