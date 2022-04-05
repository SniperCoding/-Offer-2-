# [剑指 Offer 29. 顺时针打印矩阵](https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)

## 题目描述

>输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。
>
>**示例 1：**
>
>```
>输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
>输出：[1,2,3,6,9,8,7,4,5]
>```
>
>**示例 2：**
>
>```
>输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
>输出：[1,2,3,4,8,12,11,10,9,5,6,7]
>```
>
>**限制：**
>
>- `0 <= matrix.length <= 100`
>- `0 <= matrix[i].length <= 100`

### 想法：模拟

时间复杂度O( mn )，空间复杂度O( 1 ）

~~~java
class Solution {
    public int[] spiralOrder(int[][] matrix) {
        if(matrix.length==0 || matrix[0].length==0){
           return new int[0];
        }
        int m = matrix.length;
        int n = matrix[0].length;
    
        int[] result = new int[m*n];
        int k = 0;

        // 一共需要遍历 (min(m,n)+1)/2 词
        int num = (Math.min(m,n)+1) / 2;
        int i=0,j=0;
        while(num-- > 0){
            // ① 先遍历 (i,j) - (i,n-1-j)，即从左到右
            for(int x=j; x<n-j; x++){
                result[k++] = matrix[i][x];
            }

            // ② 再遍历 (i+1,n-1-j) - (m-1-i,n-1-j)，即从上到下
            if(i+1>=m-i){
                break;
            }
            for(int x=i+1; x< m-i; x++){
                result[k++] = matrix[x][n-1-j];
            }

            // ③ 再遍历 (m-1-i,n-2-j) - (m-1-i,j)，即从右到左
            if(n-2-j<j){
                break;
            }
            for(int x= n-2-j; x>=j;x--){
                result[k++] = matrix[m-1-i][x];
            }
            // ④ 最后遍历 (m-2-i,j) - (i+1,j)，即从下到上
            if(m-2-i<i+1){
                break;
            }
            for(int x=m-2-i; x>=i+1;x--){
                result[k++] = matrix[x][j];
            }

            // 更新下一轮的起点
            i++;
            j++;
        }
        return result;

    }
}
~~~













