# [剑指 Offer 12. 矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)

## 题目描述

>给定一个 `m x n` 二维字符网格 `board` 和一个字符串单词 `word` 。如果 `word` 存在于网格中，返回 `true` ；否则，返回 `false` 。
>
>单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。
>
>例如，在下面的 3×4 的矩阵中包含单词 "ABCCED"（单词中的字母已标出）。
>
>![](https://cdn.jsdelivr.net/gh/SniperCoding/pictures1/20220313204147.jpg)
>
>**示例 1：**
>
>```
>输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
>输出：true
>```
>
>**示例 2：**
>
>```
>输入：board = [["a","b"],["c","d"]], word = "abcd"
>输出：false
>```
>
>**提示：**
>
>- `1 <= board.length <= 200`
>- `1 <= board[i].length <= 200`
>- `board` 和 `word` 仅由大小写英文字母组成

## 解法

### 想法：递归 + 剪枝

本题要在二维数组中查找一个字符串是否存在，且要求路径连续，并且同一单元格的字母不允许重复使用。很多小伙伴的第一想法应该就是暴力解法。

首先遍历二维数组 board（双重 for 循环），然后判断以 `board[i][j]`为起点，是否能找到一个连续的路径，使得此路径上的所有字符拼接后等于指定字符串，我们通过定义一个方法作此判断，方法需要传入路径的当前位置（i和j)，以及遍历到指定字符串的哪个位置了（index）。

- 如果当前位置的字符 `board[i][j]` 等于字符串索引位置的值`word.charAt(index)`，则递归调用本方法四次，参数分别为当前位置的上/下/左/右位置以及字符串的下一个索引 index+1，注意这四个方法只要有一个返回  true，最终结果就返回 true
- 如果当前位置的字符 `board[i][j]` 不等于字符串索引位置的值`word.charAt(index)`，则进行剪枝，返回 false 即可。

难点在于如何判断同一单元格的字母是否被重复使用过，我们可以额外定义一个布尔型二维数组，专门用于判断单元格的字母是否被使用过，一旦被使用过，将指定位置设置 true。

时间复杂度O(mn * 3^k)，空间复杂度O(mn)，其中m，n是矩阵的行数和列数，k为字符串长度。

对于 mn个起点，在搜索中每个字符有上、下、左、右四个方向可以选择，舍弃回头（上个字符）的方向，剩下 3种选择，因此方案数的复杂度为 O(mn * 3^k)。

~~~java
class Solution {
    String word;
    char[][] board;
    boolean[][] repeat ; // 为了记录重复占用问题

    public boolean exist(char[][] board, String word) {
        // 特殊情况
        if(board.length==0 || board[0].length==0 || word.length()>board.length * board[0].length){
            return false;
        }
        this.word = word;
        this.board = board;
        this.repeat = new boolean[board.length][board[0].length];

        for(int i=0;i<board.length;i++){
            for(int j=0;j<board[0].length;j++){
                if(exist(0,i,j)){
                    return true;
                }
            }
        }
        return false;
    }

    /**
        index : 当前遍历到的单词索引
        i:二维中的行
        j:二维中的列
     */
    public boolean exist(int index,int i,int j){
        // 如果当前位置超出数组范围/字母不等于单词中字母/已被使用过，则直接返回false
        if(i<0 ||  j<0 ||  i>=board.length ||  j>=board[0].length || word.charAt(index)!=board[i][j]  || repeat[i][j]){
            return false;
        }
        // 索引走到单词最后，返回true
        if(index == word.length()-1){
             return true;
        }
        // 记录已被占用的索引
        repeat[i][j] = true;
        // 索引没走到单词最后，判断当前位置的上下左右
        boolean result = exist(index+1,i-1,j) || exist(index+1,i+1,j) || exist(index+1,i,j-1) || exist(index+1,i,j+1);
        // 解除已被占用的索引
        repeat[i][j] = false;
        return result;
    }
}
~~~

注意在本题中可以对空间复杂度进行优化：

我们为了记录单元格是否被重复占用，重新定义了一个二维数组，但是由于题目中说明矩阵中元素仅由大小写英文字母组成，因此我们可以**直接使用原始矩阵进行重复占用的判断**，一旦矩阵某个单元格被占用后，我们可以将其元素设置为'0'，之后判断是否被重复占用，只需要判断元素是否等于'0'即可。

