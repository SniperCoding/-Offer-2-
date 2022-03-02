# [剑指 Offer 04. 二维数组中的查找](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

## 题目描述

>在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个高效的函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
>
>**示例:**
>
>现有矩阵 matrix 如下：
>
>```
>[
>  [1,   4,  7, 11, 15],
>  [2,   5,  8, 12, 19],
>  [3,   6,  9, 16, 22],
>  [10, 13, 14, 17, 24],
>  [18, 21, 23, 26, 30]
>]
>```
>
>给定 target = `5`，返回 `true`。
>
>给定 target = `20`，返回 `false`。
>
>**限制：**
>
>```
>0 <= n <= 1000
>0 <= m <= 1000
>```

## 解法

### 想法1：暴力解法，双重for循环

遍历二维数组的所有元素，判断目标值是否在二维数组中存在。

时间复杂度O(mn)，空间复杂度O(1)

~~~java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        if(matrix.length==0 || matrix[0].length==0){
            return false;
        }
        int n = matrix.length;
        int m = matrix[0].length;
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(matrix[i][j]==target){
                    return true;
                }
            }
        }
    }
}
~~~

### 想法2：线性查找

由于数组中每一行每一列均是有序的，我们从右上角元素开始遍历（将二维矩阵逆时针旋转45°，类似于一颗二叉搜索树），

- 如果当前值小于 target，那么说明当前前面整行的值均小于 target，因此直接可以舍弃此行，将元素往下移动一格；
- 如果当前值大于 target，那么说明当前下面整列的值均大于 target，因此可以舍弃此列，将元素往左移动一格；
- 如果当前值等于 target，那么直接返回true即可。
- 下图展示了当 target=20 时，元素的遍历轨迹。

<img src="https://cdn.jsdelivr.net/gh/SniperCoding/pictures1/20220302233635.png" alt="image-20220302233607447" style="zoom: 80%;" />

时间复杂度O(m + n)，空间复杂度O(1)

~~~java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        if(matrix.length==0 || matrix[0].length==0){
            return false;
        }
        int n = matrix.length;
        int m = matrix[0].length;
        int row = 0;
        int col = m-1;
        while(row<n && col>=0){
            if(matrix[row][col]==target){	// 存在，返回true
                return true;
            }else if(matrix[row][col]>target){  //值比target大，那么此列中所有值均比target大，因而可以往前移动一列
                col--;
            }else{  // 值比target小，那么此行中所有值均比target大，因为可以往下移动一列
                row++;
            }
        }
        return false;
}
~~~



