# [剑指 Offer 28. 对称的二叉树](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)

## 题目描述

>请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。
>
>例如，二叉树 [1,2,2,3,4,4,3] 是对称的
>
>~~~
>		1  
>	   /  \ 
>	  2    2 
>	 / \  / \
>	3  4  4  3
>~~~
>
>但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的：
>
>```
>		1  
>	   /  \ 
>	  2    2 
>	   \    \
>	    3    3
>```
>
>**示例 1：**
>
>```
>输入：root = [1,2,2,3,4,4,3]
>输出：true
>```
>
>**示例 2：**
>
>```
>输入：root = [1,2,2,null,3,null,3]
>输出：false
>```
>
>**限制：**
>
>```
>0 <= 节点个数 <= 1000
>```

### 想法1：DFS

我们可以通过递归遍历所有节点，判断其左子树的左/右节点是否等于其右子树的右/左节点。

时间复杂度O( n )，空间复杂度O( n ), 其中 n 是树的节点数量，因为递归会有栈的开销，所以时间复杂度为O(n)

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
    public boolean isSymmetric(TreeNode root) {
        if(root==null){
            return true;
        }
        return dfs(root.left,root.right);
    }

    public boolean dfs(TreeNode node1,TreeNode node2){
        if(node1==null && node2==null){
            return true;
        }
        // 一个为空另一个不为空或值不一样
        if((node1==null && node2!=null)  || (node1!=null && node2==null) || node1.val !=node2.val){
            return false;
        }
        // 查看其左右子树是否是对称的
        return dfs(node1.left,node2.right)  && dfs(node1.right,node2.left);
    }
}
~~~

### 想法2：BFS

除了使用 DFS 遍历节点外，另外一种就是使用 BFS 对数进行层序遍历，然后同 DFS 一致，对于遍历的每一个节点，判断其左子树的左/右节点是否等于其右子树的右/左节点。

时间复杂度O( n )，空间复杂度O( n ), 其中 n 是树的节点数量

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
    public boolean isSymmetric(TreeNode root) {
        if(root==null){
            return true;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        queue.add(root);
        while(!queue.isEmpty()){
            int size = queue.size();
            for(int i=0;i<size/2;i++){
                TreeNode node1 = queue.remove();
                TreeNode node2 = queue.remove();
                if(node1.val!=node2.val){
                    return false;
                }
                // 放node1的左和node2的右到队列中
                if((node1.left==null && node2.right!=null) || (node1.left!=null && node2.right==null)){
                    return false;
                }else if(node1.left!=null && node2.right!=null){
                    queue.add(node1.left);
                    queue.add(node2.right);
                }
                // 放node1的右和node2的左到队列中
                if((node1.right==null && node2.left!=null) || (node1.right!=null && node2.left==null)){
                    return false;
                }else if(node1.right!=null && node2.left!=null){
                    queue.add(node1.right);
                    queue.add(node2.left);
                }
            }
        }
        return true;
    }
}
~~~













