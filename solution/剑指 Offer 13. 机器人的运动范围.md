# [剑指 Offer 13. 机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)

## 题目描述

>地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？
>
>示例 1：
>
>~~~
>输入：m = 2, n = 3, k = 1
>输出：3
>~~~
>
>示例 2：
>
>~~~
>输入：m = 3, n = 1, k = 0
>输出：1
>~~~
>
>提示：
>
>~~~
>1 <= n,m <= 100
>0 <= k <= 20
>~~~

## 解法

### 想法：深度优先搜索

我们可以通过暴力法对二维矩阵进行遍历，这里采用深度优先搜索的思想，让机器人从(0,0)点不断地往下或往右进行遍历，同时使用一个辅助数组用于记录某个方格是否已被遍历过，如果已被遍历过或者当前方格的行坐标和列坐标的数位之和大于k，则无需再进行遍历，如果没有，则继续往右和往下进行遍历。

时间复杂度O(mn)，空间复杂度O(mn)

~~~java
class Solution {

    public int movingCount(int m, int n, int k) {
        // 新建辅助数组, 如果机器人可以到达某个单元格，则将其方格位置设置为true【也可以用HashMap】
        boolean[][] repeat = new boolean[m][n];
        // 移动机器人
        return movingCount(repeat,0,0,k,0);
    }
	
    /**
    	不断递归向下和向右遍历方格
    */
    public int  movingCount(boolean[][] repeat, int m, int n, int k,int result){
        if(m<0 || n<0 || m>=repeat.length || n>=repeat[0].length || repeat[m][n] || sumBits(m)+sumBits(n)>k){
            return result;
        }
        // 设置标志位
        repeat[m][n] = true;
        result++;
        // 向下或向右移动
        result = movingCount(repeat,m+1,n,k,result);
        result = movingCount(repeat,m,n+1,k,result);
        return result;
    }

    /**
        计算一个数的数位之和
     */
    public int sumBits(int num){
        int sum= 0;
        while(num!=0){
            sum += num %10;
            num /= 10;
        }
        return sum;
    }
}
~~~

