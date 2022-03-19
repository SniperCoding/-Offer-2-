# [剑指 Offer 26. 树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)

## 题目描述

>输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)
>
>B是A的子结构， 即 A中有出现和B相同的结构和节点值。
>
>例如:
>给定的树 A:
>
>~~~sh
>	 3  
>	/ \  
>   4   5 
>  / \ 
> 1   2
>~~~
>
>给定的树 B：
>
>~~~sh
>  4
> /
>1
>~~~
>
>返回 true，因为 B 与 A 的一个子树拥有相同的结构和节点值。
>
>**示例 1：**
>
>```
>输入：A = [1,2,3], B = [3,1]
>输出：false
>```
>
>**示例 2：**
>
>```
>输入：A = [3,4,5,1,2], B = [4,1]
>输出：true
>```
>
>**限制：**
>
>```
>0 <= 节点个数 <= 10000
>```

### 想法：递归

我们对 A 树进行递归遍历，一旦发现某个节点值等于B的节点值，那么就判断B树是不是以当前节点为根的树的子结构，判断标准为：

- B的左子树是以当前节点为根的树的左子树的子结构（要求子树根节点数值相同）
- B的右子树是以当前节点为根的树的右子树的子结构（要求子树根节点数值相同）

时间复杂度O( mn )，空间复杂度O( m ), 其中 m,n 分别是树A和树B的节点数量

~~~java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isSubStructure(TreeNode A, TreeNode B) {
        if(A==null || B==null){
            return false;
        }
        // 遍历A,找到值和B的根节点相同的节点
        if(A.val==B.val){
            if(dfs(A,B)){
                return true;
            }
        }
        return isSubStructure(A.left,B) || isSubStructure(A.right,B);
    }

    // A 和 B的根节点相同，判断B是不是A的子结构
    public boolean dfs(TreeNode A, TreeNode B){
        if(A==B){
            return true;
        }

        boolean leftFlag = true;
        boolean rightFlag = true;

        // 判断B的左子树是不是A的左子树的子结构
        if(B.left!=null){
            if(A.left==null  || B.left.val!=A.left.val){
                return false;
            }
            leftFlag = dfs(A.left,B.left);
        }

        // 判断B的右子树是不是A的左子树的子结构
        if(B.right!=null){
            if(A.right==null  || B.right.val!=A.right.val){
                return false;
            }
            rightFlag = dfs(A.right,B.right);
        }
        return leftFlag && rightFlag;

    }
}
~~~













