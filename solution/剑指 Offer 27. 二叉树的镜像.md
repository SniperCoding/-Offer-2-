# [剑指 Offer 27. 二叉树的镜像](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)

## 题目描述

>请完成一个函数，输入一个二叉树，该函数输出它的镜像。
>
>例如输入：
>
>~~~
>		4  
>	   /  \ 
>	  2    7 
>	 / \  / \
>	1  3  6  9
>~~~
>
>镜像输出：
>
>```
>		4  
>	   /  \ 
>	  7    2 
>	 / \  / \
>	9  6  3  1
>```
>
>**示例 1：**
>
>```
>输入：root = [4,2,7,1,3,6,9]
>输出：[4,7,2,9,6,3,1]
>```
>
>**限制：**
>
>```
>0 <= 节点个数 <= 1000
>```

### 想法1：DFS

二叉树的镜像实际上就是将二叉树的所有节点的左右子节点交换了一下位置，因此我们可以通过递归遍历所有节点，并将其左右子节点交换即可。

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
    public TreeNode mirrorTree(TreeNode root) {
        dfs(root);
        return root;
    }

    public void dfs(TreeNode node){
        if(node==null){
            return;
        }
        // 交换左右子树
        TreeNode temp = node.left;
        node.left = node.right;
        node.right = temp;
        // 递归翻转左子树和右子树
        dfs(node.left);
        dfs(node.right);
    }
}
~~~

### 想法2：BFS

除了使用 DFS 遍历节点外，另外一种就是使用 BFS 对数进行层序遍历，然后同 DFS 一致，对于遍历的每一个节点，都将其左右子节点进行交换即可。

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
    public TreeNode mirrorTree(TreeNode root) {
        if(root==null){
            return root;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while(!queue.isEmpty()){
            int size = queue.size();
            for(int i=0;i<size;i++){
                TreeNode node = queue.remove();
                // 交换节点的左右节点
                TreeNode temp = node.left;
                node.left = node.right;
                node.right = temp;
                // 将左右节点加入到队列中
                if(node.left!=null){
                    queue.add(node.left);
                }
                if(node.right!=null){
                    queue.add(node.right);
                }
            }
        }
        return root;
    }
}
~~~













